---
title: "Claude Code on the Webの仕様を徹底解剖"
emoji: "🔬"
type: "tech"
topics: [claudecode, sandbox, ai, anthropic]
published: true
published_at: "2025-11-01 18:01"
---

:::message
- 2025年11月1日時点のClaude Code on the Web環境の調査結果です
- 記事執筆時点でのClaude Code on the WebのClaude Codeのバージョンは2.0.25でした
- すでにSandboxの挙動が変わっているのを確認しています。実際の挙動は手元の環境で試してみてください
:::

Oikonです。普段はAIツール、特にClaude Codeで遊んでいます。

Claude Codeには**Claude Code on the Web**という、ブラウザ上でClaude Codeを実行できる環境があります。手元にPCがなくてもブラウザさえあれば開発できます。

https://docs.claude.com/en/docs/claude-code/claude-code-on-the-web

先日、[aimeetup](https://aimeetup.connpass.com/event/373363/)というイベントに顔を出した際に「Claude Code on the Webを使って何か作ってくる」というお題をいただいていたため、Claude Code on the Webの仕様を調べようと思いました。

Sandbox環境の制限を理解したい方や、Claude Code on the Webを使いこなしたい方の参考になれば幸いです。

## Claude Code on the Webとは

Claude Code on the WebはAnthropicが提供する、**ブラウザ上でClaude Codeを実行できる環境**です。

https://claude.com/code

ローカルのClaude Codeと違い、ブラウザさえあればどこからでも使えるのが最大の魅力です。スマートフォンのモバイルアプリからも起動できます。

基本的な使い方は以下の記事にすでに書かれています:

https://zenn.dev/beagle/articles/bc6ef88dd68615

## Sandbox環境

Claude Code on the Webは**gVisorベースのコンテナ環境**で実行されています。[gVisor](https://gvisor.dev/)はGoogleが開発したアプリケーションカーネルで、コンテナのセキュリティを強化するSandbox技術です。

### 基本スペック

| 項目 | 仕様 |
|-----|------|
| **OS** | Ubuntu 24.04.3 LTS |
| **コンテナランタイム** | gVisor (runsc) |
| **CPU** | 4コア |
| **メモリ** | 8GB上限 |
| **ディスク** | 9.8GB |
| **ファイルシステム** | 9p protocol |
| **ネットワーク** | プロキシ経由（JWT認証） |

## Claude Code on the Webの仕組み

![overview](/images/cc-web-sandbox/overview.png)

Claude Code on the WebはgVisorベースのコンテナ環境にGitHubリポジトリをCloneしてClaude Codeを実行しています。以下に流れを紹介します。

1. Claude Code on the Webを起動
2. リポジトリを選択してチャット開始
3. Sandbox環境を起動
4. Sandboxの`/home/user/`にリポジトリをClone
5. git repository内でClaude Codeを起動
6. 起動したClaude Code内でセッション開始
7. セッションの内容をClaude Code on the Webにフィードバック
8. 以降セッションを続行

つまり、**Claude Code on the WebはLinux環境にgit repositoryをcloneしてきてclaude codeを起動している技術**です。この前提を理解することで、Sandbox環境内でできることも見えてきます。

## Claude Code on the Webの仕様

Claude Code on the Webはgit repositoryをcloneしているため、自分が持ち込める設定はプロジェクト設定のみとなります。つまり`~/.claude/`のグローバル設定は持ち込めませんが、`./.claude/`のプロジェクト設定は持ち込めます。

### Claude Code on the Webのグローバル設定

Claude Code on the WebではSandbox内の`~/.claude/settings.json`にClaude Codeのためのグローバル設定があります。

中身は以下の通りです。

```json:settings.json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/stop-hook-git-check.sh"
          }
        ]
      }
    ]
  }
}
```

上記を見て分かる通り、Stop Hooksのみが設定されています。`stop-hook-git-check.sh`の中身が気になる方用にこちらに添付しておきます。

:::details　stop-hook-git-check.sh

```sh
#!/bin/bash

# Read the JSON input from stdin
input=$(cat)

# Check if stop hook is already active (recursion prevention)
stop_hook_active=$(echo "$input" | jq -r '.stop_hook_active')
if [[ "$stop_hook_active" = "true" ]]; then
  exit 0
fi

# Check if we're in a git repository - bail if not
if ! git rev-parse --git-dir >/dev/null 2>&1; then
  exit 0
fi

# Check for uncommitted changes (both staged and unstaged)
if ! git diff --quiet || ! git diff --cached --quiet; then
  echo "There are uncommitted changes in the repository. Please commit and push these changes to the remote branch." >&2
  exit 2
fi

# Check for untracked files that might be important
untracked_files=$(git ls-files --others --exclude-standard)
if [[ -n "$untracked_files" ]]; then
  echo "There are untracked files in the repository. Please commit and push these changes to the remote branch." >&2
  exit 2
fi

current_branch=$(git branch --show-current)
if [[ -n "$current_branch" ]]; then
  if git rev-parse "origin/$current_branch" >/dev/null 2>&1; then
    # Branch exists on remote - compare against it
    unpushed=$(git rev-list "origin/$current_branch..HEAD" --count 2>/dev/null) || unpushed=0
    if [[ "$unpushed" -gt 0 ]]; then
      echo "There are $unpushed unpushed commit(s) on branch '$current_branch'. Please push these changes to the remote repository." >&2
      exit 2
    fi
  else
    # Branch doesn't exist on remote - compare against default branch
    unpushed=$(git rev-list "origin/HEAD..HEAD" --count 2>/dev/null) || unpushed=0
    if [[ "$unpushed" -gt 0 ]]; then
      echo "Branch '$current_branch' has $unpushed unpushed commit(s) and no remote branch. Please push these changes to the remote repository." >&2
      exit 2
    fi
  fi
fi

exit 0
```
:::

簡単に`stop-hook-git-check.sh`の動作を説明すると以下の流れになります。

1. Hooksがループしないようにチェック
2. Gitリポジトリ確認
3. commit変更のチェック
4. 未追跡ファイルのチェック
5. Commit Pushのチェック

つまり、`リポジトリに差分があったらcommit + pushをClaudeに促す`というものです。Claude Code on the Webが変更を作ってすぐにCommit + PushするのはこのHooksが設定されているからです。

### Claude Codeの起動時オプション

Claude Code on the WebではSandbox内でClaude Codeを起動しています。その際に設定されているClaude Codeの起動時オプションは以下の通りです。

```sh
claude \
  --output-format=stream-json \
  --verbose \
  --replay-user-messages \
  --input-format=stream-json \
  --debug-to-stderr \
  --allowed-tools Task,Bash,Glob,Grep,ExitPlanMode,Read,Edit,MultiEdit,Write,NotebookEdit,WebFetch,TodoWrite,WebSearch,BashOutput,KillBash,Tmux,mcp__codesign__sign_file \
  --disallowed-tools Bash(gh:*) \
  --append-system-prompt "You are Claude, ..." \
  --model claude-sonnet-4-5-20250929 \
  --add-dir /home/user/repo_name \
  --sdk-url wss://api.anthropic.com/v1/session_ingress/ws/session_id \
  --resume=https://api.anthropic.com/v1/session_ingress/session/session_id \
  --debug
```

`--varbose`オプションが指定されているため、起動時オプションは`grep -A 5 "Executing Claude Code" /tmp/claude-code.log`などをClaudeに実行させることで取得できました。

上記からわかるようにモデルは`Sonnet 4.5`に固定されています。settings.jsonで"model"を指定しても変わりません。

起動時オプション:
https://docs.claude.com/en/docs/claude-code/cli-reference#cli-flags

興味深いのは、`gh`コマンドが禁止ツールに入っていることです。これは`--append-system-prompt`でも厳重に指定されていました。

:::details --append-system-promptの中身

```txt

You are Claude, an AI assistant designed to help with GitHub issues and pull requests. Think carefully as you analyze the context and respond appropriately. Here's the context for your current task:
Your task is to complete the request described in the task description.

Instructions:
1. For questions: Research the codebase and provide a detailed answer
2. For implementations: Make the requested changes, commit, and push

## Git Development Branch Requirements

You are working on the following feature branches:

 **repository-name**: Develop on branch `branch-name`

### Important Instructions:

1. **DEVELOP** all your changes on the designated branch above
2. **COMMIT** your work with clear, descriptive commit messages
3. **PUSH** to the specified branch when your changes are complete
4. **CREATE** the branch locally if it doesn't exist yet
5. **NEVER** push to a different branch without explicit permission

Remember: All development and final pushes should go to the branches specified above.

## Git Operations

Follow these practices for git:

**For git push:**
- Always use git push -u origin <branch-name>
- CRITICAL: the branch should start with 'claude/' and end with matching session id, otherwise push will fail with 403 http code.
- Only if push fails due to network errors retry up to 4 times with exponential backoff (2s, 4s, 8s, 16s)
- Example retry logic: try push, wait 2s if failed, try again, wait 4s if failed, try again, etc.

**For git fetch/pull:**
- Prefer fetching specific branches: git fetch origin <branch-name>
- If network failures occur, retry up to 4 times with exponential backoff (2s, 4s, 8s, 16s)
- For pulls use: git pull origin <branch-name>

The GitHub CLI (`gh`) is not available in this environment. For GitHub issues ask the user to provide the necessary information directly.

```

:::

## Claude Code on the Webで使える機能

Claude Code on the Webは、Sandbox内でClaude Codeを起動しているだけなのですが、ローカル環境のClaude Codeの機能は使えないものもあります。いくつかできることとできないことを紹介します。

### CLAUDE.md

![claudemd](/images/cc-web-sandbox/claudemd.jpeg)

CLAUDE.mdの内容は反映されます。例えば以下のように記述したとします。

```md
## コンテキストウィンドウの報告

**重要**: 各応答の最後に、コンテキストウィンドウの使用状況を簡潔に報告してください。

形式:
/```
---
コンテキスト: 使用済みXXK / 残りXXK (XX%)
/```

例:
/```
---
コンテキスト: 使用済み32K / 残り168K (84%残)
/```
```

このような記述をすると、画像のようにClaude Code on the Webに正しく指示が反映されていることが確認できました。

### Slash Command

![slashcommand](/images/cc-web-sandbox/slashcommand.png)

スラッシュコマンド（カスタムスラッシュコマンド）は**使えません**。`canUseTool`が使用できないためです。

ただし、Claudeも賢いので直接Markdownファイルを読みにいくことで間接的にスラッシュコマンドを実行してくれます。

画像では`run /xyz`というように自然言語での記述をしています。現在`/xyz`と直接コマンドを叩きにいくと、**Claude Code on the Webはレスポンスが返ってこない不具合**があります。

### Hooks

Hooksは**使用できます**。前述のようにClaude Codeのグローバル設定に`stop-hooks-check-git`が設定されていることからも分かります。

### Subagents

![subagent](/images/cc-web-sandbox/subagent.png)

Subagentsは**使えます**。上記の画像ではデフォルトの@agent-Exploreというサブエージェントを並列実行させています。

ローカルのClaude Codeと違う点としてはSubagentsは**Task**という表記で実行され、具体的なSubagent名は表示されません。（Claude Code 1.0.x時代もそうだった）

### Skills

![skills](/images/cc-web-sandbox/skills.png)

Skillsは**使えません**。これも`canUseTool`が使用できない設定のためです。ただSkillsの中身はMarkdownなので、Claudeが勝手に読み取って使ってくれることが多いです。スラッシュコマンドと同じような挙動をすると思って結構です。

### output-style

![output-style](/images/cc-web-sandbox/output-style.png)

output-styleは**使用できます**。まぁそもそもoutput-styleは使用率1%未満ほどの機能らしいので、11月5日Deprecateされるそうですが笑

移行について:
https://docs.claude.com/en/docs/claude-code/output-styles

個人的に好きな機能なので掲載しました。

:::message
2025-11-05 追記

output-styleのdeprecateがコミュニティの意見により撤回されました！output-styleは2.0.33時点で使用可能です

https://x.com/oikon48/status/1985516353651163439

:::

## まとめ

今回は、Claude Code on the WebのSandbox環境について調査しました

Claude Code on the Webの仕様や動きが分かると、できることが見えてくると思います。

## Xフォローしてくれると嬉しいです

Xでも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://docs.claude.com/en/docs/claude-code/claude-code-on-the-web

https://gvisor.dev/docs/

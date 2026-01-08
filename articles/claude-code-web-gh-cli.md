---
title: "Claude Code on the Webでghコマンドを使う"
emoji: "💫"
type: "tech"
topics: [claudecode, github, cli, claude, anthropic]
published: true
---

:::message
Claude Code on the Web の仕様は2026年1月8日時点です。今後`gh`コマンドがデフォルトでインストールされるといくつかの設定が不要になります。
:::

Oikonです。普段はAIツール、特にClaude Codeで遊んでいます。

以前、[Claude Code on the Webの仕様を徹底解剖](https://zenn.dev/oikon/articles/claude-code-web-sandbox)という記事を書きました。その中で`gh`コマンドが禁止されていることを紹介しましたが、最近の仕様変更で`gh`が`disallowed_tools`から削除され、使えるようになりました。

今回は、Claude Code on the Webでghを使えるようにする設定を紹介します。

## Claude Code on the Webとは

Claude Code on the Webは、**ブラウザ上でClaude Codeを実行できる環境**です。

https://claude.com/code

手元にPCがなくてもブラウザさえあれば開発できることが魅力です。GitHubリポジトリをcloneして、Sandbox環境内でClaude Codeを動かす仕組みになっています。詳しい仕様は前回の記事をご覧ください：

https://zenn.dev/oikon/articles/claude-code-web-sandbox

ちなみに余談ですが、Claude Codeのプロンプトの先頭に`&`をつけるとローカルのClaude Codeからon the Webにタスクを送れます。

https://x.com/oikon48/status/1996177293551747577?s=20

## `gh`コマンドの禁止解除

以前のClaude Code on the Webでは、ghコマンドが明確に禁止されていました。起動時のオプションで`--disallowed-tools Bash(gh:*)`が設定されており、システムプロンプトにも以下の記述がありました：

> The GitHub CLI (`gh`) is not available in this environment.

つまり、**PRの作成やissueの操作といったGitHub CLIを使った操作は一切できませんでした**。

しかし、後日(2025年12月17日)で確認したところ、**`disallowed_tools`からghが削除されていました**。

![startup.jsonの差分](https://pbs.twimg.com/media/G-HBEEraIAAxYwp?format=png&name=small)

上記の画像は`data/startup.json`の差分です。`"disallowed_tools": ["Bash(gh:*)"]`が削除されているのがわかります。これにより、**ghコマンド自体は禁止ではなくなりました**。ただし、デフォルトではghはインストールされていないため、そのままでは使えません。

![has gh](/images/claude-code-web-gh-cli/no-gh.png)

## ghを使うための条件

ghを使うには以下の2つが必要です：

1. カスタム環境でGITHUB_TOKENを設定する
2. ghをインストールする

### カスタム環境の設定

Claude Code on the Webでは、カスタム環境を使って環境変数を設定できます。

![カスタム環境の設定画面](/images/claude-code-web-gh-cli/custom-env-settings.png)

カスタム環境で.env形式の`GITHUB_TOKEN`を設定することで、ghコマンドの認証に必要なトークンを渡すことができます。ネットワークアクセスは"Full"ならば特に設定の必要はなし、"カスタム"であればインストール元（今回のScriptの場合は、`release-assets.githubusercontent.com`）を追加します。

### sessionStartHooksでghをインストール

GITHUB_TOKENが設定されている環境で`gh`コマンドをインストールすれば、`gh`が使えます。Claude Codeに明示的に頼むこともできますが、sessionStartHooksを使えば自動でインストールできます。

リポジトリ内に以下のような`.claude/hooks/gh-setup.sh`を作成します:

:::details gh-setup.sh

```bash:gh-setup.sh
#!/bin/bash
# This is sample script, and you should verify your own.
# SessionStart hook: GitHub CLI auto-installation for remote environments
# This script installs gh CLI when running in Claude Code on the Web
# following best practices: idempotent, fail-safe, proper logging

set -e

LOG_PREFIX="[gh-setup]"

log() {
    echo "$LOG_PREFIX $1" >&2
}

# Only run in remote Claude Code environment
if [ "$CLAUDE_CODE_REMOTE" != "true" ]; then
    log "Not a remote session, skipping gh setup"
    exit 0
fi

log "Remote session detected, checking gh CLI..."

# Check if gh is already available
if command -v gh &>/dev/null; then
    log "gh CLI already available: $(gh --version | head -1)"
    exit 0
fi

# Setup local bin directory
LOCAL_BIN="$HOME/.local/bin"
mkdir -p "$LOCAL_BIN"

# Check if gh exists in local bin
if [ -x "$LOCAL_BIN/gh" ]; then
    log "gh found in $LOCAL_BIN"
    # Ensure PATH includes local bin
    if [[ ":$PATH:" != *":$LOCAL_BIN:"* ]]; then
        export PATH="$LOCAL_BIN:$PATH"
        # Persist to CLAUDE_ENV_FILE if available
        if [ -n "$CLAUDE_ENV_FILE" ]; then
            echo "export PATH=\"$LOCAL_BIN:\$PATH\"" >> "$CLAUDE_ENV_FILE"
            log "PATH updated in CLAUDE_ENV_FILE"
        fi
    fi
    exit 0
fi

log "Installing gh CLI to $LOCAL_BIN..."

# Create temp directory for installation
TEMP_DIR=$(mktemp -d)
trap "rm -rf $TEMP_DIR" EXIT

# Detect architecture
ARCH=$(uname -m)
case "$ARCH" in
    x86_64)
        GH_ARCH="amd64"
        ;;
    aarch64|arm64)
        GH_ARCH="arm64"
        ;;
    *)
        log "Unsupported architecture: $ARCH"
        exit 0  # Fail-safe: exit 0 even on failure
        ;;
esac

# Download and install gh CLI
GH_VERSION="2.62.0"
GH_TARBALL="gh_${GH_VERSION}_linux_${GH_ARCH}.tar.gz"
GH_URL="https://github.com/cli/cli/releases/download/v${GH_VERSION}/${GH_TARBALL}"

log "Downloading gh v${GH_VERSION} for ${GH_ARCH}..."

if ! curl -sL "$GH_URL" -o "$TEMP_DIR/$GH_TARBALL"; then
    log "Failed to download gh CLI"
    exit 0  # Fail-safe
fi

log "Extracting..."
if ! tar -xzf "$TEMP_DIR/$GH_TARBALL" -C "$TEMP_DIR"; then
    log "Failed to extract gh CLI"
    exit 0  # Fail-safe
fi

# Move binary to local bin
if ! mv "$TEMP_DIR/gh_${GH_VERSION}_linux_${GH_ARCH}/bin/gh" "$LOCAL_BIN/gh"; then
    log "Failed to install gh CLI"
    exit 0  # Fail-safe
fi

chmod +x "$LOCAL_BIN/gh"

# Update PATH
export PATH="$LOCAL_BIN:$PATH"

# Persist PATH to CLAUDE_ENV_FILE if available
if [ -n "$CLAUDE_ENV_FILE" ]; then
    echo "export PATH=\"$LOCAL_BIN:\$PATH\"" >> "$CLAUDE_ENV_FILE"
    log "PATH persisted to CLAUDE_ENV_FILE"
fi

log "gh CLI installed successfully: $($LOCAL_BIN/gh --version | head -1)"
exit 0
```

:::

`.claude/settings.json`に上記のHooksの設定を追加します：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "./.claude/hooks/gh-setup.sh"
          }
        ]
      }
    ]
  }
}
```


`CLAUDE_CODE_REMOTE`はClaude Code on the Webの環境でのみ`true`に設定される環境変数です。これにより、**ローカル環境では実行をスキップし、on the Web環境でのみghをインストール**します。今回は`gh`コマンドのインストールに使いましたが、他のリモートのみで実行したいケースでも使えると思います。

```bash
if [ "$CLAUDE_CODE_REMOTE" != "true" ]; then
    log "Not a remote session, skipping gh setup"
    exit 0
fi
```

また`CLAUDE_ENV_FILE`はClaude Codeが提供する環境変数ファイルのパスです。ここに書き込むことで、セッション中のPATH設定を永続化できます。

```bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
    echo "export PATH=\"$LOCAL_BIN:\$PATH\"" >> "$CLAUDE_ENV_FILE"
    log "PATH persisted to CLAUDE_ENV_FILE"
fi
```

上記の設定がされているリポジトリにPushして、Claude Code on the Webでそのリポジトリを開けば、セッション開始時に自動でghがインストールされます。ghがインストールされている場合はスキップします。

これにより、Claude Code on the Web上でPRの作成やissueの操作が可能になります。

![has gh](/images/claude-code-web-gh-cli/has-gh.png)

## まとめ

今回は、Claude Code on the WebでGitHub CLI（gh）を使えるようにする方法を紹介しました。

- 2025年12月17日頃に`disallowed_tools`からghが削除された
- カスタム環境で`GITHUB_TOKEN`を設定する
- `sessionStartHooks`でghを自動インストールするスクリプトを設定
- `CLAUDE_CODE_REMOTE`でon the Web環境を判定できる

ghが使えるようになったことで、Claude Code on the Web上でissueを読んでタスクを実行などができます。また、`CLAUDE_CODE_REMOTE`を使えば他のリモートのみのワークフローにも使えると思います。この記事が参考になれば幸いです。

## Xフォローしてくれると嬉しいです

Xでも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://docs.claude.com/en/docs/claude-code/claude-code-on-the-web

https://docs.claude.com/en/docs/claude-code/hooks

https://zenn.dev/oikon/articles/claude-code-web-sandbox

https://docs.github.com/ja/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens

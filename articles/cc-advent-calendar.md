---
title: "Claude Codeアドベントカレンダー: 24 Tips"
emoji: "💫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [claudecode, claude, llm, anthropic]
published: false
---

Oikonです。普段はClaude CodeのCHANGELOGの内容を

## TL;DR

- Claude Codeアドベントカレンダーを12/1~12/24でX(Twitter)で実施
- 24個のClaude CodeのTipsを共有
- [#claude_code_advent_calendar](https://x.com/hashtag/claude_code_advent_calendar?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E2003798309354549671%7Ctwgr%5Eff6fa3222f1582c13d303319a3f55712934a2390%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fembed.zenn.studio%2Ftweetzenn-embedded__d3110cd769602&src=hashtag_click)でポストが見れる

## 背景

12月に入ると技術記事の**アドベントカレンダー**を目にするようになります。さまざまな技術トピックを12月1日〜12月24日まで（25日までのケースもある）、エンジニア達が交代で技術記事を書いていきます。

そんな中でAntrhopicの方がアドベントカレンダーを始めているのを目にしました。

https://x.com/adocomplete/status/1995523532663755010?s=20

このポストを見て、自分も普段Claude Codeについて発信しているのでやってみようと思い始めてみました。

## Claude Code アドベントカレンダー

Day1 ~ Day24までの内容の再掲と、それぞれについて補足をしていきます。

おそらく1つくらいは参考になる話もあると思うので、良かったら眺めてみてください。

### 【Day1】 Opus 4.5 移行ガイド

:::details Original post
https://x.com/oikon48/status/1995541792746602606
:::

:::message
【Day1】Opus 4.5 移行ガイド

古いClaude モデルを Opus 4.5 へ移行する公式Skillsが面白い。Opus 4.5 の振る舞いが以前のモデルと変わっているので、以下の観点での修正指示をしている。

・ツールの過剰な呼び出し
・オーバーエンジニアリング防止
・コード探索不足
・フロントエンドデザインの質
・"think" キーワードへの過剰反応

Opus 4.5 を使う際のモデルの傾向が見えてきて、プロンプトを考える参考になる。

![](https://pbs.twimg.com/media/G7EhDjGagAArLQM?format=jpg&name=4096x4096)

Opus 4.5はツールの利用に積極的になった分、ツールを呼ばなくても良いケースも呼んでコンテキストが埋まったりするので、動きをしっかり見ると良い。
:::

Anthropicの公式リポジトリの中に、**Claudeのモデルを移行するためのSkills**があったので共有しました。Sonnet 4.0, Sonnet 4.5, Opus 4.1で使用していたプロンプトをOpus 4.5のためにアップデートすると言うものです。

興味深いのは、Opus 4.5がツールの呼び出しを積極的に実行するなど、Opus 4.5の傾向について記述されている点でした。モデルが変化することで、API callなどの挙動も変わるため一読しておくと良いと思います。

https://github.com/anthropics/claude-code/tree/main/plugins/claude-opus-4-5-migration

### 【Day2】 Claude Code statusline

:::details Original post
https://x.com/oikon48/status/1995821119467913268
:::

:::message
【Day2】Claude Code statusline

先日の登壇で、「チャット欄下の使用量表示はどうやるの？」という質問が来ていました。

`/statusline <表示したい内容>` で作成できます。

簡単なおすすめ設定は、画像のccusage のstatuslineです！ Thanks to @ryoppippi !

![](https://pbs.twimg.com/media/G7KOLD7a8AARe0N?format=jpg&name=medium)
:::

Claude Codeのstatuslineは、知らない人も多いですが隠れた良い機能です。

使用しているモデルやGitブランチなどを載せることができます。

今までstatuslineを使ったことがない人は、GitHubとかに転がっている設定を使っても良いですが、[ryoppippi](https://x.com/ryoppippi)氏のccusageの設定を試してみると良いと思います。

```md:~/.claude/settings.json
{
  "statusLine": {
    "type": "command",
    "command": "bun x ccusage statusline"
  }
}
```

### 【Day3】 `&` でon the Webに送る

:::details Original post
https://x.com/oikon48/status/1996177293551747577
:::

:::message
【Day3】`&` でon the Webに送る

Claude Code CLIでプロンプトに `&`を先頭に付けると、タスクをClaude Code on the Webに送ることができる。

on the Webは「CLIから開く」ボタンで、ローカルに転送可能。

ちなみにローカルのモデルがon the Webでも使われる。

![](/images/cc-advent-calendar/day3.gif)
:::

### 【Day4】Thinking キーワード

:::details Original post
https://x.com/oikon48/status/1996535955625533508
:::

:::message
【Day4】Thinking キーワード

Claude CodeのThinking(拡張思考)は、内部推論を挟み複雑なタスクが可能に。Tabで有効化できる。

⭕️ ultrathink
❌ think, think hard, 考えて, よく考えて

以前のThinkingキーワードの多くは無効になり、現在は ultrathink のみ有効

![](/images/cc-advent-calendar/day4.gif)
:::

### 【Day5】Claude Code の AGENTS.md 対応

:::details Original post
https://x.com/oikon48/status/1996898343784763479
:::

:::message
【Day5】Claude Code の AGENTS.md 対応

Claude CodeはAGENTS.mdを公式サポートしていないが回避策は存在する。

CLAUDE.mdの中で、`@AGENTS.md`をImportすることでメモリファイル参照してくれるようになる。

Importされたかは`/memory`コマンドで確認可能。
:::

### 【Day6】Claude Code on the Web のSetup

:::details Original post
https://x.com/oikon48/status/1997261626517692850
:::

:::message
【Day6】Claude Code on the Web のSetup

CC on the Webには`CLAUDE_CODE_REMOTE`という環境変数が用意されており、リモート環境か判定できる。

これとSessionStart Hooks利用することで、リモート環境限定のSetupが可能。

実は session-start-hook がリモートのon the Webでも動作する。
:::

### 【Day7】MCPツールのコンテキスト

:::details Original post
https://x.com/oikon48/status/1997623622438174852
:::

:::message
【Day7】MCPツールのコンテキスト

Claude Codeでは `/context` コマンドで、コンテキストウィンドウ内のプロンプトやツールの占有量が確認可能。

特に注意するべきはMCPツールであり、ものによっては8% ~ のコンテキストを占有することがある。
:::

### 【Day8】Claude Skillsを作る Skill Creator

:::details Original post
https://x.com/oikon48/status/1997988839571530111
:::

:::message
【Day8】Claude Skillsを作る Skill Creator

Claude Skills は専門的なタスクを実行する能力を動的にロードできる機能。Codex CLIにもSkillsが最近追加されて注目を集めている。

公式の「Skillsを作るSkills(skill-creator)」で簡単に試すことが可能

```bash
/plugin marketplace add anthropic-agent-skills
```
:::

### 【Day9】Claude Skills + Subagents

:::details Original post
https://x.com/oikon48/status/1998347895431397638
:::

:::message
【Day9】Claude Skills + Subagents

SkillsとSubagentsの使い分けは、「知識やワークフローの提供」と「専門タスクの実行」の観点で考えると良い。

相互に補完し合う関係にあり、連携することも可能。

SubagentsにはYAMLフロントマターで`skills`を指定できる。
:::

### 【Day10】Project ルール `.claude/rules/`

:::details Original post
https://x.com/oikon48/status/1998717425223872974
:::

:::message
【Day10】Project ルール `.claude/rules/`

Claude CodeはCLAUDE.mdの他に、rulesを設定可能。`.claude/rules/` にMarkdownでプロジェクトルールを記載する。

トピック別のプロジェクト指示が管理しやすい。

(例)
・コードスタイル
・テスト規約
・セキュリティ
:::

### 【Day11】CLAUDE.mdのロード

:::details Original post
https://x.com/oikon48/status/1999075187237781744
:::

:::message
【Day11】CLAUDE.mdのロード

Claude Codeのプロジェクト CLAUDE.mdは以下の2箇所に定義でき、起動時にロードされる

./CLAUDE.md
./.claude/CLAUDE.md

ネストされたディレクトリにも定義でき、サブディレクトリ固有の指示を追加できる。
:::

### 【Day12】`rm -rf` の悲劇を防ぐ

:::details Original post
https://x.com/oikon48/status/1999441853423231428
:::

:::message
【Day12】`rm -rf` の悲劇を防ぐ

Claude Codeユーザーが最近も増えているので注意喚起。

定期的に話題に上がるのが、AIエージェントが `rm -rf` を実行してディレクトリを消したなどの悲劇。先日もRedditで話題になっていた。

回避する方法は色々あるが、まずはSandboxモードを使うことを推奨。
:::

### 【Day13】Sandbox で`rm -rf`を予防

:::details Original post
https://x.com/oikon48/status/1999798453006631176
:::

:::message
【Day13】Sandbox で`rm -rf`を予防

Claude Codeは /sandbox コマンドで、ファイルシステムとネットワークのアクセスを制限できる。

アクセスできるディレクトリやネットワークは permissions でコントロール可能。

Sandbox外ではEditやReadだけでなく、Bashの`rm`コマンドも制限される。

![](/images/cc-advent-calendar/day13.gif)
:::

### 【Day14】Claude Hooks でフィルタリング

:::details Original post
https://x.com/oikon48/status/2000164839503917319
:::

:::message
【Day14】Claude Hooks でフィルタリング

Claude CodeのHooksを活用すれば、AIエージェントに渡す情報のフィルタリングが可能。

UserPromptSubmit や PreToolUse を使うのが効果的。

例えば、APIキーなどの機密情報をマスクして渡すことができる。

![](/images/cc-advent-calendar/day14.gif)
:::

### 【Day15】Claude Hooks でフォーマット

:::details Original post
https://x.com/oikon48/status/2000522222319133071
:::

:::message
【Day15】Claude Hooks でフォーマット

Claude CodeのHooksは、イベントにフックしてスクリプトを実行することができ、Claude Codeに決定的な動作を可能にする。

代表的な使い方だと、Linter やFormatter をHooksで起動する。

例として、Claude Codeの PostToolUse フックでフォーマッタを実行する。
:::

### 【Day16】Ctrl + G でエディタ編集

:::details Original post
https://x.com/oikon48/status/2000884610591281582
:::

:::message
【Day16】Ctrl + G でエディタ編集

Claude Codeでは`Ctrl + G`で、外部エディタを開いて編集ができる。

プロンプトだけでなくPlanモードの編集も可能。

外部エディタは環境変数`VISUAL`で決まる
e.g.)
export VISUAL="vim"
export VISUAL="emacs"
export VISUAL="code --wait"

![](/images/cc-advent-calendar/day16.gif)
:::

### 【Day17】Planモード

:::details Original post
https://x.com/oikon48/status/2001247249955733715
:::

:::message
【Day17】Planモード

Claude Code はShift + TabでPlan モードに切り替えることが可能。

Claudeの作成した計画書は【Day16】で紹介した Ctrl + G で編集可能。

Planモードのドキュメントは、`~/.claude/plans/` に保存される。
:::

### 【Day18】Claude in Chrome でGIF作成

:::details Original post
https://x.com/oikon48/status/2001609660521156620
:::

:::message
【Day18】Claude in Chrome でGIF作成

Claude Code と Chrome デスクトップを連携する機能がv2.0.72から追加された。/chrome コマンドで設定可能。

Claude in Chrome にはGIF作成機能がある。これはWebブラウザの操作を記録してGIFアニメーションを作成できる。

![](/images/cc-advent-calendar/day18.gif)
:::

### 【Day19】Agent Skillsベストプラクティス

:::details Original post
https://x.com/oikon48/status/2001973851874570355
:::

:::message
【Day19】Agent Skillsベストプラクティス

Agent Skillsがオープンスタンダードになった。Claude以外のCursorやGitHub CopilotでSkillsを利用できる。

Agent Skillsを作る際には、Claude Skillsベストプラクティスが参考になる↓

・SKILL.mdは500行以下
・具体的なexampleを含める
・トリガー条件を明確にする
:::

### 【Day20】Auto-compact Bufferサイズ

:::details Original post
https://x.com/oikon48/status/2002334161814573399
:::

:::message
【Day20】Auto-compact Bufferサイズ

Claude CodeはAuto-compactを有効にした時、会話を自動圧縮をするためのバッファ(Auto-compact Buffer)が存在する。

このAuto-compact Bufferは、環境変数CLAUDE_CODE_MAX_OUTPUT_TOKENS で変動する。

デフォルトは32k
:::

### 【Day21】非同期 Subagents

:::details Original post
https://x.com/oikon48/status/2002703847291236753
:::

:::message
【Day21】非同期 Subagents

Claude Code のSubagents は、非同期(バックグラウンド)でタスクを実行できる。

非同期タスクが終了した際には、メインエージェントに通知される。

非同期で並列作業を実施できるため、以下のような使い方が可能↓
・並列コードベース探索
・バックグラウンドでのテスト実行
:::

### 【Day22】Claude Code Action + Agent Skills

:::details Original post
https://x.com/oikon48/status/2003076804769464433
:::

:::message
【Day22】Claude Code Action + Agent Skills

Claude Code Actionは、GitHubでUbuntuマシンにリポジトリをCloneして、そこでClaude Codeを動かす。

この際リポジトリ内にSkillsがあれば、もちろんGitHub Actions上でSkillsを使用することできる。

必要な設定は、allowed_toolsにSkillを追加するだけ。
:::

### 【Day23】Claude Codeの並列実行

:::details Original post
https://x.com/oikon48/status/2003445380370042959
:::

:::message
【Day23】Claude Codeの並列実行

Claude Codeは自律的にタスクをこなせるため、並列実行を積極的に使っていきたい。

並列実行には主に2つのアプローチがある:
・同じブランチで並列実行(Subagents)
・別のブランチで並列実行(git worktree)

前者はClaude Code内蔵、後者はターミナルを複数開いて別々のClaude Codeを起動する。
:::

### 【Day24】Claude Code Ecosystem

:::details Original post
https://x.com/oikon48/status/2003783713465745652
:::

:::message
【Day24(Last)】Claude Code Ecosystem

Claude Codeのエコシステムを再確認する。2025年2月24日から、たった10ヶ月でこのエコシステムが出来ているのは尋常ではない。

Claude Code CLIから始まりSDKが登場。IDE拡張機能にVSCode、JetBrains、Xcode対応。GitHub Actions連携、Chromeブラウザ連携など、多様なエコシステムが構築されている。
:::

## まとめ

https://x.com/oikon48/status/2003798309354549671?s=20

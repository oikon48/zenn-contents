---
title: "Claude Codeアドベントカレンダー: 24 Tipsまとめ"
emoji: "💫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [claudecode, claude, ai, anthropic]
published: true
published_at: 2025-12-26 07:03
---

## TL;DR

- Claude Codeアドベントカレンダーを12/1~12/24で単独実施
- 24個のClaude CodeのTipsをX(Twitter)で共有
- [#claude_code_advent_calendar](https://x.com/hashtag/claude_code_advent_calendar?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E2003798309354549671%7Ctwgr%5Eff6fa3222f1582c13d303319a3f55712934a2390%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fembed.zenn.studio%2Ftweetzenn-embedded__d3110cd769602&src=hashtag_click)でポストが見れる

| Day | トピック | Day | トピック |
|:---:|:--------|:---:|:--------|
| [Day1](#【day1】-opus-45-移行ガイド) | Opus 4.5 移行ガイド | [Day13](#【day13】-sandbox-でrm--rfを予防) | Sandboxで予防 |
| [Day2](#【day2】-claude-code-statusline) | Claude Code statusline | [Day14](#【day14】-claude-hooks-でフィルタリング) | Hooks でフィルタリング |
| [Day3](#【day3】--でon-the-webに送る) | `&`でon the Webに送る | [Day15](#【day15】-claude-hooks-でフォーマット) | Hooks でフォーマット |
| [Day4](#【day4】-thinking-キーワード) | Thinking キーワード | [Day16](#【day16】-ctrl--g-でエディタ編集) | Ctrl + G でエディタ編集 |
| [Day5](#【day5】-claude-code-の-agentsmd-対応) | AGENTS.md 対応 | [Day17](#【day17】-planモード) | Planモード |
| [Day6](#【day6】-claude-code-on-the-web-のsetup) | on the Web のSetup | [Day18](#【day18】-claude-in-chrome-でgif作成) | Claude in Chrome でGIF作成 |
| [Day7](#【day7】-mcpツールのコンテキスト) | MCPツールのコンテキスト | [Day19](#【day19】-agent-skillsベストプラクティス) | Agent Skillsベストプラクティス |
| [Day8](#【day8】-claude-skillsを作る-skill-creator) | Skill Creator | [Day20](#【day20】-auto-compact-bufferサイズ) | Auto-compact Bufferサイズ |
| [Day9](#【day9】-claude-skills--subagents) | Skills + Subagents | [Day21](#【day21】-非同期-subagents) | 非同期 Subagents |
| [Day10](#【day10】-project-ルール-clauderules) | Project ルール | [Day22](#【day22】-claude-code-action--agent-skills) | Code Action + Agent Skills |
| [Day11](#【day11】-claudemdのロード) | CLAUDE.mdのロード | [Day23](#【day23】-claude-codeの並列実行) | Claude Codeの並列実行 |
| [Day12](#【day12】-rm--rf-の悲劇を防ぐ) | `rm -rf`の悲劇を防ぐ | [Day24](#【day24】-claude-code-ecosystem) | Claude Code Ecosystem |

## 背景

12月に入ると技術記事の**アドベントカレンダー**を目にするようになります。さまざまな技術トピックを12月1日〜12月24日まで（25日までのケースもある）、エンジニア達が交代で技術記事を書いていきます。

そんな中でAnthropicの方が、Xでアドベントカレンダーを始めているのを目にしました。

https://x.com/adocomplete/status/1995523532663755010?s=20

このポストを見て、自分も普段Claude Codeについて発信しているのでやってみようと思ったのがきっかけです。

もともと自分の中で検証が足りていないと感じていた機能を、調査できる良い機会だと思いました。

## Claude Code アドベントカレンダー

Day1 ~ Day24までの内容の再掲と、それぞれについて補足をしていきます。

おそらく1つくらいは参考になる話もあると思うので、良かったら眺めてみてください。

:::details トピック一覧(再掲)
| Day | トピック | Day | トピック |
|:---:|:--------|:---:|:--------|
| [Day1](#【day1】-opus-45-移行ガイド) | Opus 4.5 移行ガイド | [Day13](#【day13】-sandbox-でrm--rfを予防) | Sandboxで予防 |
| [Day2](#【day2】-claude-code-statusline) | Claude Code statusline | [Day14](#【day14】-claude-hooks-でフィルタリング) | Hooks でフィルタリング |
| [Day3](#【day3】--でon-the-webに送る) | `&`でon the Webに送る | [Day15](#【day15】-claude-hooks-でフォーマット) | Hooks でフォーマット |
| [Day4](#【day4】-thinking-キーワード) | Thinking キーワード | [Day16](#【day16】-ctrl--g-でエディタ編集) | Ctrl + G でエディタ編集 |
| [Day5](#【day5】-claude-code-の-agentsmd-対応) | AGENTS.md 対応 | [Day17](#【day17】-planモード) | Planモード |
| [Day6](#【day6】-claude-code-on-the-web-のsetup) | on the Web のSetup | [Day18](#【day18】-claude-in-chrome-でgif作成) | Claude in Chrome でGIF作成 |
| [Day7](#【day7】-mcpツールのコンテキスト) | MCPツールのコンテキスト | [Day19](#【day19】-agent-skillsベストプラクティス) | Agent Skillsベストプラクティス |
| [Day8](#【day8】-claude-skillsを作る-skill-creator) | Skill Creator | [Day20](#【day20】-auto-compact-bufferサイズ) | Auto-compact Bufferサイズ |
| [Day9](#【day9】-claude-skills--subagents) | Skills + Subagents | [Day21](#【day21】-非同期-subagents) | 非同期 Subagents |
| [Day10](#【day10】-project-ルール-clauderules) | Project ルール | [Day22](#【day22】-claude-code-action--agent-skills) | Code Action + Agent Skills |
| [Day11](#【day11】-claudemdのロード) | CLAUDE.mdのロード | [Day23](#【day23】-claude-codeの並列実行) | Claude Codeの並列実行 |
| [Day12](#【day12】-rm--rf-の悲劇を防ぐ) | `rm -rf`の悲劇を防ぐ | [Day24](#【day24】-claude-code-ecosystem) | Claude Code Ecosystem |
:::

ハッシュタグ [#claude_code_advent_calendar](https://x.com/hashtag/claude_code_advent_calendar?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E2003798309354549671%7Ctwgr%5Eff6fa3222f1582c13d303319a3f55712934a2390%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fembed.zenn.studio%2Ftweetzenn-embedded__d3110cd769602&src=hashtag_click)で、実際のポストも確認できます。

### 【Day1】 Opus 4.5 移行ガイド

:::details Original post
https://x.com/oikon48/status/1995541792746602606
:::

:::message
**【Day1】 Opus 4.5 移行ガイド**

古いClaude モデルを Opus 4.5 へ移行する公式Skillsが面白い。Opus 4.5 の振る舞いが以前のモデルと変わっているので、以下の観点での修正指示をしている。

・ツールの過剰な呼び出し
・オーバーエンジニアリング防止
・コード探索不足
・フロントエンドデザインの質
・"think" キーワードへの過剰反応

Opus 4.5 を使う際のモデルの傾向が見えてきて、プロンプトを考える参考になる。

![](https://pbs.twimg.com/media/G7EhDjGagAArLQM?format=jpg&name=4096x4096)

Opus 4.5はツールの利用に積極的になった分、ツールを呼ばなくても良いケースでも呼び出してコンテキストが埋まることがあるので、動きをしっかり見ると良い。
:::

Anthropicの公式リポジトリの中に、**Claudeのモデルを移行するためのSkills**があったので共有しました。Sonnet 4.0, Sonnet 4.5, Opus 4.1で使用していたプロンプトをOpus 4.5のためにアップデートするSkillsです。

興味深いのは、Opus 4.5がツールの呼び出しを積極的に実行するなど、Opus 4.5の傾向について記述されている点でした。モデルが変化することで、API callなどの挙動も変わるため一読しておくと良いと思います。

https://github.com/anthropics/claude-code/tree/main/plugins/claude-opus-4-5-migration

### 【Day2】 Claude Code statusline

:::details Original post
https://x.com/oikon48/status/1995821119467913268
:::

:::message
**【Day2】 Claude Code statusline**

先日の登壇で、「チャット欄下の使用量表示はどうやるの？」という質問が来ていました。

`/statusline <表示したい内容>` で作成できます。

簡単なおすすめ設定は、画像のccusage のstatuslineです！ Thanks to @ryoppippi !

![](https://pbs.twimg.com/media/G7KOLD7a8AARe0N?format=jpg&name=medium)
:::

Claude Codeのstatuslineは、知らない人も多いですが隠れた良い機能です。

使用しているモデルやGitブランチなどをチャット欄の下に載せることができます。

今までstatuslineを使ったことがない人は、GitHubとかに転がっている設定を使っても良いですが、[ryoppippi](https://x.com/ryoppippi)氏のccusageの設定を試してみると良いと思います。

```json:~/.claude/settings.json
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
**【Day3】 `&` でon the Webに送る**

Claude Code CLIでプロンプトに `&`を先頭に付けると、タスクをClaude Code on the Webに送ることができる。

on the Webは「CLIから開く」ボタンで、ローカルに転送可能。

ちなみにローカルのモデルがon the Webでも使われる。

![](/images/cc-advent-calendar/day3.gif)
:::

Claude Code 2.0.45から入った機能です。Claude Code on the Webが登場したことで、ローカルとリモートのタスクが繋げられるようになりました。

特にローカルのCLIからは簡単にバックグラウンドタスクとして、小さいタスクを投げられるので使い道は多いです。

### 【Day4】 Thinking キーワード

:::details Original post
https://x.com/oikon48/status/1996535955625533508
:::

:::message
**【Day4】 Thinking キーワード**

Claude CodeのThinking(拡張思考)は、内部推論を挟み複雑なタスクが可能に。Tabで有効化できる。

⭕️ ultrathink
❌ think, think hard, 考えて, よく考えて

以前のThinkingキーワードの多くは無効になり、現在は ultrathink のみ有効

![](/images/cc-advent-calendar/day4.gif)
:::

長いこと誤解されていましたが、**現在のClaude Codeは`ultrathink`以外のキーワードはThinking（拡張思考）を発火しません**。

Claude Code 2.0.0でThinking modeをトグルで有効にできるようになった際に、`think`や`think hard`は効かなくなりました。

[Anthropicの方ですら把握していない](https://x.com/adocomplete/status/1999529578990088416?s=20)ようだったので、知らなくても仕方がないと思います。

ちなみに公式ドキュメントは12月13日に更新されてました。更新まで2ヶ月かかった。。。

[![](/images/cc-advent-calendar/diff.png)](https://github.com/oikon48/cc-doc-tracker/commit/aa39e0cc3a1d45ee12399b2d49ba13fea9f44be2#diff-cb684818efff3165e0bc021799dc36a422197e65316ad5ce26efb41fef3bfa8dR536)

### 【Day5】 Claude Code の AGENTS.md 対応

:::details Original post
https://x.com/oikon48/status/1996898343784763479
:::

:::message
**【Day5】 Claude Code の AGENTS.md 対応**

Claude CodeはAGENTS.mdを公式サポートしていないが回避策は存在する。

CLAUDE.mdの中で、`@AGENTS.md`をImportすることでメモリファイル参照してくれるようになる。
Importされたかは`/memory`コマンドで確認可能。

![](https://pbs.twimg.com/media/G7ZjsvBa4AAIYmv?format=jpg&name=medium)
:::

Claude Codeは**AGENTS.mdをサポートしていません**。これはGitHub issue上でも8月から長く議論されています。

https://github.com/anthropics/claude-code/issues/6235

Anthropicの[公式ドキュメント](https://code.claude.com/docs/en/claude-code-on-the-web#best-practices)には一応解決法が書いてあり、CLAUDE.md内でAGENTS.mdをImportするように書かれています

他の方法としては、`ln -s AGENTS.md CLAUDE.md`でシンボリックリンクを設定する回避策もあります。

### 【Day6】 Claude Code on the Web のSetup

:::details Original post
https://x.com/oikon48/status/1997261626517692850
:::

:::message
**【Day6】 Claude Code on the Web のSetup**

CC on the Webには`CLAUDE_CODE_REMOTE`という環境変数が用意されており、リモート環境か判定できる。これとSessionStart Hooks利用することで、リモート環境限定のSetupが可能。

実は session-start-hook がリモートのon the Web 環境にはグローバル設定で用意されているため、これを利用することでリモート環境限定で起動するHooksの作成を簡単にお願いできる。

![](https://pbs.twimg.com/media/G7ew2JZa4AAbPBx?format=jpg&name=medium)
:::

Claude Code on the Webはリモートサンドボックス環境を利用するため、ローカルのセットアップを持ち込むことができません。

`CLAUDE_CODE_REMOTE`で判定して、リモートのみセットアップスクリプトをSessionStart Hooksを実行することで、ローカルのセットアップをリモートでも再現できます。

ちなみにClaude Code on the Webには`session-start-hooks`が内蔵されており、公式もSessionStart Hooksをリモートで使うことを推奨しています。

https://code.claude.com/docs/en/claude-code-on-the-web

### 【Day7】 MCPツールのコンテキスト

:::details Original post
https://x.com/oikon48/status/1997623622438174852
:::

:::message
**【Day7】 MCPツールのコンテキスト**

Claude Codeでは `/context` コマンドで、コンテキストウィンドウ内のプロンプトやツールの占有量が確認可能。

特に注意するべきはMCPツールであり、ものによっては8% ~ 30%近く占有する物もある(200kと仮定)。必要最小限のMCPツールを使用することが望ましい。

![](https://pbs.twimg.com/media/G7fqCBgbUAAGu5u?format=jpg&name=medium)
:::

MCPツールはツール説明（Description）をAIエージェントに読み込ませるため、存在しているだけでコンテキストを占領します。

特にブラウザ操作系のMCPツールは大量のコンテキストを食い潰しがちです。

SkillsでMCPツールを使用すればコンテキストの問題は解決するという意見と時々目にしますが、AIエージェントのコンテキストにMCPツールが含まれている限り解決しません。SkillsでMCPサーバーを使わず、MCPツールと同等の実行が可能なスクリプトやAPIコールを使う場合は、Skillsを使ってコンテキストの圧迫を軽減できるケースはあります。

### 【Day8】 Claude Skillsを作る Skill Creator

:::details Original post
https://x.com/oikon48/status/1997988839571530111
:::

:::message
**【Day8】 Claude Skillsを作る Skill Creator**

Claude Skills は専門的なタスクを実行する能力を動的にロードできる機能。Codex CLIにもSkillsが最近追加されて注目を集めている。

公式の「Skillsを作るSkills(skill-creator)」で簡単に試すことが可能

```
/plugin marketplace add anthropics/skills
/plugin install example-skills@anthropic-agent-skills
```

Claude Codeを再起動したら「〇〇のSkillsを作って」と依頼することでskill-creator skillsを発動可能。

![](https://pbs.twimg.com/media/G7pH8aqbYAEXgtW?format=jpg&name=medium)
:::

Agent Skillsを作るskill-creatorです。最近話題になっているのを見かけますが、[10月からskill-creatorは存在しています](https://x.com/oikon48/status/1979826910093099336?s=20)。

「〇〇のSkillsを作成して」と頼めばSkillsを作るSkillsが発動するため、Skillsの叩き台を作るのに適しています。

ただ必ずしもベストプラクティスに従っている訳ではないので、ブラッシュアップは必要だと思っています。ベストプラクティスについては【Day19】で触れています。

### 【Day9】 Claude Skills + Subagents

:::details Original post
https://x.com/oikon48/status/1998347895431397638
:::

:::message
**【Day9】 Claude Skills + Subagents**

SkillsとSubagentsの使い分けは、「知識やワークフローの提供」と「専門タスクの実行」の観点で考えると良い。相互に補完し合う関係にあり、連携することも可能。

SubagentsにはYAMLフロントマターで`skills` を定義可能。例えば画像のように、フルスタック用のSubagentsに、特定の2つのSkillsを定義できる。ここで定義されたSkillsは、Subagents起動時に自動的にコンテキストにロードされる。

![](https://pbs.twimg.com/media/G7uCi1HbcAAZIRF?format=jpg&name=medium)
:::

この頃は**Codex CLIがSkills対応した**ので、SkillsのTipsを紹介してます。

専門タスクを実行するSubagentsに、フロントマターを使ってSkillsを明示的に指定する方法です。

意外とフロントマターで色々指定できるので公式ドキュメントを読んでおくと良いです。

https://code.claude.com/docs/en/sub-agents#file-format

### 【Day10】 Project ルール `.claude/rules/`

:::details Original post
https://x.com/oikon48/status/1998717425223872974
:::

:::message
**【Day10】 Project ルール `.claude/rules/`**

Claude CodeはCLAUDE.mdの他に、rulesを設定可能。`.claude/rules/` にMarkdownでプロジェクトルールを記載する。

トピック別のプロジェクト指示が管理しやすい。

(例)
・コードスタイル
・テスト規約
・セキュリティ

YAMLフロントマターで特定のファイルのみに適用されるルールを定義可能:

(例)
---
paths: src/api/**/*.ts
---

検証の結果、Globパターンマッチングで動的にrulesがコンテキストにロードされる仕様と推察される。

![](https://pbs.twimg.com/media/G7zdCyIbMAAFmCs?format=jpg&name=medium)
:::

v2.0.64にProjectルールがサポートされたので紹介しました。

`paths`のフロントマター設定で動的にルールをロードできる検証もしています。

https://x.com/oikon48/status/1998710902854660528?s=20?conversation=none

### 【Day11】 CLAUDE.mdのロード

:::details Original post
https://x.com/oikon48/status/1999075187237781744
:::

:::message
**【Day11】 CLAUDE.mdのロード**

Claude Codeのプロジェクト CLAUDE.mdは以下の2箇所に定義でき、起動時にロードされる

./CLAUDE.md
./.claude/CLAUDE.md

ネストされたディレクトリにも定義でき、 これらは起動時にロードされず、画像のようにディレクトリ内を操作する際に読み込まれる。

v2.0.64 から`./.claude/rules` のプロジェクトルールをサポートするようになった。大規模プロジェクトの際にrulesが推奨されているが、rulesはCLAUDE.mdよりも柔軟に適用条件を限定できるため、現在はネストされたCLAUDE.mdとrulesを、プロジェクトに応じて選択することができる。
![](https://pbs.twimg.com/media/G74jQg4asAApwpz?format=jpg&name=medium)
:::

Claude CodeのProjectルール実装時に、CLAUDE.mdとの違いが話題になったので紹介しました。

従来の方法ではfrontendやbackendといったディレクトリの中に別途CLAUDE.mdを配置することで、段階的にCLAUDE.mdを読む方法も存在しています。Projectルールとの使い分けですね。

### 【Day12】 `rm -rf` の悲劇を防ぐ

:::details Original post
https://x.com/oikon48/status/1999441853423231428
:::

:::message
**【Day12】 `rm -rf` の悲劇を防ぐ**

Claude Codeユーザーが最近も増えているので注意喚起。

定期的に話題に上がるのが、AIエージェントが `rm -rf` を実行してディレクトリを消したなどの悲劇。先日もRedditで話題になっていた。

回避する方法は色々あるが、まずは settings.json に`permissions.deny` の設定する。ここで `rm` や DB操作のコマンド権限を否定することで、Claude Codeが誤って消してはいけないファイルやディレクトリを削除する悲劇をある程度回避できる。

`permissions.ask` でClaudeが確認を取ってから実行する柔軟な設定もできるので、こちらで`rm`を実行する時だけ確認することもできる。`--dangerously-skip-permissions`オプションがついていても、permissions設定が優先される。

![](https://pbs.twimg.com/media/G79okHeacAAda1o?format=jpg&name=medium)
:::

AIエージェントが`rm -rf`を実行した話です。正直何度も見たので見飽きました笑。が、新規ユーザーにClaude Codeに失望されるのも悲しいので取り上げています。

Redditで話題になっていたのもあり、結構反響がありました。

https://www.reddit.com/r/ClaudeAI/comments/1pgxckk/claude_cli_deleted_my_entire_home_directory_wiped/

### 【Day13】 Sandbox で`rm -rf`を予防

:::details Original post
https://x.com/oikon48/status/1999798453006631176
:::

:::message
**【Day13】 Sandbox で`rm -rf`を予防**

Claude Codeは /sandbox コマンドで、ファイルシステムとネットワークのアクセスを制限できる。アクセスできるディレクトリやネットワークは permissions でコントロール可能。

Sandbox外ではEditやReadだけでなく、Bashの`rm` コマンドなども実行できないため、変更を作業ディレクトリと許可されたディレクトリに限定できる。

Sandbox設定と【Day12】の permissions 設定を組み合わせることで、AIエージェントによるファイル操作の事故が起きにくい開発環境を整えることが期待できる。

![](/images/cc-advent-calendar/day13.gif)
:::

【Day12】の`rm -rf`コマンド繋がりです。現在は作業ディレクトリ以外に変更を加えないようSandboxを使うことをおすすめします。

他にもHooksなどでコマンド実行前に止めることも可能です。いくつかの防御策を講じることで、ガードレールをより強固にできます。

### 【Day14】 Claude Hooks でフィルタリング

:::details Original post
https://x.com/oikon48/status/2000164839503917319
:::

:::message
**【Day14】 Claude Hooks でフィルタリング**

Claude CodeのHooksを活用すれば、AIエージェントに渡す情報のフィルタリングが可能。UserPromptSubmit や PreToolUse を使うのが効果的。

例えば、API キーがプロンプトやファイルに含まれているケースでは、AIエージェントに読み込まれるまでにHooksでブロックすることが可能。センシティブ情報のフィルタリングの使い方ができる。

![](/images/cc-advent-calendar/day14.gif)
:::

Claude Hooksの使い方が難しいという意見を目にしたので、Hooksを取り上げました。

個人的にもUserPromptSubmitとPreToolUseのHooksを使ったことがなかったので、研究がてらフィルタリングのHooksを作ってみました。

この例は事前にチェックして実行を止めるタイプですが、UserPromptSubmitはプロンプトのAppendなどにも使えるので、実はHooksは奥深かったりします。

### 【Day15】 Claude Hooks でフォーマット

:::details Original post
https://x.com/oikon48/status/2000522222319133071
:::

:::message
**【Day15】 Claude Hooks でフォーマット**

Claude CodeのHooksは、イベントにフックしてスクリプトを実行することができ、Claude Codeに決定的な動作を可能にする。代表的な使い方だと、Linter やFormatter をHooksで起動する。

例として、Claude Codeの PostToolUse Hooks でファイル編集後に自動で prettier を実行するサンプルを挙げる。Hooksのサンプルスクリプトを、settings.jsonに画像のように設定すれば、Claude Code編集後に PostToolUse Hooksが発火していることが分かる。

![](https://pbs.twimg.com/media/G8NAMQ1b0AARnWR?format=jpg&name=medium)
:::

【Day14】に続きClaude Hooksです。

フォーマットのためにPostToolsUse Hooksを使う方は結構多いと思います。HooksのメリットはClaudeのコンテキスト外で処理を確定的に行えるので、ぜひ使ってみて欲しいため取り上げました。

### 【Day16】 Ctrl + G でエディタ編集

:::details Original post
https://x.com/oikon48/status/2000884610591281582
:::

:::message
**【Day16】 Ctrl + G でエディタ編集**

Claude Codeでは`Ctrl + G`で、外部エディタを開いて編集ができる。プロンプトだけでなくPlanモードの編集も可能。

外部エディタは環境変数`VISUAL`で決まる

e.g.)
export VISUAL="vim"
export VISUAL="emacs"
export VISUAL="code --wait"
export VISUAL="cursor --wait"

![](/images/cc-advent-calendar/day16.gif)
:::

Claude Codeは`Ctrl + G`で、使い慣れたエディタで編集が可能です。

ポストには書きそびれましたが`VISUAL`だけでなく`EDITOR`もチェックしています。両方存在する場合`VISUAL`が優先されます。

### 【Day17】 Planモード

:::details Original post
https://x.com/oikon48/status/2001247249955733715
:::

:::message
**【Day17】 Planモード**

Claude Code はShift + TabでPlan モードに切り替えることが可能。Claudeの作成した計画書は【Day16】で紹介した Ctrl + G で編集可能。

Planモードのドキュメントは、`~/.claude/plans/` に保存されるようになっており、詳細を確認できる。ただし現在はPlanドキュメントを任意のパスに生成することはできない。

![](https://pbs.twimg.com/media/G8WToHQaQAAJB8D?format=jpg&name=medium)
:::

Planモードは度々変更があるので紹介しました。v2.0.34あたりから最近ドキュメントとして保存されるようになったのは大きな変更だと思います。

Planドキュメントの場所を指定したいというIssueも存在するため、今後Planモードのドキュメントの生成場所を指定できるようになるかもしれません。

https://github.com/anthropics/claude-code/issues/12619

### 【Day18】 Claude in Chrome でGIF作成

:::details Original post
https://x.com/oikon48/status/2001609660521156620
:::

:::message
**【Day18】 Claude in Chrome でGIF作成**

Claude Code と Chrome デスクトップを連携する機能がv2.0.72から追加された。/chrome コマンドで設定可能。

Claude in Chrome にはGIF作成機能がある。これはWeb ブラウザの操作をレコーディングして、一部始終をGIFに出力することができる。この機能でWebアプリの簡単な操作ガイドを作ることも可能。

例えば動画はフライト検索について、成功・失敗の2つのケースをGIFにする作業の様子。ブラウザ経由でGIFをダウンロードする。実際に作成されたGIFはスレッドに添付する↓🧵

![](/images/cc-advent-calendar/day18.gif)
:::

Claude in Chromeの機能がリリースされたので紹介しました。

Chrome DevTools MCPと役割がかぶる点も多いですが、ユーザープロファイルを使用してChromeの操作をしやすくなった点が大きいです。

詳しい機能については別途以下のポストで紹介しています。

https://x.com/oikon48/status/2001451347380703258?s=20

### 【Day19】 Agent Skillsベストプラクティス

:::details Original post
https://x.com/oikon48/status/2001973851874570355
:::

:::message
**【Day19】 Agent Skillsベストプラクティス**

Agent Skillsがオープンスタンダードになった。Claude以外のCursorやGitHub CopilotでSkillsを利用できる。

Agent Skillsを作る際には、Claude Skillsベストプラクティスが参考になる↓

・SKILL.mdは500行以下
・例は具体的に
・ファイル参照は深さ１階層まで
・段階的開示を適切に設定
・明確なステップのワークフロー
・スクリプトの活用
・Windows-styleのパスは使わない
・重要な操作の検証確認とフィードバックループ
・最低でも3つの評価がある
・複数のモデルでテストされている

など。

これら以外にも命名規則やアンチパターンが公開されており、参考になる点が非常に多い。まずは"【Day8】Claude Skillsを作るSkill Creator"を使用してAgent Skillsを作成してみて、そこからブラッシュアップするのが良い。

<https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices>

![](https://pbs.twimg.com/media/G8hkTY3bkAArhH8?format=jpg&name=medium)
:::

Claude Skillsがオープンスタンダードになり、[Agents Skills](https://agentskills.io/home)に名前が変わりました。今後しばらくは去年のMCP同様、注目を集めると思います。

10月に公開された[Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)も再注目されていたため、この際に取り上げました。

### 【Day20】 Auto-compact Bufferサイズ

:::details Original post
https://x.com/oikon48/status/2002334161814573399
:::

:::message
**【Day20】 Auto-compact Bufferサイズ**

Claude CodeはAuto-compactを有効にした時、会話を自動圧縮をするためのバッファ(Auto-compact Buffer)が存在する。

このAuto-compact Bufferは、環境変数CLAUDE_CODE_MAX_OUTPUT_TOKENS で変動する。デフォルトは32k であり、200kのコンテキストウィンドウの22.5%を占有する。モデル概要をみると64k が最大値だが、その場合コンテキストウィンドウの40%近くがバッファで占有されることを理解するべきである。

Haiku、Sonnet、Opusのモデルによるバッファの差は見られなかった。

![](https://pbs.twimg.com/media/G8iSCiFakAIKeBS?format=jpg&name=medium)
:::

Auto-compat Bufferがなぜか以前と違うことに気づいたので調査したところ、今回のポストのような仕様を発見しました。

コンテキストへの関心が高まっている中で、できるだけコンテキストウィンドウの空きを確保しようとしている方は一考する価値がある設定だと思います。

**あとは`CLAUDE_CODE_MAX_OUTPUT_TOKENS`を何も考えず設定している方も見直したほうがいいと思います**。

### 【Day21】 非同期 Subagents

:::details Original post
https://x.com/oikon48/status/2002703847291236753
:::

:::message
**【Day21】 非同期 Subagents**

Claude Code のSubagents は、非同期(バックグラウンド)でタスクを実行できる。非同期タスクが終了した際には、メインエージェントに通知される。非同期で並列作業を実施できるため、以下のような使い方が可能↓

・並列コードベース探索
・コードレビューの実行
・検索タスクの並列実行

画像はコードレビューの並列実行であり、Codex CLIとCodeRabbit CLIをヘッドレスで呼び出すSubagentsを、複数呼び出してレビューさせている。

![](https://pbs.twimg.com/media/G8sCbz9bIAArmAd?format=jpg&name=medium)
:::

非同期でSubagentsを実行できる機能はv2.0.60で追加されました。

以前として関連するタスクについて複数のSubagentsを同時並行で流すと、Subagentsのタスク終了を待つケースがありますが、前よりも使いやすくなった印象です。

2026年にはSwarming（組織実行）に取り掛かるという話をAnthropicのエンジニアからも聞いているので、並列実行系は試しておいて損はないと思います。

### 【Day22】 Claude Code Action + Agent Skills

:::details Original post
https://x.com/oikon48/status/2003076804769464433
:::

:::message
**【Day22】 Claude Code Action + Agent Skills**

Claude Code Actionは、GitHubでUbuntuマシンにリポジトリをCloneして、そこでClaude Codeを動かす。この際リポジトリ内にSkillsがあれば、もちろんGitHub Actions上でSkillsを使用することできる。

必要な設定は、

・GitHub Actionsで使用するSkillsを`.claude/skills/`に用意する
・claude_args: `--allowed-tools`に"Skill"を追加する

/install-github-appコマンドを使えばCLAUDE_CODE_OAUTH_TOKENのシークレットは自動で登録されるので、それを使ってGitHub ActionsのワークフローにClaude Code Actionを組み込むと良い。

![](https://pbs.twimg.com/media/G8xbiP4aMAAGAjd?format=jpg&name=medium)
:::

Claude Code ActionでAgent Skillsを実行するのは、最近個人的に注目している機能です。

以前からClaude Code Actionは使えましたが、Skillsでより柔軟に専門タスクを実行できるようになりました。それに加えて、GitHub Actionsの料金改定が予定されており、GitHubホストの場合は値下げされるためますます使いやすくなります。

https://github.blog/changelog/2025-12-16-coming-soon-simpler-pricing-and-a-better-experience-for-github-actions/

### 【Day23】 Claude Codeの並列実行

:::details Original post
https://x.com/oikon48/status/2003445380370042959
:::

:::message
**【Day23】 Claude Codeの並列実行**

Claude Codeは自律的にタスクをこなせるため、並列実行を積極的に使っていきたい。並列実行には主に2つのアプローチがある:

・同じブランチで並列実行(Subagents)
・別のブランチで並列実行(git worktree)

前者は、Claude Code の複数のSubagentsを並列実行するケースで、Read権限のみのコードレビューや、コンポーネントの境界面がはっきりしているタスクなどを割り振る例が挙げられる。最近ではSkillsで明示的にSubagentsを呼び出す手法も注目されており、簡単に試せる割に適用範囲が広い。

後者は、git worktreeを用いて複数のエージェントを並列実行させるケース、タスクの分割だけでなく実装のコンペなどにも応用できる。tmux を用いればClaude Codeから別のWorker paneのClaude Codeに指示を出すことも可能。

Anthropicは今後Claudeの組織実行(Swarming)の拡張も検討していると話しているため、並列実行に慣れておくと良い。

![](https://pbs.twimg.com/media/G82q22DagAUQtOx?format=jpg&name=medium)
:::

先日、Claude Code Meetup Tokyoがあり、そこで直接Anthropicのエンジニアに質問をする機会がありました。

その時に私は最初の質問をしたのですが、Swarmingの強化について回答をもらったため、並列実行について取り上げました。

Meetupの議事録を取ってくださった方がいらっしゃるので、そちらも読んでみると参考になります。

https://x.com/iotorishima/status/2003172156134556027?s=20

### 【Day24】 Claude Code Ecosystem

:::details Original post
https://x.com/oikon48/status/2003783713465745652
:::

:::message
**【Day24(Last)】Claude Code Ecosystem**

Claude Codeのエコシステムを再確認する。2025年2月24日から、たった10ヶ月でこのエコシステムが出来ているのは尋常ではない。

Claude Code CLIから始まりSDKが登場。IDE拡張機能にVSCode ForkやJetBrainsなどがサポートされ裾野が広がった。GitHub Actionsも初期から活躍している。

Sandbox 機能が整ったことでClaude Code on the Webがリリースされると、モバイルアプリ・デスクトップ・SlackでClaude Code on the Webにタスクを投げられる環境になった。

Chrome拡張も単体機能としてAIブラウザ的な使い方がされていたが、最近のアップデートでClaude CodeからMCPツールで、Chromeを直接操作できる機能がリリースされた。

今後さらに加速し、Claude Codeのエコシステムは拡大するだろう。来年の今頃にはどのようになっているかとても楽しみ。

![](https://pbs.twimg.com/media/G87ZmcZbgAApatV?format=jpg&name=medium)
:::

アドベントカレンダーの最後はClaude Codeのエコシステムについて紹介しました。

もともとターミナルのみのプロダクトだったところから、今ではon the Webを使用してモバイルアプリでも実行できるようになったのは驚くべきことですね。

## まとめ

Claude Codeのアドベントカレンダーを実施して、Xで24日間Tipsを共有しました。

3月ごろからずっとClaude Codeを追っていたこともあり、ネタ出しには困りませんでしたが、ある程度の情報のクオリティを担保するのに少し苦心しました。

今回の企画で、自分の中のClaude Codeの知識の確認と新規の検証ができたのでよかったです。

https://x.com/oikon48/status/2003798309354549671?s=20

## Xフォローしてくれると嬉しいです

今回のような内容をXで情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://github.com/anthropics/claude-code/tree/main/plugins/claude-opus-4-5-migration

https://github.com/anthropics/claude-code/issues/6235

https://code.claude.com/docs/en/claude-code-on-the-web

https://code.claude.com/docs/en/sub-agents#file-format

https://www.reddit.com/r/ClaudeAI/comments/1pgxckk/claude_cli_deleted_my_entire_home_directory_wiped/

https://github.com/anthropics/claude-code/issues/12619

https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices

https://github.blog/changelog/2025-12-16-coming-soon-simpler-pricing-and-a-better-experience-for-github-actions/

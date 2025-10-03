---
title: "Claude Code 2.0.0 のメジャーアップデートについて"
emoji: "💫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [claudecode, anthropic, claude, ai, zennfes2025ai]
published: false
---

:::message
10/3時点のClaude Code v2.0.0 - v2.0.5の内容です
:::

Oikonです。普段はAIツール、特にClaude Codeで遊んでいます。

Claude Codeが2.0.0にメジャーアップデートされました！2025年5月23日の1.0.0以来のメジャーアップデートです。この間にClaude Codeは1.0.126までアップデートを重ねています。

https://x.com/oikon48/status/1972845004206096535

今回はXでまとめたポストからClaude Code 2.0.0を使ってみて、個人的に数日使ってみて分かったアップデートもあったので、それも含めつつまとめてきます。

## アップデートの概要

![alt text](/images/claude-code-2/changelog.png)

Claude Code 2.0.0の変更点です：

- VSCode拡張のネイティブ化
- アプリUI/UXの全体的な刷新
- `/rewind` コマンド: 会話の巻き戻し
- `/usage` コマンド: プラン制限の確認
- Tabキーでthinkingをトグル（セッションで永続化）
- Ctrl-Rで履歴検索
- config commandの追加（Unshipped）
- Hooks: PostToolUseエラーの削減
- Claude Code SDK → Claude Agent SDKに名称変更
- --agentsフラグでサブエージェントを動的追加

これらについて試した内容をまとめます。

CHANGELOG.md:

https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md

## VSCode拡張のネイティブ化

![VSCode拡張のモード](https://pbs.twimg.com/media/G2D0YRRbMAA6F4f.jpg)

VSCode拡張が大きく刷新され、CLIの延長線上からCursorやCodexのようなチャットウィンドウ形式になりました。[VSCodeのマーケットプラレスからインストール](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code)できます。もちろんVSCodeのフォークであるCursorなどでもインストールすることができます。

**リリース直後の2.0.0では、日本語変換時にEnterを押すとそのままチャットが送信されてしまう不具合がありましたが、2.0.5時点では解消されています**。

左上のPast Conversationsから過去のチャット履歴にアクセスできます。Claude Code VSCodeとClaude Codeのターミナルのチャット履歴と同期されています。

チャットモードは以下の３つを、左下のボタンもしくはShift + Tabで切り替え可能です

- Ask before edits: 編集前に確認を行う
- Edit automatically: 基本的な編集確認を行わない
- Plan mode: プランモード

Ask before editsの場合、ファイル編集をする場合に確認を行います。

![Ask before editsの例](https://pbs.twimg.com/media/G2D15XBaIAAE-89.jpg)

右下の`/`ボタンを押すといくつかメニューが出現します。

- New conversation
- Resume conversation
- Clear conversation
- Mention a file
- Attach file
- Select model
- MCP
- Login

![スラッシュコマンド](https://pbs.twimg.com/media/G2D16KTaMAEGBY1.jpg)

上記に加えて、デフォルトのスラッシュコマンド・カスタムスラッシュコマンドもClaude Code CLI同様に使用可能です。

![Subagentの確認](https://pbs.twimg.com/media/G2D2hRtbkAAPXOV.jpg)

注意点としては、Claude Code CLIは`@`でSubagentも指定できましたが、**Claude Code VSCodeはファイル選択のみしか2.0.0の時点ではできず、Subagentの直接指定はできませんでした**。

## `/rewind`コマンド: 会話の巻き戻し

![/rewindの例](https://pbs.twimg.com/media/G2D3Vc2awAAmInF.jpg)

巻き戻しが可能な`/rewind`コマンドが追加されました。Escを2回押すことでも起動します。

会話の履歴を表示して、選択した位置に戻ることができます。戻る際のオプションは３つあります：

- Restore Code and Conversation
- Restore Conversation
- Restore Code

コードと会話の巻き戻しを柔軟に選択可能です。これで試行錯誤がしやすくなりました。

![オプション選択](https://pbs.twimg.com/media/G2D3nLlbgAAeKRh.jpg)

## `/usage`コマンド: プラン制限の確認

![/usageの表示](https://pbs.twimg.com/media/G2D4olqbMAAvBAv.jpg)

`/usage`コマンドで使用量を確認できます。表示されるのは：

- Current session: 現在の5時間枠のセッション使用量
- Current week (all models): 週次のセッション使用量（すべてのモデル）
- Current week (Opus) 週次のセッション使用量（Opus）

これでレートリミットまでの使用量をチェックできるようになりました。Tabキーで`/status`や`/config`画面に遷移する変更も追加されました。

記事執筆時点ではOpusは、今までよりも早く使用上限に到達してしまう印象です。Sonnet 4.5を適宜使用する方がいいかもしれないです。

## Tabキーでthinkingをトグル（セッションで永続化）

![ctrl+r](/images/claude-code-2/think.gif)

TabキーでThinking modeを簡単に切り替え可能になりました。セッション間で永続化されます。

従来の"think"や"think hard"などのマジックワードが不要になります。日本語の「考えて」なども考える必要がなくなりました。

調べたところ、**Thinking modeがオンの時は`ultrathink`（31,999 token）状態になるようです**。`ultrathink`は相変わらずマジックワードとして扱われ、Thinking modeのトグルに関わらずThinking modeがONになります。

## Ctrl + Rで履歴検索

![ctrl+r](/images/claude-code-2/ctrlr.gif)

Ctrl + Rを押すと過去のプロンプトを検索・再利用可能になりました。

直近の履歴から表示され、繰り返し押すと遡れます。プロンプトの使い回しが格段に便利になりました。

## Claude Code SDK → Claude Agent SDKに名称変更

Claude Code SDK から Claude Agent SDKに名称が変更されています。

https://docs.claude.com/en/api/agent-sdk/overview

余談ですが、AnthropicからClaudeに最近ドメインを変えており、Claude ファミリーとして統一したいように思います。

![anthropic2claude](/images/claude-code-2/anthropic2claude.png)

## --agentsフラグでサブエージェントを動的追加

![--agentsの例](https://pbs.twimg.com/media/G2EB-tqbYAAygmM.jpg)

起動時に`--agents`フラグでサブエージェントを追加可能になりました。例えば：

```bash
claude --agents '{"reviewer":{"description":"Use after writing code to review changes","prompt":"You are a meticulous code reviewer. Focus on bugs and regressions.","tools":["ReadFile","Search"]}}'
```

これで`@reviewer`のようなカスタムエージェントを動的に使えます。

https://docs.claude.com/en/docs/claude-code/cli-reference#agents-flag-format

## 選択できるモデルの変更

![モデルの選択](https://pbs.twimg.com/media/G2D_LYTaAAE37m9.jpg)

`/model`コマンドで選べるモデルが変更されます：

- Default: Sonnet 4.5
- Opus: Opus 4.1
- Sonnet (1M context) (解放されているMax 20xユーザーのみ)

Opus-Plan mode（PlanモードはOpus、実行はSonnet）はなくなったようです。

## CONFIGの内容の変更

![configの追加](https://pbs.twimg.com/media/G2ED1zcbwAA0EBp.jpg)

`/config`で「Rewind code (checkpoints)」を設定可能です。

falseにすると前述の`/rewind`時にコードのrestore確認がスキップされ、会話のみがrestoreされます。

## まとめ

今回はClaude Codeの久々のメジャーアップデート 2.0.0の内容についてまとめました。

今までのUI・使用感・できることが大きく変わったので、実際に使ってみてください。個人的には前よりさらに使いやすくなっている印象です

## Xフォローしてくれると嬉しいです

宣伝ですが、今度の2025/10/17のClaude Code Meetup Tokyoに登壇します。良かったら聞いてみてください。

https://aiau.connpass.com/event/369265/

Xでも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

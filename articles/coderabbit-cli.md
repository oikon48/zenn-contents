---
title: "CodeRabbit CLIのレビューとClaude Codeとの統合"
emoji: "🐇"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [coderabbit, codereview, cli, claudecode, zennfes2025ai]
published: true
published_at: 2025-09-20 18:03
---

Oikonです。

先日、CodeRabbitから**CodeRabbit CLI**がリリースされました。

最近はClaude Code, Codex CLI, Gemini CLIといったCLIツールの潮流が広がっていますが、CodeRabbitも例に漏れずCLIツールをリリースしてきました。

他のCLIツールと異なる点は、**CodeRabbit CLIはコードレビューに特化したCLIツール**であることです。もともとCodeRabbit自体、GitHubやGitLabなどのプルリクエスト（以下 PR）でコードレビューを行うツールのため、コードレビュー特化のツールとしてリリースしてきたのは納得の流れです。

CodeRabbit CLIツールの概要は以下の通りです：

- Review 専門CLIツール
- git 前提
- レートリミットあり
- ヘッドレスでの呼び出しがメイン
- Claude Code, Codex, Cursor統合が可能

この記事では**CodeRabbit CLIツールの概要と、Claude Codeをはじめとした他のCLIツールとの統合方法**を紹介します。

https://x.com/oikon48/status/1968264364592623888

この記事は上記のXのポストをベースにさらに詳しい話をします。

## CodeRabbitの概要

### PR ReviewをするCodeRabbit

![CodeRabbit AI commenting on a GitHub pull request](/images/coderabbit-cli/coderabbit-github.png)

CodeRabbitは、GitHubのPR上で自動的にレビューをしてくれるAIツールとして知られています。

近年はAI開発ツールによって開発プロセスが高速化している一方で、人間によるコードレビューがボトルネックになりつつあります。その中でCodeRabbitは、**AIによるコードレビュー特化の機能を提供**しています。CodeRabbitにFirst Reviewをしてもらうことで、開発者のコードレビューの負荷を減らすことが可能です。

またPR上で`@coderabbitai`を使って質問やコメントをすることで、レビュワーと対話するようにレビュープロセスを進めることもできます。

Publicリポジトリでは無料で利用ができるので、まだ使ってみたことがない方は使ってみてください。

https://www.coderabbit.ai/

## CodeRabbit CLI

**CodeRabbit CLIは、2025年9月16日にリリースされたレビュー特化のCLIツール**です。

https://x.com/coderabbitai/status/1967956147601895803

CodeRabbit CLIは、GitHub PRレビューの性能をそのままCLIツールに落とし込んでローカルプロジェクトで使用できます。

インストール方法は以下の手順で行います。内容は以下の公式ドキュメントを参考にしています。

https://docs.coderabbit.ai/cli/overview

### 1. インストール

![Terminal screen showing CodeRabbit CLI installation process](/images/coderabbit-cli/install.png)

```bash
curl -fsSL https://cli.coderabbit.ai/install.sh | sh
```

### 2. shellのリスタート

```bash
source ~/.zshrc
```

### 3. 認証

```bash
coderabbit auth login
```

初回のみブラウザ経由での認証が必要です。

また`coderabbit`は`cr`というaliasを使用することも可能です。

CodeRabbit CLIには、2つの使い方があります:

- **CodeRabbit CLI単体での利用**
- **他社CLIツールとCodeRabbit CLIの併用**

それぞれについて詳しくみていきます。

### CodeRabbit CLI単体での利用

まずCodeRabbit CLI単体での利用について紹介します。

前提として**git管理されているプロジェクトディレクトリでないと起動しません**。さらに言うと、**プロジェクトディレクトリ内に差分が存在しなければ起動してもあまり意味がありません**。

もともとCodeRabbitというツールがgitの差分を参照して、コードレビューを行うツールであるためだと思います。

git管理されているプロジェクトディレクトリで、`coderabbit`もしくは`cr`というコマンドを実行すると、以下の画像のような起動画面が立ち上がります。

![CodeRabbit CLI startup screen showing project information](/images/coderabbit-cli/coderabbit-cli.png)

画像からわかるように、現在のブランチやコード変更をgitから取得しています。

起動画面から`Enter`を押すと以下の解析画面になります。

![CodeRabbit CLI code analysis in progress screen](/images/coderabbit-cli/coderabbit-cli2.png)

解析画面では特にやることはありません。解析が終わるのを待ちます。

![CodeRabbit CLI review results with code comments and suggestions](/images/coderabbit-cli/coderabbit-cli3.png)

コードの解析が全て完了するとレビュー結果を出してくれます。上記の画像のように、問題のあるファイル名とレビュー結果が表示されます。

レビューで変更するべき点があれば、

- `a`: Apply suggestion (提案の受け入れ)を行う
- `c`: Copy Prompt to Fix with AI (AI変更用のプロンプトをコピーする)
- `r`: Reject comment (変更提案を拒否する)

などをすることが可能です。

もともとGitHub PRのCodeRabbitは、PR上での変更の受け入れやAIツールへの変更プロンプトを提供していたので、その機能がそのままCLIツールでも使える印象です。

ここまでCodeRabbit CLI単体での利用方法を紹介してきましたが、CodeRabbitが使って欲しい機能としては次に紹介する**他社CLIツールとCodeRabbit CLIの併用**だと思います。

### 他社CLIツールとCodeRabbit CLIの併用

CodeRabbit CLIは、他社CLIツールとの統合を前提に作成されています。

![Works with integration logos: Claude, Codex, Gemini, and Cursor](/images/coderabbit-cli/claude-rabbit-0.png)

- Claude Code
- Codex CLI
- Gemini CLI
- Cursor

上記に限らず、あらゆるAPIツールと統合が可能です。

CodeRabbit CLIは、他社AIツールと統合するためにヘッドレスモードでの呼び出しを使用します。モードは主に2種類あります

- `--prompt-only`: AIツール連携向け、簡便
- `--plain`: CIログなど向け、詳細

ツールのAIエージェントに、これらのモードオプションをつけたCodeRabbit CLIの呼び出しを実行させることで、**AIエージェントのコンテキストにレビュー内容を直接反映させることが可能になります**。

![CodeRabbit CLI integration workflow diagram showing code review process](/images/coderabbit-cli/integ.png)

たとえば以下のようなClaude Codeのプロンプトとして渡すような使い方ができます。

```bash
〇〇の機能を実装して。coderabbit --prompt-onlyをバックグラウンドタスクで実行してレビューを受けること
```

Claude CodeのようなBackground Taskが使用できるAIツールであれば、裏でCodeRabbit CLIにレビューをさせつつ実装を進めて、CodeRabbit CLIのレビュー結果を即座にコードベースに反映することが可能になります。

![Claude Code running CodeRabbit CLI as a background task](/images/coderabbit-cli/claude-rabbit.png)

上記の画像では、Claude CodeのBackground TaskとしてCodeRabbit CLIが実行されていることがわかります。Claude Code以外のAIツールでも、プロンプトでCodeRabbit CLIをヘッドレスで呼び出すように指示すれば同じようなことが可能です。

CLIツールとの統合は以下の動画がわかりやすいので、よければ参考にしてみてください。

https://youtu.be/IqBKf4u5MtA?si=VQEauThlqCOk9OyN

### 実際にCodeRabbit CLIを使用してみた所感

CodeRabbit CLIを実際にAIツールと併用してみました。筆者はClaude Codeをメインに使用しているのと、CodeRabbitの公式動画もClaude Codeとの統合を紹介しているため、Claude Codeでの利用です。

Claude CodeでCodeRabbit CLIを利用する方法をいくつか試しました。

1. プロンプト内で`coderabbit --prompt-only`を指示する
2. カスタムスラッシュコマンド化する
3. SubagentにCodeRabbit CLIを呼び出してもらう

1の方法は公式が動画でも紹介していた手法です。Claude Codeの実装の最中に、CodeRabbit CLIにバックグラウンドでレビューしてもらうことができます。**`coderabbit --prompt-only`をいちいちプロンプトに打つのは面倒なので、辞書登録するか`CLAUDE.md`のプロジェクトメモリに記載しておくといい**かもしれません。

2の方法は個人的に試してみたものです。カスタムスラッシュコマンドはプロンプトの再利用・外部化が可能なので、CodeRabbit CLIのレビュープロセス自体をコマンド化してしまうのも手です。

単純にCodeRabbit CLIにレビューをお願いするなら、以下のようなカスタムスラッシュコマンドが作れると思います。

```md
# run coderabbit CLI $ARGUMENTS

Just run following in the background task. Do not execute the other commands.

If $ARGUMENTS is prompt (default)

\```bash
coderabbit --prompt-only
\```

If $ARGUMENTS is plain

\```bash
coderabbit --plain
\```

```

3の方法としてSubagentも試してみました。個人的にこちらはあまり上手くいきませんでした。Subagentもバックグラウンドタスクを走らせることは可能なのですが、Claude CodeがSubagentのタスク終了を待つケースがあったりしたため1, 2の方法をメインに使用すると思います。

他社CLIツールとの統合は、CodeRabbit CLIのヘッドレスモードで簡単に行えるのですが、CodeRabbitのレビュー終了タイミングが読めないため、Claude CodeなどのAIエージェントの実装と同時に走らせる難しさはかなり感じました。ただCodeRabbit CLI自体はBetaリリースとも書かれているので、今後はシームレスに他AIツールとの統合ができることを期待します。

![CodeRabbit CLI documentation page showing Beta version notice](/images/coderabbit-cli/beta.png)

### レートリミット

CodeRabbit CLIはレートリミットが存在します。

筆者はLiteプラン($12/Month)を使用しているのですが、頻繁に使用していると結構早めにレートリミットに遭遇しました。

レートリミットがいつ解除されるかは、`coderabbit`を起動してみると分かります。

![CodeRabbit CLI rate limit exceeded warning screen](/images/coderabbit-cli/coderabbit-cli4.png)

上記の画像のようにレートリミットが来ても、数分程度で解除されます(画像は2分7秒)。一番長い時で8分ほどでした。

毎回コードレビューをCodeRabbit CLIで実行するよりも、ある程度実装が進んだ時に実行するとレートリミットに引っかかりにくい印象です。

## まとめ

CodeRabbitがCodeRabbit CLIをベータリリースしたため、実際に使用してみました。

CodeRabbit CLIの特徴は、Claude Code, Cursorといった他社AIツールと併用することで、**AIエージェントにレビュー内容をコンテキストに直接フィードバックすることができる点**だと思います。

`coderabbit --prompt-only`でのヘッドレスモードの呼び出しは若干の手間があるものの、CLIツールゆえに他のツールとの統合がしやすいのが良かったです。

CodeRabbitは、ローカルからリモートまでレビュープロセスの中に組み込めるように意識しているようなので、CodeRabbitを使ったことがある人は利用してみるといいと思います。CodeRabbit CLIは、まだベータ版のため今後はさらに使いやすくなると思います。

![CodeRabbit integration workflow: CLI to CI/CD to Pull Request reviews](/images/coderabbit-cli/integ2.png)

## Xフォローしてくれると嬉しいです

Xでも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://www.coderabbit.ai/

https://www.coderabbit.ai/ja/cli
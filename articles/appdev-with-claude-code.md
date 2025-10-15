---
title: "iOSアプリ開発経験ゼロから３週間でグローバルハッカソンで入賞するまでにやったこと"
emoji: "💫"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

Oikonです。AIツールとよく遊んでいます。

先日、RevenueCat主催のグローバルハッカソン **Shipaton** が開催されました。

8月1日〜9月30日の期間の間に、RevenueCat決済機能付きのモバイルアプリ(iOS, Android, macOS)を開発するというタフなハッカソンでした。幸運にも私の開発したアプリが "Best Vibes Award" の３位に入賞することができました！

https://x.com/oikon48/status/1977774327878840759

iOSアプリ開発は全くしたことがなかったところから、AI開発ツールの助けを借りて3週間でリリースまで出来たことが評価されたと思っています。

この記事では、AI開発ツールをどのように活用したのかに焦点を当てつつ、試したVibeCodingを備忘録的にまとめたいと思います

## モバイルアプリを作るきっかけ

### VibeCodingCatfe in Tokyo

![shipaton](/images/appdev-cc/shipaton.png)

2025年7月29日〜8月1日に開催されたRevenueCatのイベント **VibeCodingCatfe in Tokyo** で登壇の機会をいただいたことが、Shipatonに参加するきっかけでした。このイベントはもともとグローバルハッカソンShipatonのキックオフイベントの位置付けでした。

Shipatonの概要は以下の通りです：

- 主催：RevenueCat主催のモバイルハッカソン
- 賞金：賞金総額 $350,000以上
- 参加人数：2025年は55,457人が登録、812件のプロジェクトが提出。
- 条件：8月1日-9月30日にiOS、Android、Mac向けアプリをストアに公開する。収益化にRevenueCat SDKを統合する。

Shipaton 2025の入賞カテゴリは以下の通り：

![shipaton category](/images/appdev-cc/shipaton-category.png)

https://revenuecat-shipaton-2025.devpost.com/

### 筆者のモバイルアプリ開発経験

筆者は、**今回のモバイルアプリを開発するために必要なスキルセットを全く持っていなかった**といっても過言でなかったと思います笑

- モバイルアプリ開発経験なし
- XcodeはMacの初期設定時に最初に削除するもの
- Swift, Kotlin, Flutter, ReactNative分からん

以前からモバイルアプリ開発をやってみたいなーとは思ってはいても、他にやることがあることを言い訳にしていました。今回Shipatonという締め切りが決まっているイベントに参加することで、無理矢理にでも開発の手を進めることができたと思います。締め切りを作るの大事。

## 開発期間と制約

![github history](/images/appdev-cc/github-history.png)

Shipatonの開催期間は8月1日〜9月30日の２ヶ月間ありましたが、私は仕事の都合で9月6日までまともにスタートを切ることができませんでした。３週間ほどしかない中で今までリリースしたこともないモバイルアプリを開発する際に、いくつか制約を設定しました。

- iOSのみにリリース
　- 未経験のリリース作業をマルチプラットフォームでやるのは無理
- SwiftUIを選択
  - AIツールはDesign to Codeについて、ネイティブ言語の方が再現性が高い印象
- ユーザーデータは集めず基本的にローカルデバイスで完結する
  - ユーザーデータを扱う・外部サービスを利用すると審査が厳しくなる

## 実際の開発

(図)

9月6日開発スタート
9月24日App Store提出
9月27日ファーストリリース

備考：
Claude CodeやAIツールのリリースタイミング
本業の仕事もしつつ平日4-5時間、土日祝8-12時間くらい

## Claude Codeは最高の相棒

Claude Codeは2025年3月頃から使い始めて以降、基本全てのリリースノート（CHANGELOG）に目を通して機能を試すくらい愛用しているAIツールです。

Claude Codeの開発メンバーは"Claude is Horse, Claude Code is Harness"と言っていましたが、まさにその通りだと思います。Claude Codeという手綱の使い方次第で、Claudeという馬の性能は大きく変わると思います。

今回は私がiOSアプリ開発の際にお世話になった、Claude Codeの機能や使い方を紹介したいと思います

### 開発環境 （VSCode）

iOSアプリ開発者はXcodeを使って開発をすると思いますが、私はシンプルなVSCodeで開発を進めました。大きな理由としてはXcodeという開発環境の学習に割く時間が無かったためです。Xcodeは`xcrun`や`xcodebuild`などのコマンドラインツールを提供しているため、シミュレータの起動やビルドもコマンドラインで実行できます。

それ以外にもClaude CodeはIDE接続することでmcp__ide__getDiagnosticsが使用できるようになります。VSCodeなどのIDEから、コードのlint（静的解析）や診断情報（エラー、警告、提案など）を直接引き出すことができるためVSCode推奨（Serenaでも良い）

![alt text](image.png)

設定周りは以下の記事が参考になりました

https://zenn.dev/solar/articles/6a2c9ffaf9fe16

### output-styleを活用したスキルギャップ埋め

今回したSwiftは筆者は全く学習したことが無かったため、まず何をしているかを理解する必要がありました。

Claude Codeの`/output-style`には**Learningモード**があり、タスクごとにInsightを教えてくれます。Claude Codeを複数立ち上げた状態で、少なくとも1つは実装、もう一つは何をやっているかについての解説と学習しました。つまり、**アプリの実装と並行して学習も行うことで限られた時間内でアプリ開発をしました**。

### コンテキスト管理

個人的には以下を実践しています

- CLAUDE.mdは助長になりがちなので見直す(Local, Global共に)
- 不要なコンテキストは入れない (`/clear` vs `/compact`)
- 不要なMCPサーバーは使わない
- 情報はドキュメント化、必要に応じて挿入する (Planモード)
- Subagentsを積極的に活用する

/contextコマンドが実装されたことによりコンテキスト占有率が分かるようになったので確認推奨。

### 実装の一連の流れ

1. 要件定義ドキュメントの作成 (Planモード + ☑️付きドキュメント化)
2. Subagentを使って実装する
3. 複数のSubagentを使ってレビュー・評価・フィードバック
4. 複数のAIツールでPRレビュー
5. Merge

### Subagentの活用

Subagentの活用はClaude Codeにおいて非常に重要だと感じています。

まずコンテキストウィンドウの観点ですが、SubagentはClaude CodeのMain agentとは独立したコンテキストウィンドウを持ちます。実装をSubagentに任せることで、Main agentをコンテキストウィンドウが汚染されにくいです。

Subagentを使いこなす個人的なコツとしては、

- ドキュメントで参照させる
- タスク粒度を大きすぎないようにする
- 明確な役割を持ったSubagentを作成する（Description・ツールで用途を限定）

### Subagentの詳細

#### Implementor Subagent

Mainのエージェントに実装を任せてもいいですが、コンテキストウィンドウの観点から実装用のSubagentを用意しました。MCPサーバーはContext7を与え、実装の際にSwiftドキュメントを参照させつつベストプラクティスで実装するように指示しています。Brave-Search MCPツールも与え、問題に直面した時にこちらも参照するようにしています。

#### Validator Subagent

Context7 MCPツールを提供しています。またREAD権限しか与えていないため、不用意に実装を始めないように制限をしています。READ onlyのSubagentは変更によるConflictが発生しないため、積極的に複数（4つ以上）Subagentsを使用するようにワークフローを組んでいます。

#### Archtect Subagent

コンポーネントのアーキテクチャの観点から分析するSubagentです。Subagentは役割を分けた方がいいと感じているので、全体を見る意味でArchtect Subagentも呼び出し、フィードバックをしています。このSubagentもREAD onlyです。

ValidatorとArchtectのREADワークフローはカスタムスラッシュコマンド化しています。Claude Codeがカスタムスラッシュコマンド実行できるようになったので、以前よりも実行タイミングが柔軟になりました。

### ビルドスクリプト・テストのフィードバックサイクル

iOSアプリのビルドは通常はXcodeからUIを通じて実行すること多いかもしれませんが、ビルドスクリプトを作成してClaude Code自身に実行させています。

実装後にビルドスクリプトを実行することをCLAUDE.mdに記載しているため、Claude Codeはビルドスクリプトを実行し、仮にビルドエラーが発生すれば、Claude Codeのコンテキスト内にビルドログが残るので、そのログをフィードバックして修正作業を再開します。これにより実装からビルド完了までの作業を割り込みなく行うことができます。

作成した主なスクリプトは以下の通りです：

- ビルド
- シミュレータ
- 実機インストール

単体テストもClaude Codeのワークフローで実行するようにしています。できるだけフィードバックループをAIだけで完結させて、人間は全体の理解とワークフローを整えることに専念します。

### レビュープロセス

個人的にコードレビューはClaude Codeで完結するよりも、他のツールを利用する方がいいと思っています。AIモデルによってコードレビュー内容が違うため、複数の視点でのレビューを行い、妥当な変更だけを蒸留するワークフローを実施しました。

使用したAIツールは以下の通り：

- Claude Code `/review`, `/security-review` command
- Codex CLI `/review`
- CodeRabbit, CodeRabbit CLI (`cr --plain > cr-review.md`)

それぞれのレビュー結果をドキュメントで出力し、1つのドキュメントに複数のドキュメントをまとめます。

### スマホ単体での開発

限られた時間の中でも本業やプライベートの用事もあったため、開発に専念できない日もあります。その時はスマホ単体で開発できるように工夫をしました。

1a. GitHubでIssueを作り、@claudeで変更を作成
1b. Codex Cloudで変更作成
2. 1a/b からPRを作成
3. Xcode CloudでBuild、Test Flightへ自動配信、モバイルで確認
4. PRでCodeRabbitによりレビューされる（必要に応じてClaude, Codexも）
5. レビューの妥当性を検討、必要に応じてClaudeで修正、人間もレビュー
6. 更新をTest Flightで配信、モバイルで確認
7. 4~6を繰り返す
8. 変更が完成したらMainへMerge

GitHubとTest Flightの配信を使用すればスマホだけで軽微な開発・修正作業ができます。

### 開発フローのまとめ

(図)

### ccusage

今回の開発期間のccusage：

（図）

余談ですが、ccusageのStatuslineオススメです。簡単に設定できて、モデルやコンテキストの使用量をざっくりと把握できます。

## まとめ

今回はClaude CodeをメインツールとしてiOSアプリ開発を実施しました。

Shipatonの"Best Vibes Award"をいただけたということで、Vibe Codingを体現できたのではないかと思っています。

今後も「どのAIツールがいいか」論争は起きると思いますが、自分の手に馴染む使い慣れたツールを使うことで自分の能力の拡張を出来るはずです。AIツールの普遍的なスキルは抑えつつ、AIのハーネスとしての開発ツールをうまくコントロール出来ることが重要だと思っています。

## 参考文献

https://zenn.dev/solar/articles/6a2c9ffaf9fe16

https://www.nathanonn.com/how-a-read-only-sub-agent-saved-my-context-window-and-fixed-my-wordpress-theme/

https://zenn.dev/solar/articles/6a2c9ffaf9fe16
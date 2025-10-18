---
title: "iPhoneだけでiOSアプリ開発するワークフロー"
emoji: "📱"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [claudecode,codex,coderabbit,xcode,zennfes2025ai]
published: true
published_at: "2025-10-18 18:06"
---

Oikonです。普段はAIツール、特にClaude Codeで遊んでいます。

先日、[Claude Code Meetup Tokyo](https://aiau.connpass.com/event/369265/)があり登壇の機会をいただきました。主催は[AIエージェントユーザー会（AIAU）](https://aiau.connpass.com/)さまと[AI駆動開発さま](https://aid.connpass.com/)です。

ちなみに登壇内容は以下のスライドです：
**Claude Codeを駆使した初めてのiOSアプリ開発 ~ゼロから3週間でグローバルハッカソンで入賞するまで~**

https://speakerdeck.com/oikon48/claude-codewoqu-shi-sitachu-metenoiosapurikai-fa-zerokara3zhou-jian-degurobaruhatukasonderu-shang-surumade

登壇の中で**iPhone単体で開発できるように工夫**について話したところ、それなりに反響があったようなので記事にしようと思いました。

https://x.com/_nogu66/status/1979134233601020277

モバイルアプリ開発者の方々には既知の情報も多いと思いますが、筆者のモバイルアプリ開発歴は1ヶ月未満ということで、ご容赦いただければ幸いです。

## 使用したツール・サービス

今回、利用したツール・サービスは以下の通りです：

- GitHubモバイル版（Safariとかでもできる）
- Claude Code GitHub Actions
- Codex Cloud
- CodeRabbit (Optional)
- Xcode Cloud
- TestFlight

ClaudeとCodexは片方だけでも実現できます。個人的には複数のAIエージェントに相互フィードバックさせるのが好きなので、複数のAIサービスを採用しています。

## ワークフロー全体像

![iPhone単体での開発ワークフロー](/images/dev-from-mobile/iphone-workflow.png)

実際に運用しているワークフローです。以下の5つのステップで構成されています

1. Create PR(プルリクの作成): Claude Code GitHub Actions または Codex CloudでPRを作成
2. Review feedback(レビュー): CodeRabbitが自動的にコードレビューを実施(Claude, Codexも可)
3. Provide fix(変更): @claude plz fix または @codex plz fix で修正を提供
4. Build(ビルド): Xcode Cloudで自動ビルド
5. Distribute(配信): TestFlight経由でアプリを配信・テスト

それぞれのステップについて詳しく紹介します

### Step 1. Create PR（Claude Code GitHub Actions / Codex Cloud）

iPhoneから開発を開始するには、**Claude Code GitHub Actions**または**Codex Cloud**を使用しました。

**方法１: Claude Code GitHub Actions**

Claude Code GitHub Actionsは、GitHub上のIssueやPRから`@claude`をメンションすることでClaude Codeを起動できる機能です。詳しいことは公式ドキュメントに書いてあります。

https://docs.claude.com/ja/docs/claude-code/github-actions

基本的にはClaude Codeで`install-github-app`を一度実行してGitHubとClaudeを連携すれば良いです。

![install-github-app](/images/dev-from-mobile/install-github-app.png)

使い方はGitHub上でIssueを作成して変更計画をDescriptionに記載して、`@claude`をつけて変更を作成するようにお願いします。ClaudeはGitHubに自動でブランチを作成し、タスクを完了するとボタン一つでPull Requestを作成するところまで実行できます。

![claude-github](/images/dev-from-mobile/claude-github2.png)

**方法2: Codex Cloud**

Codex Cloudは、OpenAI CodexのCloud機能で、リポジトリのコードを読み書き・実行し、PRを作成できます。Codex Cloudは、サンドボックス化されたクラウドコンテナを起動し、指定したコードと依存関係を使ってタスクを実行します。

**Codex CloudでもClaude Code GitHub Actions同様にiPhoneからPRの作成までできます**。一度Web版でGitHub連携をして、Codexにどのリポジトリに対して権限を与えるか設定する必要があります。

https://chatgpt.com/codex

一度Webで設定をしたら、ChatGPTのモバイルアプリからCodex Cloudを起動します。サイドバーを開けばすぐに見つかると思います。

![chatgpt-code](/images/dev-from-mobile/chatgpt-codex.png =400x)

Codex Cloudの特徴として、一つのタスクに対して4つまで並列で変更を作成することができます。そのうちの一つをGitHubのPRとして作成できます。

![chatgpt-code2](/images/dev-from-mobile/chatgpt-codex2.png =400x)

両方使ってみた個人的な感想ですが、**Codex Cloudの方が実装内容が良いことが多かったです**。Claude Code GitHub Actionsは`@`メンションで簡単にGitHub上でやり取り・変更作成できる点が良かったです。

## Step 2. Review feedback (CodeRabbit / Codex / Claude)

PRが作成されると、自動的にコードレビューを実施してくれます。以下の3つのAIツールはすべて自動レビュー機能が使えます

- CodeRabbit
- Claude Code
- Codex

Claude CodeとCodexは**1. Create PR**で説明した初期設定の際に自動レビューをつけるか設定可能です。Web UIやGitHub Actionsの設定ファイルからも設定変更可能です。

CodeRabbitはレビュー専用AIエージェントです。個人的には使いやすいと感じておりかなり愛用しているので、**ファーストレビューはCodeRabbitに一任**しています。

![coderabbit](/images/dev-from-mobile/coderabbit.png)

全てのAIエージェントをレビューすることも可能ですが、3つのAIエージェントが同時にレビューするとレビュー結果が大量になりPRがてんやわんやになるので、個人的にはあまり体験が良くなかったです。

### Step 3. Provide fix

CodeRabbitがPR上でレビューをしてくれた後に、人間がざっとレビュー内容に目を通します。レビュー漏れなどがありそうだったら、追加で `@claude`、`@codex`などを使いレビュー内容の確認や追加のレビューをお願いします。

こうしてざっと変更とレビュー内容を確認できたら、妥当なものを判断してClaude(`@claude`)かCodex(`@codex`)にレビューを頼みました。この際、ClaudeやCodexは別の変更用Branchを切って変更を作成するので、その点は注意です。

![codex-fix](/images/dev-from-mobile/codex-fix.png)

上記は妥当なものを自分で判断していない悪い例ですが(笑)、大体こんな感じでPR内でAIとやり取りをしながら変更をブラッシュアップしていきます。

## Step 4. ビルド（Xcode Cloud）

モバイル端末での開発の１番のネックは、**変更が正しいものか手元で確認することが難しい点**だと思います。これはモバイルアプリ開発だけでなくWebアプリ開発も共通だと思います。もちろん、無理ではないです。

iOSアプリであれば**Xcode Cloud**を使えばPRの変更に対してワークフローを組んで自動でビルドまですることが可能です。

![xcodecloud](/images/dev-from-mobile/xcodecloud.png)

具体的なやり方は以下の記事が参考になります。

https://future-architect.github.io/articles/20250609a/

App Store ConnectのUI上でもある程度ワークフローを組むことができます。筆者のワークフローはUIからでも組むことは可能です。ワークフローを組んだ際に、PRに関連する開始条件（トリガー）を設定することで、スマートフォンだけでもBuildを発火できます。

![app-store-connect](/images/dev-from-mobile/app-store-connect.png)

- ブランチの変更
- プルリクエストの変更
- タグの変更
- 手動開始

などいくつかトリガーを設定できます。

上記のXcode Cloudのワークフローを設定することにより、**PR上の変更だけでBuildを自動で走らせて、AIツールが作成した変更が正しくBuildされるかを確認できます**。仮にBuildが失敗したらApp Store ConnectのBuildログをPR上に貼り付けてフィードバックします。大体これで直してくれることも多いです。

### Step 5. Distribute (TestFlight)

Xcode Cloudのワークフロー設定では、**ビルド成功後に自動的にTestFlightへアップロードするように設定できます**。これによりビルドだけでなく実機確認のためのアプリ配信まで完全に自動化されます。

![testflight](/images/dev-from-mobile/testflight.png)

TestFlightのアプリはAppStoreからダウンロードできます。TestFlightはテスターグループがベータリリースなどを試すことを目的で使用されることが多いですが、個人レベルでも実機確認のために利用できると思いました。

**TestFlightでの確認:**

1. iPhoneでTestFlightアプリを開く
2. 新しいビルドが利用可能になっている
3. インストールして動作確認

iPhoneから開発を始めて、iPhoneで動作確認まで完結します。**iPhoneで意図しない変更であった場合は、スクリーンショットを撮ってPR上にフィードバックをすることでそれなりの修正をClaudeやCodexは作成してくれます**。上記のサイクルを何回か繰り返して、外出中でも変更を作成できます。念の為、ローカルにPullして最終確認してからマージすることをお勧めします。

TestFlightの設定方法は以下の記事が参考になります。

https://qiita.com/spc_knakano/items/b7dff01cdb4b111eae04

:::message
Xcode cloudは**1ヶ月あたり25時間分は無料**で利用できます。
あまりにも何度もビルドをするようだと制限に引っかかるかもしれませんが、個人レベル + ビルドトリガーを適切に設定すれば問題ないと思います。
![freeplan](/images/dev-from-mobile/freeplan.png)
:::

## まとめ

![iPhone単体での開発ワークフロー](/images/dev-from-mobile/iphone-workflow.png)

改めてワークフローを再掲します。
**今回は、iPhoneだけでiOSアプリ開発を行うワークフローを紹介しました**。

**ワークフローのステップ**:

1. Create PR（Claude Code / Codex）
2. Review feedback（CodeRabbit/ Claude Code / Codex）
3. Provide fix（@claude / @codex）
4. Build（Xcode Cloud）
5. Distribute（TestFlight）

GitHubとTestFlightの配信を使用すれば、**iPhoneだけで軽微な開発・修正作業が可能**です。

## Xフォローしてくれると嬉しいです

Xでも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://docs.claude.com/ja/docs/claude-code/github-actions

https://chatgpt.com/codex

https://www.coderabbit.ai/

https://developer.apple.com/jp/xcode-cloud/

https://future-architect.github.io/articles/20250609a/

https://qiita.com/spc_knakano/items/b7dff01cdb4b111eae04

https://zenn.dev/oikon/articles/coderabbit-cli

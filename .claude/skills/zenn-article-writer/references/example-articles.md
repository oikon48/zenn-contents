# Oikonの記事の実例抜粋

このドキュメントは、Oikonの実際の記事から特徴的な部分を抜粋したものです。

## 例1: ワークフロー解説型の記事から

### タイトルとフロントマター

```markdown
---
title: "iPhoneだけでiOSアプリ開発するワークフロー"
emoji: "📱"
type: "tech"
topics: [claudecode,codex,coderabbit,xcode,zennfes2025ai]
published: true
published_at: "2025-10-18 18:06"
---
```

### 導入部分

```markdown
Oikonです。普段はAIツール、特にClaude Codeで遊んでいます。

先日、[Claude Code Meetup Tokyo](https://aiau.connpass.com/event/369265/)があり登壇の機会をいただきました。主催は[AIエージェントユーザー会（AIAU）](https://aiau.connpass.com/)さまと[AI駆動開発さま](https://aid.connpass.com/)です。

ちなみに登壇内容は以下のスライドです：
**Claude Codeを駆使した初めてのiOSアプリ開発 ~ゼロから3週間でグローバルハッカソンで入賞するまで~**

https://speakerdeck.com/oikon48/claude-codewoqu-shi-sitachu-metenoiosapurikai-fa-zerokara3zhou-jian-degurobaruhatukasonderu-shang-surumade

登壇の中で**iPhone単体で開発できるように工夫**について話したところ、それなりに反響があったようなので記事にしようと思いました。

https://x.com/_nogu66/status/1979134233601020277

モバイルアプリ開発者の方々には既知の情報も多いと思いますが、筆者のモバイルアプリ開発歴は1ヶ月未満ということで、ご容赦いただければ幸いです。
```

### 使用ツールのリスト

```markdown
## 使用したツール・サービス

今回、利用したツール・サービスは以下の通りです：

- GitHubモバイル版（Safariとかでもできる）
- Claude Code GitHub Actions
- Codex Cloud
- CodeRabbit (Optional)
- Xcode Cloud
- TestFlight

ClaudeとCodexは片方だけでも実現できます。個人的には複数のAIエージェントに相互フィードバックさせるのが好きなので、複数のAIサービスを採用しています。
```

### 全体像の提示

```markdown
## ワークフロー全体像

![iPhone単体での開発ワークフロー](/images/dev-from-mobile/iphone-workflow.png)

実際に運用しているワークフローです。以下の5つのステップで構成されています

1. Create PR(プルリクの作成): Claude Code GitHub Actions または Codex CloudでPRを作成
2. Review feedback(レビュー): CodeRabbitが自動的にコードレビューを実施(Claude, Codexも可)
3. Provide fix(変更): @claude plz fix または @codex plz fix で修正を提供
4. Build(ビルド): Xcode Cloudで自動ビルド
5. Distribute(配信): TestFlight経由でアプリを配信・テスト

それぞれのステップについて詳しく紹介します
```

### ステップの詳細解説

```markdown
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

Codex Cloudは、OpenAI CodexのCloud機能で、リポジトリのコードを読み書き・実行し、PRを作成できます。

...（中略）...

両方使ってみた個人的な感想ですが、**Codex Cloudの方が実装内容が良いことが多かったです**。Claude Code GitHub Actionsは`@`メンションで簡単にGitHub上でやり取り・変更作成できる点が良かったです。
```

### 注意事項の提示

```markdown
:::message
Xcode cloudは**1ヶ月あたり25時間分は無料**で利用できます。
あまりにも何度もビルドをするようだと制限に引っかかるかもしれませんが、個人レベル + ビルドトリガーを適切に設定すれば問題ないと思います。
![freeplan](/images/dev-from-mobile/freeplan.png)
:::
```

### まとめセクション

```markdown
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
大きい変更などは人間でのレビューが難しかったり、Claude・CodexのRemoteでの実装力が足りなかったりで上手くいかないこともあるので、上手くタスクのサイズを調整するのが良いと思います。
```

## 例2: アップデート解説型の記事から

### 導入部分

```markdown
:::message
2025年10月3日時点のClaude Code v2.0.0 - v2.0.5の内容です
:::

Oikonです。普段はAIツール、特にClaude Codeで遊んでいます。

Claude Codeが2.0.0にメジャーアップデートされました！2025年5月23日の1.0.0以来のメジャーアップデートです。この間にClaude Codeは1.0.126までアップデートを重ねています。

https://x.com/oikon48/status/1972845004206096535

今回はXでまとめたポストからClaude Code 2.0.0を使ってみて、個人的に数日使ってみて分かったアップデートもあったので、それも含めつつまとめてきます。
```

### アップデート概要

```markdown
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
```

### 機能詳細の解説

```markdown
## `/rewind`コマンド: 会話の巻き戻し（Checkpoints機能）

![/rewindの例](https://pbs.twimg.com/media/G2D3Vc2awAAmInF.jpg)

**Checkpoints**は最もリクエストが多かった機能の1つで、`/rewind`コマンドとして実装されました。Escを2回押すことでも起動します。

この機能により、Claudeが重要な編集を行う前に自動的にチェックポイントが保存され、いつでも以前の状態にロールバックすることができます。

会話の履歴を表示して、選択した位置に戻ることができます。戻る際のオプションは３つあります：

- Restore Code and Conversation
- Restore Conversation
- Restore Code

コードと会話の巻き戻しを柔軟に選択可能です。これで試行錯誤がしやすくなりました。
```

## 例3: ツール連携型の記事から

### 導入部分

```markdown
:::message
この記事は人力で書いてます。
:::

Oikonです。外資系IT企業でエンジニアをしています。

AIエージェントのコンテキストエンジニアリングが最近は注目を集めています。今後もしばらくはこの流れが続くんじゃないかと。

AIエージェントにコンテキストを与える仕組みのために、**知識の源泉としてObsidianを3ヶ月ほど前から試行錯誤しながら使っています**。

最近になって、ようやく自分の中でしっくり来る運用が固まってきたので、その方法を共有します。

この記事は、先日Xにポストした内容の詳細版です。

https://x.com/oikon48/status/1955918989340893275

Obsidianの運用方法は人によって全く違うと思うので、「こんな運用方法もあるのね」くらいに読んでいただければ幸いです。
```

### 運用フローの提示

```markdown
## 運用の流れ

まず以下が揃っていることを前提とします。

- [Obsidian Desktop](https://obsidian.md/)
- [Obsidian Mobile](https://apps.apple.com/jp/app/obsidian-connected-notes/id1557175442?l=en-US)(Optional)
- [Claude Code](https://docs.anthropic.com/ja/docs/claude-code/overview)

クリックで、それぞれの公式サイトに飛びます。

はじめに、Obsidianの運用の全体像を紹介します。現在の運用手順は以下の通りです。

1. **Obsidian Vaultをデバイス間で共有**
2. **Obsidian Web Clipperで記事を保存**
3. **Obsidianのフォルダ構成はシンプルに**
4. **Clippings/をClaude Codeで整理**
5. **Claude CodeとObsidian MCPサーバーを繋ぐ**
6. **Claude CodeからObsidianを活用する**

それぞれについて詳しく説明します。
```

### コマンド例の提示

```markdown
Claude CodeでこのObsidian DesktopのMCPサーバーに繋ぐには以下のコマンドを実行します。

```bash
  # "/path/to/vault" はご自身の環境に合わせて修正してください
  claude mcp add obsidian-mcp-tools -s user -- "/path/to/vault/.obsidian/plugins/mcp-tools/bin/mcp-server" --env OBSIDIAN_API_KEY=YOUR_API_KEY
```

もしくは`.claude.json`に自分で追加します。

```json
  {
    "mcpServers": {
      "obsidian-mcp-tools": {
        "command": "/path/to/vault/.obsidian/plugins/mcp-tools/bin/mcp-server",
        "args": [],
        "env": {
          "OBSIDIAN_API_KEY": "YOUR_API_KEY"
        }
      }
    }
  }
```
```

### まとめでの知見共有

```markdown
## まとめ

Obsidianが便利だと感じるまでに3ヶ月ほどかかりました。

Obsidian活用の個人的なコツは以下の通りです:

- 雑に放り込む
- AIに編集を任せる
- AIに整理を任せる
- AIに解説を任せる
- AIに引用を任せる
- 人間は文献付きの成果物だけを扱う

Obsidianは自由度が高い故に挫折してしまうことが多いですが、**AI-Firstを心掛けると上手く活用できるのではないか**と考えています。

今後は自分の考えや知識のドキュメントが資産になって、生成AIに活用させる機会がますます増えているため、Obsidianに限らずドキュメント管理は重要になると思っています。

この記事が参考になれば幸いです。
```

## フォロー誘導と参考文献

### パターン1

```markdown
## Xフォローしてくれると嬉しいです

Xでも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://docs.claude.com/ja/docs/claude-code/github-actions

https://chatgpt.com/codex

https://www.coderabbit.ai/
```

### パターン2（イベント告知付き）

```markdown
## Xフォローしてくれると嬉しいです

宣伝ですが、今度の2025/10/17のClaude Code Meetup Tokyoに登壇します。良かったら聞いてみてください。

https://aiau.connpass.com/event/369265/

Xでも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48
```

### パターン3（𝕏表記）

```markdown
## 𝕏フォローしてくれると嬉しいです！

𝕏でも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://obsidian.md/

https://docs.anthropic.com/ja/docs/claude-code/overview
```

## 文体の要点まとめ

以上の例から分かる共通の特徴：

1. **親しみやすい自己紹介**: 「Oikonです。普段は...」
2. **実体験の共有**: 登壇、ハッカソン、3ヶ月の試行錯誤など
3. **謙虚な姿勢**: 経験の浅さや読者へのリスペクト
4. **明確な構造**: 全体像 → 詳細 → まとめ
5. **視覚的補助**: 図やスクリーンショットの豊富な使用
6. **具体例の提示**: コマンド、コード、設定ファイル
7. **個人的な感想**: 「個人的には」「〜と思います」
8. **比較と選択肢**: 複数の方法を提示し比較
9. **注意点の明示**: :::message記法の活用
10. **まとめの再掲**: 要点を箇条書きで再確認
11. **自然な宣伝**: フォロー誘導とイベント告知
12. **参考文献の列挙**: リンクを全て列挙

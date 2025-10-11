---
title: "Claude・Codex・KombaiのFigma to Codeの比較"
emoji: "🎨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [figma, codex, frontend, claudecode, kombai]
published: true
published_at: 2025-10-12 07:02
---

Oikonです。

先日**AIツールでどこまでデザインを忠実に実装できるのか**というテーマで登壇する機会がありました。

その際に、以下の３つのツールについてFigmaからコードを生成してみて比較する実験を行いました。

- Claude Code + Figma MCP
- Codex CLI + Figma MCP
- Kombai

![Figma to Code](/images/frontend-ai/figma-to-code.png)

この記事では、それぞれの手法について詳しく比較していきたいと思います。

## UI/UXデザイン実装の現状と課題

最近はAIツールもデザイン力が高くなってきましたが、それでもバックエンドほど上手く実装することができない印象です。フロントエンド開発において、デザインからコードへの変換には以下のような課題を個人的に感じています。

- 再現度: デザインの意図を正確にコード化するのが難しい。AI臭のあるデザインになりがち
- 実装面: ちゃんと指示をしないとレスポンシブ対応が抜けやすい
- 知識面: ライブラリやフレームワークなどの最新情報を持っていない
- 保守性: 一貫性のあるコンポーネント設計が難しい

UI/UXはデザイナーとエンジニアの技術力無しでは、まだ納得のいく完成度になっていません。

またよくピクセルパーフェクトではないという意見も耳にします（必ずしもピクセルパーフェクトで無くともいいかもしれませんが）

現時点でデザインを比較的忠実にコードで再現できる方法は、**Figmaでデザインしてそれをもとにコードを生成する方法**だと思っています。これは画像ではAIが把握しきれないコンポーネント構造を、FigmaであればAIが認識しやすいことが理由です。

今回はFigma to Codeの観点で、大きく分けて２つの手法を紹介します。**Figma MCPを使用する手法**と**フロントエンド特化AIツールを使う手法**です。

## Figma MCPとは

Figma MCPはFigmaのデザインデータをAIが直接読み取り、コード生成を支援するツールです。Figma Desktopがあればローカルで簡単にMCPサーバーとAIエージェントを接続することが可能です。FigmaのデザインデータをAIエージェントが直接読み取って理解することができるため、一般的な画像を与えるよりもデザインを上手く実装できることが多いです。

### Claude Code / Codex CLIでの使用方法

Figma MCPをClaude CodeやCodex CLIに接続することで、Figmaデザインからコードを生成できます。Figma Desktopの設定画面から *Preferences > Enable desktop MCP server* をチェックすることで、ローカルMCPサーバーを立ち上げることができます。

![Figma MCP](/images/frontend-ai/figmamcp.png)

Figma Desktopの設定が終わった後は、以下のように設定ファイルに記述します。Claude Code、Codex CLIの両方の設定を紹介します。

**Claude Codeの設定例（`~/.claude.json`）:**

![Claude Code設定](/images/frontend-ai/figmaclaude.png)

**Codex CLIの設定例（`~/.codex/config.toml`）:**

![Codex CLI設定](/images/frontend-ai/figmacodex.png)

以下の記事が個人的に参考になりました↓

https://www.builder.io/blog/figma-mcp-server

## フロントエンド特化AIツールについて

フロントエンド特化AIエージェントはいくつか存在します。

- [Locofy.ai](https://www.locofy.ai/)
- [Anima](https://www.animaapp.com/)
- [Builder.io](https://www.builder.io/)
- [Kombai](https://kombai.com/)

この中だと、個人的にKombaiを使用しているので、今回はKombaiを使って比較してみます。

### Kombaiについて

![Kombai拡張](/images/frontend-ai/kombai-extension.png)

Kombaiはインドのスタートアップが開発したフロントエンド専用AI Agentツールです。Product Huntでも１位になったことがあり、2025年8月に正式リリースされました。

- プロジェクト全体を解析
- 技術スタック（フレームワーク・ライブラリ）を設定可能
- 仮想環境内で実行し、Acceptするまでlocalに変更を加えない
- VSCode/Cursor/Windsurf/Traeの拡張として利用可能

Kombaiについては以前に詳しく記事を書いているので、興味がある方はこちらもご覧ください：

https://zenn.dev/oikon/articles/kombai-frontend

## 実験：3ツールでFigmaデザインをコード化

同一のFigmaデザインを使用し、以下の3つのツールで比較実験を行いました。

### 検証方法

1. **Claude Code + Figma MCP**
2. **Codex CLI + Figma MCP**
3. **Kombai**

共通プロンプト:

```bash
FIGMA_DESIGN_URL このFigma Designを実装して
```

FIGMA_DESIGN_URLは、Figma DesignのURLから *Copy/Paste as > Copy link to selection* から取得できます。

![alt text](/images/frontend-ai/figmaurl.png)

今回は以下のFigma デザインをサンプルとして使用しています。

![デモデザイン](/images/frontend-ai/demo.png)

[Figma Community - Portfolio Design](https://www.figma.com/community/file/1170206889562959306)

## 実験結果

### 1. Claude Code + Figma MCP

![Claude Code結果](/images/frontend-ai/claudemcp.png)

Claude Code + Figma MCPの組み合わせでは、Next.js 14のApp Routerを使用し、13個の独立したコンポーネントに分割された実装が生成されました。ClaudeはSonnet 4.5を使用しています（Opusは私のレートリミットの関係で使えていません）

**技術スタック:**

- Next.js 14（App Router）
- TailwindCSS 3.4
- TypeScript

Claude Codeの実装では、機能ごとにコンポーネントが分割されていました。またグラデーション背景やblur効果といったデザインの細かなニュアンスも再現されていた印象です。

一方で、レスポンシブ対応については固定px値を使用している箇所があるため部分的な対応に留まっていた点がイマイチでした。

### 2. Codex CLI + Figma MCP

![Codex CLI結果](/images/frontend-ai/codexmcp.png)

Codex CLI + Figma MCPでは、Next.jsコンポーネントと純粋なHTMLの両方を生成していました。

**技術スタック:**

- Next.js 14（App Router + HTML混在）
- カスタムCSS（BEM風命名規則）
- TypeScript

Codex CLIの実装は、`<dl>`、`<dt>`、`<dd>`タグなどを活用したセマンティックHTMLを徹底しており、文書構造が明確です。またアクセシビリティに配慮した実装もしていました。レスポンシブ対応をしていた点も良かったです。

HTMLとReactコンポーネントが混在している点が個人的に少し気になりました。

### 3. Kombai

![Kombai結果](/images/frontend-ai/kombairesult.png)

Kombaiはフロントエンド特化型AIとして、Material UIコンポーネントを活用し、最もデザインに忠実な実装を生成しました。実際のプロファイル画像も含め、細部まで再現されています。

**技術スタック:**

- React 18 + Vite
- Material UI (MUI) v5
- TypeScript
- Emotion（CSS-in-JS）

Kombaiの実装は、レスポンシブ対応はMUIのブレークポイントシステムを使用して実装されていて良かったです。3つのツールの中でデザイン再現度が最も高い印象です。

Material UIへの依存性が強い点は少し気になりました。

### ３ツールの比較

| 項目 | Claude Code + MCP | Codex CLI + MCP | Kombai |
|------|------------------|-----------------|---------|
| 技術スタック | Next.js 14, TailwindCSS | Next.js 14, CSS | React 18, MUI |
| ファイル構成 | 13コンポーネント | 12コンポーネント + HTML | 9セクション + 共通コンポーネント |
| デザイン再現度 | 約65-70% | 約70-75% | 約75-80% |
| レスポンシブ対応 | 部分的（固定px値） | 良好 | 良好 |
| 画像処理 | プレースホルダー | プレースホルダー | 実画像使用 |
| アクセシビリティ | 基本的 | 優秀（aria属性） | 良好（MUI標準） |

## まとめ

今回は**Figma to Codeの観点で3つのツールを詳細に比較しました**。

現時点ではピクセルパーフェクトな実装は難しいものの、いずれのツールでも忠実度70%以上の再現が可能となっています。フレームワークやライブラリの整合性については、まだエンジニアによる評価と調整が必要な段階です。

今回の実験から分かったAIツールによるデザインからコード実装は、簡単な実装であればかなりの精度、叩き台としては十分という印象でした。

## 余談

登壇する話をしたらKombaiがクーポンコードを発行してくれました。[XのDM](https://x.com/oikon48)で連絡してくれればクーポンコードを共有します (80アカウント分有効らしいので、オーバーしたら使えなくなります。すみません)

https://x.com/oikon48/status/1976498148765004162

## Xフォローしてくれると嬉しいです

Xでも情報発信しているので、フォローしていただけると励みになります！

[X @oikon48](https://x.com/oikon48)

## 参考文献

- https://www.builder.io/blog/figma-mcp-server
- https://kombai.com

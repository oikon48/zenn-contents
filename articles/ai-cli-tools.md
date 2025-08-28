---
title: "npmダウンロード数で見るAIエージェントCLIツール動向"
emoji: "💡"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [zennfes2025free, claudecode, geminicli, codexcli, ai]
published: true
published_at: 2025-08-29 07:02
---

:::message
・この記事は人力で書かれています。
・2025年8月25日までのデータを使用しています。
:::

Oikonです。外資系企業でソフトウェアエンジニアをしています。

最近はAIエージェントのCLIツールがなにかと話題になっています。

* Claude Code
* Gemini CLI
* Codex CLI
* Cursor CLI
* Open Code
* Aider
* Cline

などなど。

特にCodex CLIは、GPT-5が8月上旬にリリースされて以降、注目されている印象です。個人的にCodex CLIの勢いを実際に数字で確かめたくなり、この記事を書きました。

今回は**Claude Code、Gemini CLI、Codex CLIについて、それぞれの「npmダウンロード数」を基にグラフを作成し、AIエージェントCLIツールの動向**をまとめたいと思います。

以下のポストの詳細について、本記事で補足します。

https://x.com/oikon48/status/1960284149908136295

## 調査方法

### 調査対象のCLIツール

大手のモデルプロバイダが提供している、以下の3つのAIエージェントCLIツールを調査対象とします。

* 🟧 **Claude Code**
* 🟦 **Gemini CLI**
* 🟩 **Codex CLI**

（色はグラフで使用する色です）

余談ですが、GrokもxAIという大手のモデルですが、記事執筆時点ではCLIツールは提供していません。有志によるOSSは存在していますが、xAI公式からCLIツールはリリースされていないため今回は対象としていません。

### データ

今回は**npmダウンロード数**を用いて、各ツールの利用動向を把握したいと思います。

ダウンロード数はnpmのDownloads APIを使用し、**2025‑06‑27〜2025‑08‑25の日別ダウンロード数**を取得しました。

API出力では日付とダウンロード数がJSON形式で返されます。

例えば、Claude CodeのAPIは以下のエンドポイントで参照できます。

**Claude Code:**
https://api.npmjs.org/downloads/range/last-month/@anthropic-ai/claude-code

同様にGemini CLI、Codex CLIも記載します。

**Gemini CLI:**
https://api.npmjs.org/downloads/range/last-month/@google/gemini-cli

**Codex CLI:**
https://api.npmjs.org/downloads/range/last-month/@openai/codex

## 調査結果と考察

### 全ツール

![combine90](/images/ai-cli-tools/combined_90days_trim_color.png)

Claude Code、Gemini CLI、Codex CLIを同じグラフに載せました。

リリース順とグラフ上の順位が概ね一致しています。Codex CLIは話題になっているように見えますが、ダウンロード数だけで見るとまだClaude Codeと大きな差があり、**市場に早く出してシェアを獲得することの重要性**が分かります。

面白いのが、平日のダウンロード数が多く、週末は少ない傾向です。やはり仕事で使用している方が大半なのですね。

以降はそれぞれのAIエージェントCLIツールを個別に見ていきます。

### 🟧 Claude Code

![claudecode90](/images/ai-cli-tools/claude_code_90days_trim_color.png)

Claude Codeは2月のリリース以降フロントランナー的な存在で、今回のAIエージェントCLIツールの中ではトップのダウンロード数です。ダウンロード数も順調に伸ばしています。

8月12日に約150万/日を記録していますが、Sonnet 4の1Mコンテキストの順次解禁や、Amazon Bedrock・Vertex AI経由での利用可能化が影響した可能性が大きいと思います。8月15日頃に「Claude Codeがアホになった」という指摘が話題になりましたが、ユーザー増加が影響しているのではないかと思います。

### 🟦 Gemini CLI

![gemini90](/images/ai-cli-tools/gemini_cli_90days_trim_color.png)

Gemini CLIは6月末にリリースされて以降、Claude Codeに次ぐダウンロード数です。ただ、ピークはリリース直後のタイミングであり、その後は横ばいです。

最近、カスタムスラッシュコマンド対応などの機能が追加されました。無料である程度使えるため、CLI初心者やClaude Codeのサブツールとして使用されているイメージです。

今後、Gemini 3.0などの大型アップデート次第では右肩上がりに転じる可能性はありますが、現時点では可もなく不可もなしといったところです。

### 🟩 Codex CLI

![codex90](/images/ai-cli-tools/codex_cli_90days_trim_color.png)

Codex CLIは4月中旬にリリースされていましたが、8月になるまであまり話題になっていませんでした。8月上旬にGPT-5がリリースされ、Codex CLIの定額利用も発表されたため、一気に約2万/日を記録しました。

その後も度重なるアップデートとGPT-5の性能が評価され、じわじわユーザーが増えている印象です。現在のダウンロード数は右肩上がりです。

0.25.0からIDE統合もされたため、今後ますますシェアを伸ばすと思います。

### 余談①: 最新のグラフを見たい人へ

ちなみにTanStackというサイトで、今回作成したグラフと同じものが作れます。

https://tanstack.com/stats/npm?packageGroups=%5B%7B%22packages%22%3A%5B%7B%22name%22%3A%22%40openai%2Fcodex%22%7D%5D%7D%2C%7B%22packages%22%3A%5B%7B%22name%22%3A%22%40anthropic-ai%2Fclaude-code%22%7D%5D%7D%2C%7B%22packages%22%3A%5B%7B%22name%22%3A%22%40google%2Fgemini-cli%22%7D%5D%7D%5D&range=30-days&transform=none&binType=daily&showDataMode=all&height=731

### 余談②: 機能比較

個人的にどのツールがどの機能を備えているのか把握するために、機能比較表を作成しました。自分の手 + DeepResearchを使用して表を作成しています。参考までに。

<!-- ![compare](/images/ai-cli-tools/comare.png) -->

https://x.com/oikon48/status/1961024064786759787

## まとめとメモ

この記事では代表的なAIエージェントCLIツールの動向を備忘録的にまとめました。

現状の構図は以下のとおりと理解しています。

* Claude Code一強
* Gemini CLIが追随
* Codexがトレンドイン

npmダウンロード数はあくまで導入の勢いを示す指標にすぎないため、参考程度に見ていただければと思います。

個人的にはClaude Codeをメインで触りつつ、Codex CLIを最近は使用することが多いです。AIツールを変更するオーバーヘッドも加味しつつ、身軽に色々試せるようにしていきたいですね。

## 𝕏フォローしてくれると嬉しいです！

𝕏でも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://www.npmjs.com/package/@anthropic-ai/claude-code
https://www.npmjs.com/package/@google/gemini-cli
https://www.npmjs.com/package/@openai/codex
https://tanstack.com
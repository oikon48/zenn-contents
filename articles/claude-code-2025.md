---
title: "Claude CodeのCHANGELOGを全て追ってきて思うこと"
emoji: "💫"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [claudecode, claude, anthropic, ai]
published: false
---

Oikonです。普段はClaude Codeの話を[X(Twitter)](https://x.com/oikon48)でしています。

激動の2025年がもうすぐ終わるということで、CLaude Codeについてまとめようと思いました。個人的に2025年はAIエージェントが大きく飛躍した年であり、**私にとってはClaude Codeの年でした**。

[Claude CodeのCHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)の更新自体は合計176回ありましたが、少なくともv1.0.xからは全て確認・検証をしています。

| バージョン系列 | リリース数(2025/12/30時点) |
|----------------|------------|
| v0.2.x         | 37         |
| v1.0.x         | 82         |
| v2.0.x         | 57         |
| 合計           | 176        |

1年を通してClaude Codeと向き合ってきて、自分なりに今年感じたことを整理したいと思います。あくまで私個人の考えであることをご留意いただければ幸いです。

## 初めてClaude Codeを使った衝撃

### Claude Codeに出会う前

私がClaude Codeを触ったのは、ベータリリースの2025年2月...ではなく3月でした。

当時はGitHub CopilotとCursorに手を出しつつ、ChatGPTやClaudeのWeb版でコードのことを聞いて、生成されたコードで良いものをコピペして手元で修正するような開発をしていました。今考えると開発方式だけでも今年一年で大きく変わったなと思います。

この時点でもClaudeのWeb版はGitHub連携ができており、チャットのやり取りでコードベースを読んで回答してくれていたと記憶しています。当時のClaude 3.7のコーディングの質は、他のAIモデルと比べて良いと個人的に感じていました。

### Claude Codeとの邂逅

そんな中で2025年3月にClaude Codeというプロダクトに出会います（この頃にはまだXのアカウントを動かしていなかったため、当時のポストがなくて残念）。きっかけは誰かのSNSポストで動かしているところを見たと記憶しています。IDE型のCursorやWindsurfが注目されている中で、CLIツールということで目に留まりました。

**初めてClaude Codeを触ってみた時にはとても感動しました**。今までのAIツールはあくまで並走してコーディングをしてくれていましたが、当時のClaude Codeでも自律的にコーディングをしていき、複数のコンポーネントを単独で作成させることができました。

AIエージェント自らコードベースを探索していき、必要な情報を集めて実装していく姿を見た時に、コーディングの在り方が変わると感じたのを覚えています。

4月以降もClaude CodeをAPI従量課金で使いながら、CLIツールでのコーディングに慣れていきました。Xで活動し始めた4月頃は、Claude Codeを賞賛しCodex CLIも使ってみたいなどのポストをしていました。余談ですが、当時のCodex CLIは正直あまり使えるものではなかったですし、Codex CLIがあったのを知らない人も多い印象です。GPT-5が登場した際にCodex CLIも大きく化けたのは、後述のClaude Codeの正式リリースの流れと重なる点が多いと思います。

https://x.com/oikon48/status/1913180659004350849?s=20

4月9日にClaude Maxプランが発表され、**5月1日にはClaude CodeがMax planで定額利用できるようになりました**。当時はあまり話題になっていませんでしたが、Claude Codeが普及する布石になっています。

https://claude.com/blog/max-plan

私はこの発表の数日後にMax 5x($100)のプランを契約しました。当時の私はせいぜい$20のAIツールへの課金しかしていなかったため、大きな変化でした。今では20x($200)を使うようになっているので分からないものですね。

https://x.com/oikon48/status/1919078662323745066?s=20

## Claude Codeの正式リリース

2025年5月25日にAnthropicからClaude 4の発表がありました。当時の様子について書いたZennの記事があるので、良かったら読んでみてください。

https://zenn.dev/oikon/articles/adac12ea9e839b

この時のClaude Codeについての大きな変化は、以下の点が挙げられます。

- **Claude 4 (Opus/Sonnet) の性能が良かった**
- **Maxの定額プランが認知された**

もともと自律駆動の観点で優秀さを見せていたClaude Codeでしたが、Claude 3.7の時点ではモデルの性能がハーネスの力を引き出すのに足りていない印象でした。**Claude 4が登場したことで、Claudeのツールの扱い方がうまくなり活躍できるレベルになったことが大きいです**。

またMaxプランが認知されたことも重要です。従来のAIコーディングは、従量課金で使用した分API料金を支払いながら実装していくスタイルが多かったため、心理的にも慎重になっていたと感じています。Maxプランで定額でClaude Codeという優秀なAIツールが利用できるようになったことで、AIコーディングのマインドセットが代わり、試行錯誤をしながら幾つもの実装を試すことができるようになりました。この定額利用は業界全体に受け入れられ、Codex・Gemini・Cursorなど一部のプランで採用されています。

## Claude CodeのCHANGELOGを振り返る

https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md

Claude Codeの正式リリースでClaude 4が登場するとともに、Claude Code自体も初のメジャーアップデート (v1.0.x系)を遂げました。2025年12月現在はv2.0.x系です(記事執筆時点の最新は2.0.74)。

これまでの176回のアップデートを全て取り上げる訳にはいかないので、v1.0.x系以降の変更の中で個人的にインパクトがあったと思うものをいくつか紹介します。

先にまとめると、**多くの機能はコンテキストエンジニアリングと強く結びついたものであり、現時点ではコンテキストエンジニアリングについて考える必要があるということを示唆している**と思います。

### Claude Code v1.0.x系

#### CLAUDE.md(メモリファイル)

CLAUDE.mdはv0.2.x系からずっとあったものですが、v1.0.xのリリースと共に「**CLAUDE.mdには何を書くべきか**」という議論が度々起こりました。もともとCursor Rulesなどもありましたが、Claude Codeが自律的に実装をする際にはより重要な立ち位置になったと思います。

**コンテキストエンジニアリング**という言葉が広まり、AIエージェントのコンテキストウィンドウの中身をコントロールする重要性が注目され、CLAUDE.mdの重要性もさらに増したと感じています。

8月にCodex CLIが[AGENTS.md](https://agents.md/)を提案し、AIツール全体のプロジェクトメモリを統合しようとしている流れからも、CLAUDE.mdの存在はAIコーディングに大きなインパクトを残しました。

ちなみにClaude CodeはAGENTS.mdをサポートしていません。GitHub Issueでこの件についてはユーザー間で議論されています。

https://github.com/anthropics/claude-code/issues/6235

個人的にAnthropicはAGENTS.mdのサポートをしないと思っています。理由は以下の通りです:

- **OpenAIの標準規格に乗っかる敷居が高い**
- **Claude Codeでworkaround(回避策)がある**
- **全てのハーネスとモデルで使える普遍的なプロジェクトメモリは難しい**

AnthropicはMCPや最近であればAgent Skillsなど、AIツールの業界標準を作ってきていますが、他社の規格に乗っかるのは慎重な印象です。

また上記のClaude CodeのGitHub Issueをみてもらうとわかりますが、`@`インポートやシンボリックリンクなどの回避策が現時点で存在します。回避策ついては、先日の投稿したアドベントカレンダーの記事の【Day5】にも記載しています。

https://zenn.dev/oikon/articles/cc-advent-calendar#%E3%80%90day5%E3%80%91-claude-code-%E3%81%AE-agents.md-%E5%AF%BE%E5%BF%9C

最後にこれは個人的な意見ですが、**AGENTS.mdを一つあれば全てのAIツールに適応できるという考えは少し難しいと感じています**。AIツールはモデルとハーネスの集合であり、それぞれの能力を十分に引き出すにはそのツールに適したプロジェクトメモリが必要なはずです。MCPと違い、AGENTS.mdは自然言語のMarkdownであり、プロトコルと呼べるものではないと思います。

この辺りがAnthropicがAGENTS.mdへの統合に乗り気でない理由ではないかと個人的に考えています。

#### Planモード

Planモードはv1.0.18の頃(6月12日)にサイレントで追加された機能です。`Shift + Tab`でPlanモードに切り替えすることができ、`/config`の設定で起動時のデフォルトにすることもできます。記憶が正しければ、Claude Codeよりも先にClineが4月頃にPlanモードを取り入れていたと思います。

Vibe Codingに浮かれていた当時の私は、軽率に`--dangerously-skip-permission`を実行していたので、愚かにもその価値をすぐに理解できないでいましたが、Claude Codeを長く使うにつれてPlanモードの有用性に気づきました。

Planモードが重要な点としては、当たり前ですが**事前にAIエージェントの実行手順が明確に分かる点**です。AIコーディングでの失敗の多くは、AIエージェントが実行する手順の内訳が不明瞭であり、意図した実装手順を踏んでくれないことにあります。

さらにPlanモードの価値は、**実装後のボトルネックの改善にも寄与する点**だと考えています。Planモードで事前に仕様(Spec)を明確にすることで、AIエージェントがその仕様にある程度沿ってタスクの実行を行い、成果物との乖離を小さくできます。その結果AIコーディングで顕在化してきたレビューなどのボトルネックの削減にも関節的に寄与します。Shift Left Testingと同様に早い段階で手戻りやリグレッションになる原因をできるだけ排除しておくことが、後の工程にも良い作用をもたらすイメージに似ていると思います。

Planモードの重要性については、Claude Codeの開発者であるBoris Cherny氏に質問した際に彼も言及していました。彼はほぼ毎回Planモードからスタートしていると話しています。

https://x.com/bcherny/status/2004711107018342430?s=20

また最新のClaude Codeでは、`~/.claude/plans/`にPlanドキュメントを作成するようになっており、仕様駆動開発(Spec Driven Development: SDD)のような手順に近づいていると感じています(こちらも[アドカレ【Day17】](https://zenn.dev/oikon/articles/cc-advent-calendar#%E3%80%90day17%E3%80%91-plan%E3%83%A2%E3%83%BC%E3%83%89)に記載済み)

#### Subagents

SubagentsはClaude Codeの初期から存在しており、`Task`ツールとして振る舞っています。当時のSubagentsについては以下の記事で言及しました。

https://zenn.dev/oikon/articles/82c9a52dc45810

v1.0.60から`/agents`コマンドが実装され、カスタムサブエージェントが使えるようになりました。

Subagentsは以下の特徴があります

- **並列起動が可能**
- **実行する役割の特化**
- **独立したコンテキストウィンドウ**

特にコンテキストウィンドウが独立している点が、コンテキストエンジニアリングの観点では重要だと考えています。現状のClaude Codeのコンテキストウィンドウは限られており、`WebSearch`や`Read`で大量の情報を取得するとすぐにコンテキストウィンドウを占領します。

Subagentsを使うことで、**余計な情報を親のコンテキストウィンドウに入れずに、Subagentsのコンテキストウィンドウに逃すことに一役買っています**。コンテキストエンジニアリングを考えなくてはいけない現時点では、このような回避策があることが重要です。

またAIエージェントは今後オーケストレーションが進むと思いますが、現時点で気軽にできるオーケストレーションの手段として並列起動できるSubagentsは重要です。

#### /context

`/context`コマンドはv1.0.86(8月20日)に追加された機能です。この`/context`コマンドの価値は、文字通りコンテキストの可視化だと思います。

![context](/images/cc-2025/context.png)

コンテキストエンジニアリングという考え方が広まってはいましたが、概念として存在していると分かっていても、なんとなく理解しきれていない人が多い印象でした。

`/context`は以下のような内容を表示します:

- システムプロンプト(System prompt)
- ツール(System tools)
- MCPツール(MCP tools)
- サブエージェント(Custom agents)
- プロジェクトメモリ(Memory files)
- メッセージ(Messages)

これらの内容から、**コンテキストウィンドウをどれだけ占有しているか、後どれくらいのコンテキストウィンドウが残っているかが一目で分かるようになりました**。

`/context`コマンドにより多くのユーザーがコンテキストウィンドウを認知することができ、コンテキストエンジニアリングの重要性を理解できたと思っています。よく言われる例としては、MCPツールの占有率が大きく、できるだけ不要なものを削除したほうがいいなどの話も最近では各所から出てきています。これは`/context`コマンド以前ではあり得ませんでした。

コンテキストエンジニアリングという多くの人にとって曖昧な概念に光を当てて輪郭を見えやすくしたという点で、`/context`コマンドの功績は大きいです。

### Claude Code v2.0.x系

#### Opus 4.5

Claude Code 2.0.x系は**Claude Opus 4.5のリリース(11/24)が重要なイベントだった**と思います。

当時はClaude CodeからCodex CLIに移行している方も多かったです。理由はClaude Codeの劣化の噂とGPT-5.1 Codexの優秀さをよく耳にしました。ちなみに私は当時もClaude Codeがメイン、Codex CLIがサブでしたが、Codexに任せるタスクの比重もそれなりにあったと記憶しています。

https://www.anthropic.com/news/claude-opus-4-5

v2.0.51からOpus 4.5がClaude Codeで使えるようになったことで、じわじわとですがClaude Codeに戻ってくる人も増えた印象です。「じわじわ」書いた理由としては、この時期のAIモデルはコーディングツールでの単純な実装では性能さを計りにくくなっており、数時間〜数日ほど使い込まないと違いを理解しにくいと感じていた人が多い印象だったためです。Codex CLIも素晴らしいAIツールに進化していたことや、CursorがComposer-1などで独自の地位を築いていたことも大きいと思います。

実際に使ってみるとClaude Opus 4.5は、Claude Codeのツールの仕様に長け、実装速度も十分であることが感じられました。特にClaude Codeというハーネスとの親和性が以前のClaude Opus 4.1やSonnet 4.5よりも高いと思いました。**Claude Opus 4.5は間違いなくClaude Codeの地位を引き上げる大きな要因になり、AIコーディングの歴史をまた一つ大きく進めた存在だと思います**。

余談ですが、AIモデルの性能を測るのは個人的には難しくなってきていると思います。単純なタスクであればどのAIツールも高水準でアウトプットができるようになっています。

よくSWE-Benchというベンチマークが引き合いに出されますが、最近はマーケティング・パフォーマンスの意味合いが強いと感じます。実際に使えるかは自分の手を動かさないと確信が持てないです。この点について、最近見たPromptLayerの方が登壇されたYoutubeの動画でも言及されており面白かったです。

https://youtu.be/RFKCzGlAU6Q?si=5bzmL5vztxb7wASN

#### Claude Skills (Agent Skills)

Claude Skillsはv2.0.22(10月17日)に追加された機能です。12月18日に[Agent Skills](https://github.com/agentskills/agentskills?tab=readme-ov-file)としてオープンスタンダードになりました。

Skillsは最近何かと話題になっており色々な方が解説してるので詳細は省きます。Skills全般については、以下の動画がわかりやすいです。(またしてもCode Summitの動画)

https://youtu.be/CEvIs9y1uog?si=gmtgYjqCYb_A2VPA

**Agent Skillsは、コンテキストの段階的開示という概念さえ理解できていればいい**と思います。逆にいうとここでもコンテキストエンジニアリングが重要になっています。またSkillsのキモはツールのオーケストレーションであり、エージェントのオーケストレーションとの組み合わせが今後ますます注目されると思います。

Anthropicは上記のSkillsの特徴を前面に押しつつ、Claude CodeだけでなくClaude Desktopなどでも使用できるポータビリティを持った設計をしています。MCP(Model Context Protocol)の時にも思いましたが、AnthropicはAIエージェントに重要な概念や機能を開発することが本当にうまいですね。

#### Interactive Question Tool

Claude CodeのInteractive Question Toolという名前は馴染み無いと思います。Planモードの時によく出てくる「Claudeがユーザーに細かいところを聞いてくる」機能です。Interactive Question Toolという名前は[CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md#2021)に記載があったため、文章内で採用しています。

https://x.com/i/status/1979338989330149754

この機能は個人的には画期的だと思っており、Planモード際の仕様の境界線を明確にすることができます。

今までのAIコーディングで手戻りが発生する問題として、ユーザーの想定している仕様とAIが実装する際に発生するにギャップが問題になっていました。ユーザーがAIエージェントにインプットするプロンプトや仕様書などではっきりとした仕様の境界線が引けていれば問題ないのですが、多くの場合はその境界線が曖昧であり、その結果AIエージェントがユーザーの想定している仕様から逸脱した実装をすることがしばしば発生していました。

**Interactive Question Toolにより、AIエージェント側から（人間さながら）仕様の詳細を確認できるようになったのは、AIツールにおける大きな進歩**だと感じました。今までユーザー側からの一方的なやりとりから、相互のやり取り（インタラクション）に変わったのは今後のAIコーディングのあり方に大きなインパクトを与えていると思います。

#### Plugin Marketplace

Claude CodeのPluginシステムも個人的に注目している機能です。AIツールはとても強力ですが、チーム開発においてはまだ十分な活用ができていない組織も多いです。**そのAIツールのチーム活用が進みにくい大きな理由としては「AIツールの習熟度がある」ことが挙げられます**

Claude CodeのようなAIツールをバリバリ使いこなす人は自前でSkillsやHooksやカスタムスラッシュコマンドを用意できますが、AIツールに抵抗があったり馴染みがない方の使い方と大きな差があります。その結果、おれおれCLAUDE.mdなどの属人性が生まれやすくなります。

Pluginシステムは機能を配布することで、チーム開発でもAIツールを使いやすくすることに今後一役買うと思っています。VSCode Marketplaceのプラグインのように簡単にインストールして機能共有できるエコシステムをすでに構築している点はClaude Codeの強みだと思います。

Pluginの作り方も`.claude-plugin`の中に`marketplace.json`と`plugin.json`を作るだけで簡単なので、ぜひ作ってみてください。参考までに私のデモ用のGitHub Projectを貼っておきます:

https://github.com/oikon48/cc-frontend-skills

### Claude Codeの優秀な点

ここまでClaude CodeのCHANGELOGの中から個人的に特筆すべき機能を見てきましたが、Claude Codeの優秀な点をツールデザインの観点から触れたいと思います。個人的にClaude Codeが優秀だと思う点はいくつもありますが、今回は2つ取り挙げます。

#### CLIツール

Claude Codeの強みの1つはCLIツールであることだと思います。CLIツールは開発者であれば誰でもどこでも使えます。

歴史的にCLIからGUI（IDEなど）に開発ツールやWebの文脈で移行していますが、AI時代では逆にCLIに戻ったというよりも、AIエージェントというユーザーインターフェースに移行したと理解しています。つまり、

**CLI -> GUI -> Agent UI**

というイメージです。Claude CodeはCLIツールのように思いますが、Agent UIの先駆けにもなっていると個人的に感じています。

Claude Codeは、[開発者のBorisのサイドプロジェクト](https://x.com/bcherny/status/2004887829252317325?s=20)だったゆえにCLIツールとして誕生した背景がありますが、最終的にCLIツールというデザインが残っているのいくつか理由があると思います。この点についてはAnthropicの動画で以下のように述べられていました:

- **開発者が必ず使うターミナルを選んだ**
- **LLM登場でリッチUIからチャットに回帰**
- **シンプルなCLIはユーザーがタスクに集中できる**
- **CLIはモデルを包む最も薄いWrapperで、可能な限りクリーンに保つことができる**

https://youtu.be/vLIDHi-1PVU?si=y7QFKKP9Qfibz-v7

#### Claude Codeというハーネス

Claude CodeがAIコーディングにおいて確固たる地位を築き始めているのは、Claude Codeというハーネスの完成度の高さにあると思います。

ここまでAIモデルとハーネスという言葉をたびたび本記事の中で使ってきましたが、これらはAnthropicの動画[The future of agentic coding with Claude Code](https://www.youtube.com/watch?v=iF9iV4xponk) でも言及されています。

https://www.youtube.com/watch?v=iF9iV4xponk

AIツールはAIモデルとハーネスに分割され、その2つの組み合わせによって能力を発揮することが述べられています。動画の中では**AIモデルを馬、Claude Codeをハーネス（馬具）と例えて、AIツールをうまくコントロールする存在として説明していました**。個人的にとても好きな表現で、登壇の際にもたびたび引用しています。

Claude Codeというハーネスは、適切にClaudeというAIモデルにツール・プロンプト・コンテキストを提供して、タスクを完了させるためにモデルを導く点でとてもうまく設計されています。この点が、他のAIツールとの大きな差になっていると認識してます。

### Claude Codeについて感じる課題

ここまでClaude Codeの良い点ばかりを話してきましたが、もちろん課題もあります。というより課題がまだまだ山積してます。いくつか大きいものを取り挙げます。

#### コンテキストウィンドウのサイズ

一つ目に挙げられる課題はコンテキストウィンドウサイズです。GPT-5.2が400k、Gemini 3 Proが1mのコンテキストウィンドウを持つのに対してClaude Opus 4.5は未だに200kです。**これは大きな制約をユーザーに課しています**。

Claude Codeはコンテキストウィンドウの問題を、Subagentsや最近実装されたrulesで緩和できるようにしていますが、ハーネスの力を持ってしてもこの問題は目につきます。

私はClaude Sonnet 4.5 \[1M\]が幸い解放されており、Claude Opus 4.5が出る前はこちらをメインで使っていました。コンテキストウィンドウが大きくなるだけでここまで使いやすくなるのかと感じた記憶があります。逆に言えば、Claudeが標準で1Mのコンテキストウィンドウを備えるようになったらどうなってしまうんでしょうね。

#### コンパクションの質

Claudeの200kのコンテキストウィンドウが現状解決できない問題だとしても、Claude Codeのコンテキストコンパクション(/compact, auto-compact)はまだ改善の余地があると感じています。

コンパクションについてはCodex CLIの方が良いと感じる場面が何度かあります。**Claude Codeは明示的にコンパクション指示を設定しなければ単純にセッションの要約をしている印象です**。

モデルのコンテキストウィンドウが大きくなれば、将来的にはコンパクションを気にする事が無くなると思いますが、現状はコンパクション指示をするかこまめに`/clear`でコンテキストウィンドウをクリーンにしてあげる必要があります。

#### ナレッジカットオフ

Claudeに限らないモデルの普遍的な問題ですが、ナレッジカットオフの影響を感じることが多いです。具体的には最新のライブラリやバージョンがモデルが知らないために弾き起こるバグによく遭遇します。

回避策として、WebSearchやContext7 MCPを用いて最新の情報を取得する方法がありますが、この辺りもハーネスであるClaude Codeがよしなにやってくれると良いと感じます。最近読んだ[Simon Willison氏の記事](https://simonwillison.net/2025/Dec/28/actions-latest/)で紹介されていたように、**バージョン関係の情報を一極集中させSkillsで必要になった時にloadさせる手段が良いと個人的に思っています**。

Claude Codeはまだ産まれて10ヶ月くらいの赤ん坊のようなプロダクトです。今後の成長を見守りたいと思います。

### Claude Codeのサードバーティツールを使うべきか

Claude Codeは今年一気にユーザー数が増え、周辺のサードパーティツールも増えてきました。有名なものだと、カスタムスラッシュコマンドやサブエージェントを集めたawesome-claudeシリーズ（いろんな人が作っている）などでしょうか。さらに複雑なハーネスの仕組みをClaude Hooksを使って実現しているツールも数多く存在します。

個人的な意見ですが、これらのサードパーティツールは慎重に使用した方が良いと思っています。

Claude Codeというプロダクトは、ClaudeというモデルとClaude Codeというハーネスの両方で成り立っています。**サードパーティツールの多くはそのハーネスの外側にさらに追加のハーネスを建設しているようなイメージです**。

Claudeのモデルとツールの進化は劇的であり、Claude Codeの動きも1週間単位で大きく変わります。サードパーティツールがこの変化についていけるほどメンテナンスされていれば問題ないかもしれませんが、多くのツールは余計なハーネスになる可能性があります。

将来的にClaude Code自体の機能に盛り込まれ改善される可能性も高いため、あまり過度にサードパーティツールに依存するのは、個人的にはオススメしていません。

## これからのClaude Code

2025年のたった10ヶ月でClaude Codeは飛躍的な進化を遂げました。ここまでClaude Codeの現在について、個人的な意見を述べてきましたが、2026年のClaude Codeについても考察してみたいと思います。

### Claude Codeの進化の行方

Claude CodeはAIコーディングツールとして確固たる地位を築いていると思いますが、まだまだ進化の余地を残してます。先日のClaude Code Meetup TokyoでClaude Codeの開発者であるBoris氏に直接質問して得られたヒントから、進化の行方を考えていきます。

https://x.com/oikon48/status/2003054927862718844?s=20

#### Long running　(長時間実行)

Claude Codeは現時点でもかなり自律的に行動をする力がありますが完全ではありません。

たとえばコードベース全体を確認してある処理を行うようなタスクの場合、Claude Codeがまだ完了していなくても途中で止まってしまうことがしばしばあります。その際にユーザーはいちいち「Continue、まだ終わってないよ」と伝えて再開させる必要があります。

今後はClaude Codeがさらに長時間実行できるようになりタスクを完遂できるような改善がされると思います。

ちなみに現時点でも長時間実行するための[**ralph-wiggum**というAnthropicの公式プラグイン](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum)があります。これが標準で搭載されるもしくは改善されそうです。

https://x.com/oikon48/status/2005105826730836374?s=20

#### Swarming (組織実行)

Claude Codeの組織実行についても述べられていました。ここでいう組織実行は、Subagentsを並列実行するものとは別物だと考えています。

組織実行といえば2025年6月ごろにtmuxを使ってClaude Codeを複数立ち上げて、Managerと複数のWorkersに役割を分けてオーケストレーションをする使い方が話題になりました。

このようなSwarming(組織実行)をClaude Codeが標準でできるようになる可能性があります。個人的に何のアナウンスもなく入っているdelegete(委譲) modeがswarming機能の一部ではないかと予想しています。

https://x.com/oikon48/status/2001064637845135572?s=20

#### 外部ツール連携

外部ツール連携についてはBoris氏のコメントではなく、私個人の予想です。

現在Claude Codeには標準のツールに加えて、Skills、MCPツールといったClaude Codeが自律的にツールが数多く用意されています。一方で、これらのツールを適切なタイミングで正しいツールを選べるかという点において、まだまだ改善の余地があると思っています。

特にMCPツールについては、コンテキストウィンドウを圧迫することが指摘されているため、ツールコールの仕組みを改善する必要性があると感じています。Anthropic Builder Summit Tokyoでもこの点は少し話題になっており、プログラムの経由でMCPツールを使うなどの発想の話を聞いています。MCPに関しては、βリリース機能として動的にMCPツールをロードする機能が仕込まれていて試すことができます。

https://x.com/oikon48/status/2005803922167083152?s=20

外部ツール絡みでもう少し話を広げるのであれば、2026年はさらにPhysical AIが注目されると思っています。ロボットや自動運転などですでに盛り上がっていますが、Claude Codeについても外部のセンサーを通じて現実世界を認識して干渉できるようになるような展開があると考えています。

### Claude Codeを追いかける価値

Claude Codeは素晴らしいツールだと思います。特にClaude Opus 4.5が出てからは頭ひとつ抜きに出ていると感じています。ただ他のAIツールも十分性能が向上しており、正直ユーザーのユースケースや肌感に合うものを選べば良いです。

そんなAIツールが群雄割拠している時代においても、**Claude Code（そしてAnthropic）は常に最前線でAIツールのマイルストーンを置き続けているため、個人的に使う・追いかける価値はあると十分ある**と思っています。少なくとも2025年はClaude CodeがAIツールを牽引してきました。Claude Codeがリリースしてきた数々の機能や仕組みは、数年後にエンジニアリングの歴史を見返した時に間違いなく記録に残るでしょう。

今後十数年とエンジニアリングに関わる人にとっては、追いかける価値のあるツールだと思います。

## まとめ

今年一年、Claude Codeを見てきて浮かんできた個人的な考えをまとめたいと思い記事を書きました。自分にとってもかなりインパクトが大きい年であったと改めて実感しています。こんな面白い時代に現役でエンジニアリング携われて良かったと心から思います。

来年はさらに加速していく予感がすでにしていますが、楽しむ事は忘れずに時折歯を食いしばりながら振り落とされないように走っていきたいと思います。

余談:

ちなみに以下の記事に触発されてまとめようと思いました。長いですが良記事なので、ぜひNotebookLMで要約せずに読んでください。

https://sankalp.bearblog.dev/my-experience-with-claude-code-20-and-how-to-get-better-at-using-coding-agents/

## Xフォローしてくれると嬉しいです

Xで情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

Claude Code CHANGELOG:
https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md

Claude Max Plan:
https://claude.com/blog/max-plan

AGENTS.md サポートについてのGitHub Issue:
https://github.com/anthropics/claude-code/issues/6235

Claude Opus 4.5 発表:
https://www.anthropic.com/news/claude-opus-4-5

Agent Skills:
https://github.com/agentskills/agentskills

ralph-wiggum Plugin:
https://github.com/anthropics/claude-plugins-official/tree/main/plugins/ralph-wiggum

Simon Willison - actions-latest:
https://simonwillison.net/2025/Dec/28/actions-latest/

Sankalp - My experience with Claude Code 2.0:
https://sankalp.bearblog.dev/my-experience-with-claude-code-20-and-how-to-get-better-at-using-coding-agents/

Anthropic YouTube - The future of agentic coding with Claude Code:
https://www.youtube.com/watch?v=iF9iV4xponk

Anthropic YouTube - Claude Code design philosophy:
https://youtu.be/vLIDHi-1PVU?si=y7QFKKP9Qfibz-v7

Skills解説動画:
https://youtu.be/CEvIs9y1uog?si=gmtgYjqCYb_A2VPA

SWE-Bench に関する動画:
https://youtu.be/RFKCzGlAU6Q?si=5bzmL5vztxb7wASN

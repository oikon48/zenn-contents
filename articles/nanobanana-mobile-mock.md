---
title: "nano-bananaでモバイルアプリUIモックアップを作る"
emoji: "🍌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [nanobanana, gemini, 画像生成, 生成ai]
published: true
---

Oikonです。

今月からモバイルアプリ開発にチャレンジしており、悪戦苦闘中です。

アプリケーションを開発する際には、要件定義やUIデザインなどを事前に決めることが多いです。今回は**UIデザインのアイデア出し**について焦点を当てます。

個人的にモバイルアプリのデザインは門外漢なのでどうしようかと思っていました。UIモックアップの作成には以下のような手法があると思います。

**UIモックアップの例**:

- ペーパープロトタイピング: 紙とペンを使って画面レイアウトをスケッチ
- ワイヤーフレーム: 基本的な構造と配置に焦点を当てた簡素なデザインを作成 (Balsamiq, Draw.io)
- デザインツールモックアップ: 実際のアプリに近い見た目のデザインを作成する手法 (Figma, Sketch、Adobe XD)

モバイルアプリ開発の初学者だと具体的なアプリのUIモックアップに躓くことも多いです。Figmaなどを使えればいいですが、学習コストもあります。

アプリUIのアイデア出しだけでも上手い方法はないかと考えていたところ、先日GoogleがGemini 2.5 Flash Image (通称nano-banana🍌)をリリースしたため、これを使ってUIモックアップを作ろうと考えました。

今回はプロンプトエンジニアリング的な記事になりますが、nano-bananaの使い方の参考になれば幸いです。

## Gemini 2.5 Flash Image (nano-banana)

Gemini 2.5 Flash Image (nano-banana)は、Googleが2025年8月26日にリリースした画像生成AIモデルです。

https://x.com/OfficialLoganK/status/1960343135436906754

参考までにプロンプト集も引用しておきます。

**Google公式プロンプト集**

https://x.com/googleaistudio/status/1962957615262224511

**Nano-banana プロンプトリポジトリ**

https://github.com/PicoTrex/Awesome-Nano-Banana-images

## 今回使用したプロンプト

今回使用したプロンプトでは、以下のようなUIモックアップの画像を生成できます。

![UI Mockup](/images/nanobanana-mobile-mock/template.png)

*UI Mockup*

プロンプトは以下のようなテンプレートです。ある程度カスタマイズ可能なものになっています。

```prompt
<Create a professional iOS app mockup figure>
showing exactly 4 iPhone screens arranged horizontally in a single high-resolution image. The screens demonstrate a complete user journey for a [Application Type] application.

<APP SPECIFIC CUSTOMIZATION>
Application Core: [User Input of application core]
Primary Features:
1. [User Input of feature 1, example: Dashboard]
2. [User Input of feature 2, example: Figure]
3. [User Input of feature 3, example: Charts]
4. [User Input of feature 4, example: Log]
Visual Theme: [User Input of Theme, example: Energetic orange and black theme, bold sans-serif typography, high contrast design, motivational imagery]
Target Audience: [User Input of audience, example: Aged 20-40]
<END CUSTOMIZATION>

DETAILED VISUAL SPECIFICATIONS:
Device Frame: Modern iPhone 15/16 Pro Max with Dynamic Island, edge-to-edge display, no home button
Screen Resolution: High-quality, crisp rendering at 2796×1290 pixels per screen
Background Setting: Subtle blurred gradient or professional studio backdrop that complements the app theme
Lighting: Soft, even lighting with minimal shadows for professional presentation

UI/UX REQUIREMENTS:
Native iOS 17+ components using SwiftUI design language
System font (SF Pro) with proper typography hierarchy
Consistent spacing following Apple's Human Interface Guidelines (8pt grid system)
Interactive elements clearly distinguishable (buttons, toggles, text fields)
Proper safe areas and status bar implementation
Tab bar or navigation structure visible where appropriate
Realistic, meaningful content (no lorem ipsum) - use actual examples relevant to the app
Clear visual flow from left to right showing user progression

COMPOSITION AND STYLE:
Arrange 4 iPhone mockups in a horizontal line with equal spacing
Each screen labeled with its function at the top or bottom
Consistent color palette throughout all screens
Use depth and shadows to create professional mockup appearance
Include subtle animations indicators where relevant (like transition hints)
Show different states (empty states, filled states, active selections)

OUTPUT FORMAT:
Single cohesive figure, 4K resolution minimum, professional presentation quality suitable for App Store submission or investor pitch deck. Photorealistic device rendering with the app screens perfectly integrated.
```

プロンプトテンプレートは以下のように構成されています。プロンプトテンプレート自体はClaudeに作成してもらっています。

- 基本要件 (**User Input required**)
  - 目的: iOSアプリのモックアップ作成
  - 基本仕様: 4つのiPhone画面を横一列に配置
  - 用途: ユーザージャーニーの完全な可視化
  - 括弧 [ ] 内が入力プレースホルダー
- ユーザーカスタマイズセクション (**User Input required**)
  - ユーザーが自由に変更できる変数部分
  - アプリの機能、4つの主要機能、ビジュアルテーマ、ターゲット層を指定
  - 括弧 [ ] 内が入力プレースホルダー
- ビジュアル仕様 (DETAILED VISUAL SPECIFICATIONS)
  - 最新のiPhoneモデル（Dynamic Island搭載）
  - 背景とライティング
- UI/UX要件 (UI/UX REQUIREMENTS)
  - Apple公式のHuman Interface Guidelines準拠
  - ネイティブiOSコンポーネント使用
  - タイポグラフィとスペーシング
- 構成とスタイル (COMPOSITION AND STYLE)
  - 4画面の水平配置
  - カラーパレット
  - 外観
- 出力形式 (OUTPUT FORMAT)
  - Single cohesive figure, 4K resolution
  - Professional presentation quality

テンプレートの1つ目(\<Create a professional iOS app mockup figure\>)と2つ目(\<APP SPECIFIC CUSTOMIZATION\>)だけ入力すればそのまま試すことができます。英語のプロンプトの方がnano-bananaの出力が良いように個人的に感じたので英語のプロンプトです。

## 生成されたモバイルアプリUIモックアップのサンプル

いくつか実際に画像生成したアプリUIモックアップの例を紹介します。プロンプトは全文だと長いのでテンプレートのユーザー差し替え部分のみです。

### フィットネスアプリ

![フィットネスアプリUIモックアップ](/images/nanobanana-mobile-mock/fitness.png)

*フィットネスアプリUIモックアップ*

**カスタマイズプロンプト**:
```prompt
<Create a professional iOS app mockup figure>
showing exactly 4 iPhone screens arranged horizontally in a single high-resolution image. The screens demonstrate a complete user journey for a Fitness tracker application.

<APP SPECIFIC CUSTOMIZATION>
Application Core: Personal fitness tracking and workout planning application with AI coaching
Primary Features:
1. Dashboard - Daily activity summary, calories burned, steps, heart rate
2. Workout Library - Exercise videos, custom routines, difficulty levels
3. Progress Charts - Weight trends, strength gains, body measurements over time
4. Activity Log - Workout history, personal records, achievement badges
Visual Theme: Energetic orange and black theme, bold sans-serif typography, high contrast design, motivational imagery with gradient accents
Target Audience: Health-conscious individuals aged 20-40
<END CUSTOMIZATION>
```

### 料理レシピアプリ

![料理レシピアプリUIモックアップ](/images/nanobanana-mobile-mock/cooking.png)

*料理レシピアプリUIモックアップ*

**カスタマイズプロンプト**:
```prompt
<Create a professional iOS app mockup figure>
showing exactly 4 iPhone screens arranged horizontally in a single high-resolution image. The screens demonstrate a complete user journey for a Cooking recipe application.

<APP SPECIFIC CUSTOMIZATION>
Application Core: Smart recipe organizer with meal planning and grocery list generation
Primary Features:
1. Recipe Feed - Curated daily recipes with dietary filters and seasonal suggestions
2. Meal Planner - Weekly calendar view with drag-and-drop meal scheduling
3. Shopping List - Auto-generated grocery lists organized by store sections
4. My Cookbook - Personal recipe collection with family recipes and notes
Visual Theme: Warm earth tones with sage green accents, elegant serif headers, food photography backgrounds, minimalist card-based layouts
Target Audience: Home cooks and food enthusiasts aged 25-50
<END CUSTOMIZATION>
```

### 財務管理アプリ

![財務管理アプリUIモックアップ](/images/nanobanana-mobile-mock/chart.png)

*財務管理アプリUIモックアップ*

**カスタマイズプロンプト**:
```prompt
<Create a professional iOS app mockup figure>
showing exactly 4 iPhone screens arranged horizontally in a single high-resolution image. The screens demonstrate a complete user journey for a personal finance manager application.

<APP SPECIFIC CUSTOMIZATION>
Application Core: Smart personal finance manager with AI-powered spending insights and investment tracking
Primary Features:
1. Overview - Total balance, monthly spending summary, budget status, upcoming bills
2. Transactions - Categorized expense list with merchant logos, search and filters
3. Analytics - Spending breakdown charts, category trends, savings goals progress
4. Investments - Portfolio performance, stock watchlist, crypto holdings, market news
Visual Theme: Professional navy blue and gold accents, clean geometric patterns, financial data visualizations with glassmorphism effects, trust-inspiring design
Target Audience: Young professionals and investors aged 25-45
<END CUSTOMIZATION>
```

## まとめ

今回はモバイルアプリのUIモックアップで使えるアイデア出しのプロンプトを紹介しました。

このまま実用的なUIコンポーネントまで落とし込めるかはユーザー次第なところはありますが、nano-bananaをアプリのデザインアイデア出しに利用できるのは個人的に良いと思います。

nano-bananaはプロンプトを作り込むことで、ある程度一定の品質の画像生成ができることも今回分かりました。

この記事が参考になれば幸いです。

## 𝕏フォローしてくれると嬉しいです！

𝕏でも情報発信しているので、フォローしていただけると励みになります！

https://x.com/oikon48

## 参考文献

https://aistudio.google.com/

https://developer.apple.com/design/human-interface-guidelines/

https://zenn.dev/ml_bear/articles/b58004bf5a3e6c

https://zenn.dev/asap/articles/8d5af79e40fa4d

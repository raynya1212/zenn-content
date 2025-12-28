---
title: "HubSpotもMCPに参戦！？MCPサーバを使ってコンタクトを登録してみた【体験レポ】"
emoji: "🚀"
type: "tech"
topics: [HubSpot,AI,mcpS]
published: true
published_at: 2025-05-12
---



# HubSpotもMCPに参戦！？MCPサーバを使ってコンタクトを登録してみた【体験レポ】

2025-05-12 · Reina

## はじめに 🌿

最近話題の「MCPサーバ」に、HubSpotも対応しはじめたって知ってる？   
AIとアプリの橋渡しをしてくれるこの仕組み、実際に触ってみたらおもしろかったのでレポするよ〜！

![ノートPCを持った女性が「HubSpotもMCPに対応！」と紹介している](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/intro-hubspot-mcp.png)

---

## この記事でわかること ✏️

* MCPとは何か、その仕組み 💡
* HubSpotのMCPサーバでできること 💻
* CSVファイルを使ってコンタクトを登録してみた体験レポ ✨

---

## MCPとは？🧩

MCP（Model Context Protocol）は、AIがアプリや外部サービスとやりとりするときに使う**標準プロトコル**。   
よく「**AI用のUSB-C**」ってたとえられてるよ！

たとえば、AIが「この会社の情報を見せて」と言ったときに、裏でMCPが仲介して、正しいデータを正しい形式でやり取りできるようにしてくれる。

USB-Cがデバイス間のやりとりを共通化したように、MCPはAIとアプリの“つなぎ”を共通化して、開発者もユーザーもハッピーにしてくれる、というわけ。

> Think of MCP like a USB-C port for AI applications.   
> MCP provides a standardized way to connect AI models to different data sources and tools.   
> → [公式サイト](https://modelcontextprotocol.io/introduction)

---

## HubSpot MCPサーバって？🐙

HubSpotが提供するMCPサーバは、MCPの仕組みをHubSpotとつなげる**専用ゲートウェイ**みたいなもの。

![HubSpotのタコマスコットと女性が、AIアプリとCRMをつなぐMCPサーバについて説明しているイラスト](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/hubspot-mcp-server-gateway.png)

ClaudeなどのAIアプリと連携して、HubSpot CRMのデータを自然言語で読み書きできるようになるのがポイント。

このサーバは、HubSpotのCRMと安全に接続し、AIが以下のようなタスクを実行できるようにするみたい（以下[サイト](\"https://developers.hubspot.com/mcp\")より引用）：

### ☑️ CRMレコードの作成・更新

  \"Create a new contact “[John.Johnson@email.com](\"mailto:John.Johnson@email.com\")” for Acme Inc.\"



  \"Update the address for John Smith in my HubSpot account\"


### ☑️ CRM内の関連情報の取得

  \"List all associated contacts and their roles for Acme Inc\"

  \"Get the company record for Google and place it in a JSON file\"

### ☑️ 分析やフィードバック

  \"Summarize all deals in the “Decision maker bought in” stage with value > $1000\"


  \"Check if a custom property 'Serial number' has consistent values\"

### ☑️ エンゲージメントの追加

  \"Add a task to send a thank-you note\"

  \"Add a note for Acme Inc.\"

HubSpot内に搭載されている [Breeze Copilot](https://knowledge.hubspot.com/ja/ai-tools/use-copilot)との違いは？というと、Breeze CopilotはHubSpot社内のAIエージェントで、より深い業務理解や文脈の活用が得意。対してMCP経由では、**外部AIエージェントがHubSpot CRMにアクセスできる**ようになるのが特徴とのこと。

> MCP allows agents not running on HubSpot to perform some of the capabilities that Copilot has.   
> → [HubSpot MCPサーバ公式ページ](https://developers.hubspot.com/mcp)

---

## 実際にやってみたこと 🧪

### ✅ やりたいこと

CSVファイルにある100件のサンプルコンタクトを、MCP経由でHubSpotに登録！

### Step 1：CSVを用意 🗂️

[個人情報テストデータ生成ツール](https://testdata.userlocal.jp/)を使って、

* 姓名
* メールアドレス
* 電話番号
* 会社名   
  などを含むCSVファイルを作成。

ヒント：
HubSpotが読めるように 姓 と 名 は分けておこう

### Step 2：環境準備 🛠️

* [`node.js`](https://nodejs.org/en/download) / [Cursor](https://www.cursor.com/ja) をインストール
* HubSpotで「非公開アプリ」を作成し、アクセストークンを取得
* MCPサーバを立ち上げるために、[`@hubspot/mcp-server`](https://www.npmjs.com/package/@hubspot/mcp-server) を使う

  + パッケージ内にある「Cursor」の手順を見てセットアップ

```json
{"mcpServers":{"hubspot":{"command":"npx","args":["-y","@hubspot/mcp-server"],"env":{"PRIVATE_APP_ACCESS_TOKEN":"<your-private-app-access-token>"}}}}
```

### Step 3：Cursorで操作してみる ✨

1. CSVを開いて、Cmd + L でAIパネルを表示
2. 「このCSVでコンタクトを作成して」と指示

   ![mcp-02](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/mcp-02.png)
3. Pythonスクリプトが自動生成されたので実行してみる

   ![mcp-03](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/mcp-03.png)  
   ![mcp-04](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/mcp-04.png)
4. スクリプトを動かすと、HubSpotに登録完了！

---

## 登録された内容を確認してみた 👀

HubSpotのコンタクト一覧画面を見てみると…

![作成されたコンタクト例](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/mcp-01.png)

CSVファイルに合った名簿がコンタクトとして登録されていました！！✨

ちなみに、MCPサーバ経由で作成されたコンタクトは以下のソースになっていました（非公開アプリ経由で作られたという扱いになってますね）

![mcp-05](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/mcp-05.png)

| プロパティ名 | 値 |
| --- | --- |
| レコードソース | 連携 |
| レコードソースの詳細1 | <非公開アプリの名前> |
| オリジナルトラフィックソース | オフライン |
| ドリルダウン1 | INTEGRATION |
| ドリルダウン2 | <非公開アプリの名前> |

---

## 感想 ✨

* MCPのおかげで、AIがCRMを操作する未来が見えてきたかも！？
* **実用にはまだ工夫が要りそう**だけど、ポテンシャルはすごい！
* **Cursor×MCPの相性も抜群◎**（Proはちょっとお高いけど…💸）

---

## まとめ 🌈

HubSpotのMCPサーバを使えば、親しみのあるAIコードエディター上でCRM操作が可能に！   
実用まではもう一歩かもだけど、未来がちょっと楽しみになる体験でした！🐙

![虹を背景に笑顔でMCPを語る女性とHubSpotタコのキャラが並ぶ](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/summary-hubspot-mcp.png)

---

## あわせて読みたい 📖

* [HubSpot MCP Server](https://developers.hubspot.com/mcp)
* [The HubSpot MCP Server - available in Public Beta](https://developers.hubspot.com/changelog/mcp-server-beta)
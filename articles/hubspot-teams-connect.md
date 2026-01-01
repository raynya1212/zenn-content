---
title: "【HubSpot × Teams 連携】2. HubSpotにチームを接続しよう [2025更新]"
emoji: "🧶"
type: "tech"
topics: [HubSpot, Teams]
published: true
published_at: 2025-10-14
---

# 【HubSpot × Teams 連携】2. HubSpotにチームを接続しよう

2025-10-14 · Reina

### はじめに

前回の記事では、Teams に HubSpot アプリを導入するところまで進めました。
この段階でも HubSpot の通知を受け取るボットとしては機能しますが、ワークフローの連携機能なんかを使うには HubSpot と Teams のチームを接続する必要があります🔌

### 🚀 この記事のゴール

というわけで、今回は 「 HubSpot と Teams のチームを接続する」をゴールに設定を進めていきます💪

### 🐾 チームの接続方法

#### A： HubSpot の 設定画面で接続先のチームが表示されている場合

HubSpot の 設定 > [連携されたアプリ] > [Microsoft Teams] > [全般設定] > [グローバル設定] にて、接続したい Teams のチームが表示されている場合は以下の手順で進めることができます：
- 接続したいチーム名の横にある [接続] ボタンをクリックして
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_13%20(1).png?width=3071&height=1550&name=teams-hubspot_13%20(1).png)
- Teams クライアントを起動（or ブラウザで Teams にアクセス）
- HubSpot アプリの追加画面がポップアップされるので、[開く] をクリック
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_14%20(1).png?width=3072&height=2325&name=teams-hubspot_14%20(1).png)
- アプリを開くか、チャネルに追加するかの選択肢が表示されるので、使用したいチャネルを選択
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_15%20(1).png?width=3066&height=2445&name=teams-hubspot_15%20(1).png)
- ボットから「こんにちは 👋 HubSpotのMicrosoft Teamsボットです」のメッセージが送信されれば、接続完了です
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_16%20(1).png?width=3512&height=2774&name=teams-hubspot_16%20(1).png)

※ 接続後、HubSpot の設定画面でもチームの接続状態に対してトグルがON🔛になっており、接続状態になっていることが確認できます

#### B： HubSpot の設定画面が接続先のチームが表示されていない場合

HubSpot の 設定 > [連携されたアプリ] > [Microsoft Teams] > [全般設定] > [グローバル設定] にて、接続したい Teams のチームが表示されていない場合は、以下の手順にて進める必要があります：


私が検証している限りでは、連携アプリをインストールした後に追加されたチームに関しては HubSpot 側で自動で取得されない場合があるようです

- HubSpot と接続したいチームにアクセスし、[チームを管理] > [アプリ] タブにアクセスします
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_17%20(1).png?width=2718&height=2175&name=teams-hubspot_17%20(1).png)
- [+さらにアプリを取得] をクリックし、アプリストアに移動します
- 「HubSpot」 と検索し、HubSpot アプリをクリック
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_18%20(1).png?width=3072&height=1347&name=teams-hubspot_18%20(1).png)
- A パターン同様にチャネルを選択することができますので、対象のチャネルを選択し[移動]を選択
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_19.png?width=702&height=1000&name=teams-hubspot_19.png)
- Hubspotのボットから「<br>こんにちは 👋 HubSpotのMicrosoft Teamsボットです」のメッセージが送られたら、完了です
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_20.png?width=3344&height=3014&name=teams-hubspot_20.png)
- 接続が完了すると、HubSpot側の設定画面でも新たに接続したチームが表示されていることがわかります
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_21.png?width=3228&height=2291&name=teams-hubspot_21.png)

### 次は？▶️HubSpot×Teams連携の色んな機能を使ってみよう

これで HubSpot と Teams のチームを接続することができました！👏
次回以降は HubSpot×Teams連携の具体的な機能の使い方、に関する記事を書いていこうと思います📝

今のところヘルプデスクやウェビナー周りの設定を見ていこうと思っていますが、何かリクエストなどあればお気軽にコメントください😺

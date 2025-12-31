---
title: "【HubSpot × Teams 連携】1. 連携をセットアップしよう [2025更新]"
emoji: "🚀"
type: "tech"
topics: [HubSpot, Teams]
published: true
published_at: 2025-10-14
---

# 【HubSpot × Teams 連携】1. 連携をセットアップしよう

2025-10-14 · Reina

### はじめに

以前「HubSpot×Teams連携をマスターしたい」のシリーズを書いていました📒

https://zenn.dev/raynya/articles/bff14a3e9189d3


ただ記事を書いてからTeams・HubSpot側にも色々とアップデートがあったので、このあたりで2025年最新版をまとめてみようと思います🐾
ピックアップする機能は気が向いたもの中心になると思いますが、まずは連携をセットアップするところから… 🏃

### 🚀 この記事のゴール

TeamsとHubSpotを連携できる状態にし、フルインストールで連携アプリをインストールします🚀

インストールの種別
HubSpotのTeams連携アプリでは次の2つのインストール方法があります。
① 限定インストール
② フルインストール

詳しくは[過去まとめたZennの記事]("https://zenn.dev/raynya/articles/bff14a3e9189d3#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E7%A8%AE%E5%88%A5")に書いていますが、

#### 💻 HubSpot×Teams連携の前提

HubSpot×Teams連携にあたっては以下の条件があります：
- HubSpotアカウントにはMicrosoft Teamsのインスタンスを1件のみ接続できます。
- [パーソナル版](https://www.microsoft.com/en-sg/microsoft-teams/teams-for-home) のMicrosoft TeamsアカウントをHubSpotに接続することはできません。
- HubSpotの異なる [データセンター](https://knowledge.hubspot.com/account-security/hubspot-cloud-infrastructure-and-data-hosting-frequently-asked-questions?hubs_content=knowledge.hubspot.com/ja/integrations/connect-hubspot-and-microsoft-teams&hubs_content-cta=%E3%83%87%E3%83%BC%E3%82%BF%E3%82%BB%E3%83%B3%E3%82%BF%E3%83%BC)間で同一のMicrosoft Teamsアカウントをインストールすることはできません。

📖 引用元
[HubSpot 公式ドキュメント](https://knowledge.hubspot.com/ja/integrations/connect-hubspot-and-microsoft-teams)

つまりは、
- 法人契約のTeamsライセンスが必要で
- １つのHubSpotのポータルに接続できるTeamsのテナントは１つまで
ということですね🏢

すでにTeamsが導入されている組織であれば、気にすることはないあたりだとは思います。
以下セクションから早速セットアップを始めていこうと思います👩‍💻

#### 📎 ステップ 1. Teams 管理センターでHubSpotアプリを許可する

まずは、Teams 側でHubSpotアプリの利用が許可されている状態にする必要があります。

ここでは、Teams 管理センターを用います。
※ Teamsの管理者権限がない場合は組織の管理者に操作を依頼するようにしましょう

Teams管理センターでは以下の３つが許可されている状態にします：
- A : 組織全体のアプリ設定 > [サードパーティ製アプリ] が オン
- B : HubSpot アプリ の設定 > ブロックされていない状態
- C : HubSpot アプリ の設定 > [ユーザーとグループ] > [全員] or [特定のユーザーまたはグループ] で利用対象ユーザーが許可されている状態

この記事では、[アプリ中心管理]("https://learn.microsoft.com/ja-jp/microsoftteams/app-centric-management")に移行したテナントでの操作方法を紹介しています
※ 従来のアプリ許可ポリシーでの方法は、[こちらの記事]("https://zenn.dev/raynya/articles/bff14a3e9189d3#microsoft-teams%E7%AE%A1%E7%90%86%E3%82%BB%E3%83%B3%E3%82%BF%E3%83%BC%E3%81%A7%E3%81%AE%E6%BA%96%E5%82%99")を参照ください

それぞれの設定箇所は以下の通りです📑

〇 A : 組織全体のアプリ設定 > [サードパーティ製アプリ] が オン

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_09%20(1).png)
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_08%20(1).png?width=3072&height=1679&name=teams-hubspot_08%20(1).png)

〇 B : HubSpot アプリ の設定 > ブロックされていない状態
下図のように HubSpot アプリがブロックされていない状態にします：
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_11%20(1).png?width=1344&height=496&name=teams-hubspot_11%20(1).png)

※ アプリがブロックされている場合、以下のような画面となります：
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_10%20(1).png?width=3072&height=1374&name=teams-hubspot_10%20(1).png)


〇 C : HubSpot アプリ の設定 > [ユーザーとグループ] > [全員] or [特定のユーザーまたはグループ] で利用対象ユーザーが許可されている状態

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_12%20(1).png?width=3072&height=1845&name=teams-hubspot_12%20(1).png)


#### 🍊ステップ 2. HubSpotでTeams連携アプリをインストールする

Teams側での許可が完了したら、HubSpot側でTeams連携アプリをインストールします🍊

インストールの操作を行うユーザーには、以下の権限が必要です：
- HubSpot : スーパー管理者 or マーケットプレイス権限
- Teams : グローバル管理者（フルインストールの場合）

手順は、以下の通りです📑

##### ① マーケットプレイスで「Microsoft Teams」のアプリをインストール
HubSpot でアプリマーケットプレイスにアクセスし、[「Microsoft Teams」のアプリ](https://app.hubspot.com/marketplace/23823306/listing/microsoft-teams-221635)をみつけます


![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_01%20(1).png?width=3072&height=1629&name=teams-hubspot_01%20(1).png)


［インストール］ボタンをクリックし、［フルインストール］を選択した状態で［インストール］をクリックします🖱️
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_02%20(1).png?width=708&height=900&name=teams-hubspot_02%20(1).png)


##### ② HubSpotのユーザーとTeamsのユーザーを紐づける

インストール後、HubSpotアプリの設定画面に遷移しますので、次には HubSpotのユーザー情報とTeamsのユーザー情報を紐づける作業を行います👥

HubSpotで使用されているEメールアドレスと、Teamsで使用されているEメールアドレスが一致している場合、この作業は不要です。

［全般設定］> ［自分の設定］タブに遷移し、［アカウントを接続］をクリック
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_04%20(1).png?width=3072&height=2198&name=teams-hubspot_04%20(1).png)


［Microsoft Teamsアカウント］の部分にTeamsにサインインするためのメールアドレス（UPN）を入力して、［リンク済みアカウントを更新］をクリックします
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_05%20(1).png?width=846&height=900&name=teams-hubspot_05%20(1).png)


##### ③ Teams側でHubspotのボットをVerifyする

HubspotのユーザーとTeamsのユーザーの紐づけが行われると、TeamsクライアントにHubspotのボットからメッセージが送られます💭
ボットから送られたメッセージ内にある［Verify］をクリックすると、
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_06%20(1).png?width=876&height=900&name=teams-hubspot_06%20(1).png)


ボットメッセージが、「こんにちは 👋 HubSpotのMicrosoft Teamsボットです」というメッセージに更新されます👋
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/teams-hubspot_07%20(1).png?width=876&height=900&name=teams-hubspot_07%20(1).png)


これで基本的なセットアップは完了です🌟

#### ❓連携をやめたい時は？

アンインストールの方法については、[こちらの記事]("https://zenn.dev/raynya/articles/bff14a3e9189d3#%E3%82%A2%E3%83%B3%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%AE%E6%96%B9%E6%B3%95")を参照ください

### 次は？▶️TeamsのチームをHubSpotに接続しよう

次回は本格的な連携のために、HubSpotにTeamsのチームを接続していきます🌱

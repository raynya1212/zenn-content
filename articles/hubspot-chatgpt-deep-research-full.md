---
title: "HubSpot×ChatGPT Deep Research：ウェビナー成果を爆速分析！プロンプト設計がカギ"
emoji: "🤖"
type: "tech"
topics: [HubSpot, ChatGPT, Deep Research, mcp]
published: true
published_at: 2025-06-08
---

# 【HubSpot×ChatGPT Deep Research】ウェビナー成果をChatGPTで爆速分析！🔍プロンプト設計がカギ✨

2025-06-08 · Reina

## はじめに 🌿
[HubSpot公式のニュース](https://www.hubspot.jp/openai-connector-20250605)で、**ChatGPT とつなげて CRM データを分析できる新機能**が発表されたのを見て、早速トライした記録です ✨
今回は、あるマーケティングイベントの分析をChatGPTにお願いしてみた記録です。
特に、「どういうプロンプトを投げればいい感じに分析してくれるか？」を中心に紹介していきます！🔍

## この記事でわかること ✏️
- HubSpot と ChatGPT の **Deep Research Connector** って何？
- **データ分析**で気をつけたい **プロンプトの書き方**
- **Starter プラン**でも活用できる分析活用シナリオ
- Deep Research が **得意なこと／苦手なこと**

## 結論 🎯
- **目的と要約項目を明確に書いたプロンプト**なら、ChatGPT が社内レポートを爆速でまとめてくれる！
- HubSpot とつないでおけば、**参加傾向や属性分析**も簡単に行える。読み取り専用なので安心 🍋

## シナリオの設定 🎬
分析対象：
今回はサンプルとして、架空のウェビナーデータをマーケティングイベントとして登録・架空のコンタクトを参加者としてインポートしておきました。

- イベント名：地域企業のためのChatGPT活用術 〜営業・サポート・業務効率化の実例紹介〜
- 開催日：2025年5月15日 14:00〜15:00
- 登録者：30名／参加者：16名／キャンセル：3名

![](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/chatgpt_hubspot%20(3)%20(1).png)

このイベントの登録データをもとに、HubSpot内にある情報をChatGPTで読み取ってもらい、参加傾向や属性などを分析してもらいます！
- 参考：
  - [マーケティングイベント参加者のインポート](https://knowledge.hubspot.com/ja/integrations/import-marketing-event-participants)
  - [HubSpot と ChatGPT Deep Research の接続](https://knowledge.hubspot.com/integrations/connect-your-hubspot-account-to-chatgpt#connect-hubspot-to-chatgpt-deep-research)

## Deep Research の使い方 🛠️
### 操作ステップ
1. ChatGPTの右上メニューから「ツール → Deep researchを実行」を選びます。
2. 「情報源 → HubSpot」を選び、HubSpotポータルで連携を許可します。
3. HubSpotがデータソースに追加されたら、ONにするだけで準備完了！

※ [GIFはナレッジベースより引用] (https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/Imported%20sitepage%20images/GIF%20showing%20how%20to%20connect%20HubSpot%20deep%20research%20connector.gif)

あとはプロンプトを入力するだけで、HubSpot内のデータを読み取りながら、ChatGPTが分析をしてくれます。

## プロンプトの書き方 🎤
何回か試してみたところ、プロンプトを明確に書く・どんなデータがほしいかをはじめに定義する というのが大事そうだなと感じました👀

### 😿 よくない例
地域企業のためのChatGPT活用術 〜営業・サポート・業務効率化の実例紹介〜 のウェビナーについて分析したい

→ 対象がふんわりしていて、ChatGPTが何をどうすればいいのか混乱してしまいます… 結果関係のないサンプルコンタクト(Brian Halligan)も拾ってしまっていました😿

![](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/chatgpt_hubspot%20(4)%20(1).png)


### 😸 良い例
HubSpotのマーケティングイベント「地域企業のためのChatGPT活用術 〜営業・サポート・業務効率化の実例紹介〜」について分析を行いたいです。

【目的】
今月開催したウェビナー「地域企業のためのChatGPT活用術 〜営業・サポート・業務効率化の実例紹介〜」の開催実績を社内報告向けにまとめたいです。

【まとめる内容】

登録者数とその属性（年齢、性別、会社名、地域、業種）

参加者数とその属性

キャンセル者数とその属性

登録者の登録タイミング（イベント何日前が多かったか）

【対象のコンタクト】
上記イベントに登録したコンタクトを対象に分析を行ってください。
https://app.hubspot.com/marketing-events/〜（省略）
https://app.hubspot.com/import/〜（省略）

→ 対象、目的、要約したい項目が明確で、ChatGPTが迷わず動いてくれます！
![](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/chatgpt_hubspot%20(2)%20(1).png)

⭐ 結果的に生成してくれたレポート

文中で作ったと書かれているグラフは生成されてないけど💦、報告資料につかうネタとしては充分な出来かなと！😼

👉 https://chatgpt.com/s/dr_68457c5d21c48191938353dd229b905a

## Starter プランでもグラフが作れる📊
HubSpotではカスタムレポート機能がProプラン以上でないと使えないのですが、
Deep Research機能を使うことでStarterプランでもグラフ・表の作成が可能✨

生成されたグラフは画像としてダウンロードもできるので、Slackや社内報告にも使いやすいです。


## 注意ポイント ⚠️
[FAQ](https://knowledge.hubspot.com/integrations/connect-your-hubspot-account-to-chatgpt#frequently-asked-questions)にある内容をもとに、注意したいことをまとめておきます👇

🔒 読み取り専用です：HubSpotデータを更新・削除されることはありません

👀 ユーザーの閲覧権限を尊重：自分がHubSpot上で見れない情報は、ChatGPTも見れません

The HubSpot deep research connector automatically respects the user permissions defined in HubSpot. This means users will only see in ChatGPT the CRM data they’re allowed to access in HubSpot.

💾 データ保持について：データがどのように扱われるか、についてはナレッジベースに詳しい記載があったので導入前にチェックしておくのがよさそうです！

## まとめ ✨
Deep Researchって、
💬「いまチームで気になってること」をちょっと聞いてみるだけで、
📊「データで答えてくれて、すぐ動けるアイデアまで出してくれる」頼れる相棒って感じです！


📌 カスタムレポートほど整った見た目じゃないけど、
「ざっくり分析したい」「全体像をパッと知りたい」ってときにはすごく便利✨

マーケも営業もCSもサポートも、いろんな場面で“次の一手”を見つけやすくなるから、
ふと迷ったときの相談相手にしてみるといいかもです🐱💡

HubSpot側はプランに限らず使えるから、まずはウェビナーデータなどで試してみてはいかがでしょうか🐱🎉

![](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/hubspot_chatgpt_deep_research_summary_corrected.png)

> 関連ドキュメント：
> - [Connect your HubSpot account to ChatGPT deep research（使い方とユースケース）](https://knowledge.hubspot.com/integrations/connect-your-hubspot-account-to-chatgpt#example-use-cases)
> - [HubSpot が CRM として初となる ChatGPT とのコネクタを発表](https://www.hubspot.jp/openai-connector-20250605)

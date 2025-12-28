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

![HubSpot ロゴ]({logo_url})

## はじめに 🌿
[HubSpot公式のニュース](https://www.hubspot.jp/openai-connector-20250605)で、**ChatGPT とつなげて CRM データを分析できる新機能**が発表されたのを見て、早速トライした記録です ✨

## この記事でわかること ✏️
- HubSpot と ChatGPT の **Deep Research Connector** って何？
- **データ分析**で気をつけたい **プロンプトの書き方**
- **Starter プラン**でも活用できる分析活用シナリオ
- Deep Research が **得意なこと／苦手なこと**

## 結論 🎯
- **目的と要約項目を明確に書いたプロンプト**なら、ChatGPT が社内レポートを爆速でまとめてくれる！
- HubSpot とつないでおけば、**参加傾向や属性分析**も簡単に行える。読み取り専用なので安心 🍋

## シナリオの設定 🎬
- 分析対象：ウェビナーのマーケティングイベントと参加者インポート
- 参考：
  - [マーケティングイベント参加者のインポート](https://knowledge.hubspot.com/ja/integrations/import-marketing-event-participants)
  - [HubSpot と ChatGPT Deep Research の接続](https://knowledge.hubspot.com/integrations/connect-your-hubspot-account-to-chatgpt#connect-hubspot-to-chatgpt-deep-research)

## Deep Research の使い方 🛠️
- ChatGPT の **ツール → Deep research** を実行
- 情報源に **HubSpot** を指定し、HubSpot ポータルで連携を許可
- 以降は **プロンプト入力**で、HubSpot 内データを読み取りながら分析実行

## プロンプトの書き方 🎤
**よくない例**（対象がふんわりしていて、不要なコンタクトまで拾う）

**良い例**（目的／要約項目／対象を明確に定義）
- 目的：開催実績を社内報告向けに整理
- 要約項目：登録者／参加者／キャンセル数の属性（年齢・性別・会社名・地域・業種）／登録タイミング
- 対象コンタクト：上記ウェビナーに登録したコンタクト

## Starter プランでもグラフが作れる📊
Deep Research を使えば、**Starter プラン**でもグラフ・表の作成が可能。画像としてダウンロードでき、Slack や社内報告にも使いやすい。

## 注意ポイント ⚠️
- **読み取り専用**：HubSpot データを更新／削除しない
- **ユーザー権限を尊重**：HubSpot 上で見られない情報は ChatGPT でも見えない
- **データ保持**については、導入前にナレッジベースの記載を確認しておくのが安心

## まとめ ✨
- Deep Research は **設定も分析も手軽**！
- 指示次第で **データの見え方が激変**
- カスタムレポートの代わりに **ざっくり把握→社内共有**にぴったり

> 関連ドキュメント：
> - [Connect your HubSpot account to ChatGPT deep research（使い方とユースケース）](https://knowledge.hubspot.com/integrations/connect-your-hubspot-account-to-chatgpt#example-use-cases)
> - [HubSpot が CRM として初となる ChatGPT とのコネクタを発表](https://www.hubspot.jp/openai-connector-20250605)

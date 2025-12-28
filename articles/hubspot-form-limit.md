---
title: "HubSpotフォームに申込み人数・期間制限をつけるには？こんなふうに解決してみたよ"
emoji: "💭"
type: "tech"
topics: [HubSpot]
published: true
published_at: "2025-04-29"
---
# HubSpotフォームに申込み人数・期間制限をつけるには？こんなふうに解決してみたよ

2025-04-29 · Reina

## はじめに 🌿

HubSpotのフォーム、便利だけど、一つだけもやもやしちゃうこと…

それは「送信上限がつけられない」ってこと！

イベントやワークショップで、予定人数を超えたくない場合もあるのに...って思ったり、申込み期限を見計らってページを編集にしにいくのめんどくなったり…🌀

そこで～、このもやもやを「開発」で解決してみたよ！✨

![ChatGPT Image 2025年4月29日 22_17_44 (1)](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8829%E6%97%A5%2022_17_44%20(1).png?width=700&height=466&name=ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8829%E6%97%A5%2022_17_44%20(1).png)

---

## この記事でわかること ✍️

* HubSpotフォームの現状は？
* 実際に運用しててどんな問題があるか
* 私が実践した「解決方法」の簡単なイメージ

---

## 結論 🎯

HubSpotのフォームは、正式には送信上限をつけられない。

だけど、HubDB＆サーバレス関数を使えば、自分たちで「上限チェック＆フォーム自動クローズ」を作れるよ！

## もやもやポイントと実際の欲しい機能 🌈

### どうして送信上限が欲しかった？

こんなシチュエーションありますよね💭

* ワークショップやイベントで、予定人数を超えると困る  
  例：会場の容量、料金の計算、アレンジを超える問題
* 単純に申込み期限を超えたらフォーム送信ができないようにしたい

### HubSpotフォームの現状は？

* [HubSpot公式ドキュメント](https://knowledge.hubspot.com/ja/forms/forms-faq#%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%81%AB%E9%80%81%E4%BF%A1%E5%88%B6%E9%99%90%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%AF%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%81%8B%EF%BC%9F)にもある通り：

  + 「任意で送信制限を設定はできない」
  + フォームを設置しているページからフォームを削除するしかない

やりたいことを実現するには、自分たちで「仕組み」を考える必要があったよ！

## 私が実践した解決方法サクっと答える ✨

### 使ったもの

* HubDBで送信数を管理
* サーバレス関数でHubDBを更新
* hbspt.formsのイベント(onFormSubmit)を利用
* HubLのif文でフォーム表示を制御

### いまさら思うこと

「普通に機能として実装されてもいいのに～～！！！」って思いながら（笑）  
👇の idea post のステータスが更新されることをとってもお祈りしてます  
[Limit form submission to a certain number](https://community.hubspot.com/t5/HubSpot-Ideas/Limit-form-submission-to-a-certain-number/idi-p/31846)

だけど、こんな実装を通してHubSpotの機能を使いこなすのも、ひとつかな🍪

---

## あわせて読みたい 📖

実装編はこちら：

https://zenn.dev/raynya/articles/hubdb-serverless-limit-full


むりなく楽しく、HubSpotとつき合っていこうねっ～～～！✨
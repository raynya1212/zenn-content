---
title: "【HubSpot】HubSpot CLI v7 の変更点まとめ"
emoji: "🛠️"
type: "tech"
topics: [HubSpot,HubSpot CLI, HubSpot CMS]
published: true
published_at: 2025-04-19
---

# 【HubSpot】HubSpot CLI v7 の変更点まとめ 🔐

2025-04-19 · Reina

![ChatGPT Image](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8819%E6%97%A5%2020_42_47.png)

### はじめに ✨
2025年4月、HubSpot CLI に関する大幅なアップデート（v7.4.0）が発表されました！ 特に注目すべきは、設定ファイルの管理方法がこれまでと大きく変わったこと。

この記事では、新しくなった設定ファイルの構成や、旧ファイルからの移行方法、開発フローへの影響などを、わかりやすく日本語でまとめてみました📝

https://developers.hubspot.com/changelog/new-global-configuration

### 🌙 これまでの設定：hubspot.config.yml にぜんぶ書いてた
今までは、プロジェクトごとに hubspot.config.yml というファイルを用意して、そこに「アカウントID」や「アクセスキー」を直接書いていました。
```yaml
defaultPortal: 12345678
portals:
  - name: my-dev
    portalId: 12345678
    authType: personalaccesskey
    personalAccessKey: "xxxxx-xxxxx"
```

この方法、わかりやすい反面…

🔓 アクセスキーがそのまま書かれていてちょっとリスク！

🤔 Git 管理下だとキーの流出リスクがある

🌀 プロジェクトごとに設定が違うと、混乱しやすい


### 🌟 新しい設定：config.yml + .hsaccount の分離スタイルに！

v7.4 からは、設定ファイルが2つにわかれました👇

|ファイル名|内容|場所|安全性|
|---|---|---|---|
|`~/.hscli/config.yml`|アクセスキーなど個人設定|ローカル|🔒 プロジェクト外|
|`.hsaccount`|アカウントIDのみ|プロジェクトフォルダ内|🧊 PIIなしで安心|

これにより、アクセスキーなどの機密情報はすべてローカルに隔離されるようになり、
プロジェクト側にはIDしか残らない＝Gitに上げても大丈夫！という安心な設計に変わったんです。

### 📂 .hsaccount の中身は超シンプル
```text
12345678
```
のようなアカウントのIDだけ。

ほんとにこれだけ。これで「このプロジェクトではこのアカウントを使うよ」と CLI に教えてあげられます。
個人情報やキーは一切入っていないので、セキュリティ的にも○


### 🛠️ 実際にやってみよう（初期設定の手順）
１ HubSpot CLI を最新にアップデート
```bash
npm install -g @hubspot/cli@latest
```
２ アカウントに認証
```bash
hs account auth
```
３ プロジェクト内で .hsaccount を作る
```bash
hs account create-override
```
もし過去の hubspot.config.yml を使っていた場合は、以下のコマンドでマイグレーションできます👇
```bash
hs config migrate
```
https://developers.hubspot.com/docs/developer-tooling/local-development/hubspot-cli/reference

### 💞 新スタイルのうれしいポイント
✅ アクセスキーがプロジェクトファイルに含まれないから 安心

✅ 設定が一元化されて 管理しやすい

✅ .hsaccount を使えば 複数プロジェクトの切り替えもスムーズ

### 💡 補足情報
現時点（2025年4月時点）では、Visual Studio Code の HubSpot 拡張機能は 上記新スタイルに正式対応していないようでした。。

（まあターミナルで打てばいいか 💭）

### 📌 まとめ
| これまで | これから |
| ---- | ---- |
| hubspot.config.yml に全部書く | ~/.hscli/config.yml + .hsaccount に分離 |
| アクセスキーが書かれてて怖い | アクセスキーはローカル保存で安心 | 
| Gitに上げると危険 | .hsaccount は上げてもOK | 
 
### 🐾 おわりに
今回のアップデートは、HubSpot CLI を安全かつ快適に使っていくうえで、すごく大事な進化でした！
HubSpot開発の環境、まだまだ進化していくので、これからも追いかけていきたいですね〜🐱✨
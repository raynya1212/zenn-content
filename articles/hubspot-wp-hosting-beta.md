---
title: "【HubSpot】WordPressサイトのサブディレクトリでHubSpotコンテンツをホスティングする方法（ベータ）】"
emoji: "📂"
type: "tech"
topics: [HubSpot,WordPress,HubSpot CMS]
published: true
published_at: 2025-04-16
---


# 【HubSpot】WordPressサイトのサブディレクトリでHubSpotコンテンツをホスティングする方法（ベータ）📂

2025-04-16 · Reina

## 🌱 はじめに

2025年春、HubSpotからついに待望のベータ機能が登場しました！  
その名も「[**WordPressサブフォルダーでのHubSpotコンテンツのホスティング**（ベータ）](https://knowledge.hubspot.com/ja/integrations/install-the-hubspot-wordpress-plugin?hubs_content=knowledge.hubspot.com/integrations/install-the-hubspot-wordpress-plugin&hubs_content-cta=%E6%97%A5%E6%9C%AC%E8%AA%9E#wordpress%E3%82%B5%E3%83%96%E3%83%95%E3%82%A9%E3%83%AB%E3%83%80%E3%83%BC%E3%81%A7%E3%81%AEhubspot%E3%82%B3%E3%83%B3%E3%83%86%E3%83%B3%E3%83%84%E3%81%AE%E3%83%9B%E3%82%B9%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0-%E3%83%99%E3%83%BC%E3%82%BF)」🌿

これまで、**HubSpotで既存のドメインを用いてページを公開するには、別途サブドメインを用意する必要がありました。**  
たとえば `blog.example.com` のように、**既存のサイトとは別の“もうひとつのサイト”のような扱い**になっていたんです。

でも、実際によくあるのはこんなケース：

`example.com` はWordPressで作られたコーポレートサイト。  
そこにマーケチームが「`example.com/blog` はHubSpotで作って運用したいな」となったとき、  
本当は同じサイト内の一部として見せたいのに、サブドメイン（`blog.example.com`）になってしまうことで、ドメイン構造上 “別のサイト” に見えてしまう。

訪問者にとっても「URLが変わった？」という違和感が生まれるほか、  
SEO的にもサブディレクトリと比べて評価が分かれてしまう可能性がありました💭

![ChatGPT Image 2025年4月13日 20_09_12](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8813%E6%97%A5%2020_09_12.png)

一応、[リバースプロキシーを駆使](https://developers.hubspot.jp/docs/guides/cms/content/performance/reverse-proxy-support)すれば `example.com/blog` をHubSpotでホスティングすることも可能でしたが、  
**サーバー構成やルーティング設定の知識が必須で、運用ハードルがとても高かった**のが実情です…。

その点、この新しいベータ機能は**WordPressのプラグインだけで設定が完結**するので、圧倒的に導入しやすくなっています👏✨  
今回はを例に、この機能の使い方を紹介します。👩‍🏫

![](https://hubspot-product-updates.s3.amazonaws.com/prod/images/1743774964686/59dc469e-ed27-411b-b935-a79b38e68d23)

※ベータ機能ページの画像を引用

> ⚠️ **注意：この機能は現在ベータ版です**  
> 利用には**ベータ機能へのオプトイン**が必要で、将来的に仕様や設定方法が変更される可能性があります。

## 💡 このベータ機能でできるようになること

* WordPressの**任意のパスでHubSpotのページをホスティング可能**
* DNSやリバースプロキシーの手動設定不要（プラグインが自動で処理）
* HubSpot CMSの機能（フォーム、CTA、A/Bテストなど）をWordPress内で活用できる
* サブディレクトリでの公開のため、SEO的にも自然なURL構造が維持できる

### 🧩 実際に試してわかった注意点

今回 www なしのドメイン（ルートドメイン）を使って設定を試みたところ、  
HubSpot側でこのドメインを「**公開準備完了（Ready for publishing）**」状態に設定できず、最終的にページ側で**403エラー**になってしまいました。😬（これは私の環境だけなのかもしれませんが）

HubSpotのナレッジベースには「DNS設定は不要」と明記されているものの、  
実際の挙動としてはルートドメインではDNSレコードの確認をスキップできず、ベータ機能に必要な設定が完了できない動作となるようです。

そのため、以下では `www` ありのサイト構成にてテストしています🧩

## 📁 サイトの構成

今回は以下のような構成を想定しています：

| パス | ホスティング元 | 用途 |
| --- | --- | --- |
| `https://www.example.com/` | WordPress | サイト全体・メインのコーポレート情報 |
| `https://www.example.com/campaign` | HubSpot | ランディングページや特定コンテンツ用 |

WordPressを軸にしつつ、一部コンテンツだけをHubSpotで柔軟に作成・運用するハイブリッドな形です。

## 🔧 設定方法

### 🧰 事前準備

#### 🔹WordPress側

* [HubSpot公式プラグイン](https://wordpress.org/plugins/leadin/?)をインストール＆有効化  
  → 管理画面左メニュー「プラグイン」から追加可能
* HubSpotアカウントと連携しておく

#### 🔸HubSpot側

* 「HubSpotのコンテンツをWordPressのサブフォルダーでホスティング」ベータ機能にオプトインする

![製品の最新情報-HubSpot-04-13-2025_07_59_PM](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/%E8%A3%BD%E5%93%81%E3%81%AE%E6%9C%80%E6%96%B0%E6%83%85%E5%A0%B1-HubSpot-04-13-2025_07_59_PM.png)

* **使用するドメインを追加**  
  → DNS設定は不要。「**公開の準備完了**」の状態にしておけばOK

---

🔍 設定の流れ：

1. 設定>コンテンツ>ドメインとURL からドメインの接続を進める
2. DNS 設定画面までたどり着いたら、[後で完了] を選択して設定画面は一度閉じる
3. ドメイン一覧画面にて[アクション] > [**公開の準備完了を設定]** を選択する  
   ![Settings-04-13-2025_07_18_PM (1)](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/Settings-04-13-2025_07_18_PM%20(1).png)
4. ドメイン下に 「🟡 公開の準備完了」 が表示されていたらOK  
   ![Settings-04-14-2025_10_21_PM](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/Settings-04-14-2025_10_21_PM.png)

---

* **対象パス（例：/hs）でページを作成・公開**しておく  
  → LPやブログなど、表示させたいページをあらかじめ準備！

---

🔍 補足：

HubSpotでページを作成して、👆で公開準備にしたドメインを選択します（この段階ではページは閲覧できない状態です）

![ランディングページ|HubSpot-04-16-2025_11_32_PM](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/%E3%83%A9%E3%83%B3%E3%83%87%E3%82%A3%E3%83%B3%E3%82%B0%E3%83%9A%E3%83%BC%E3%82%B8%7CHubSpot-04-16-2025_11_32_PM.png)

---

### ⚙️ 設定（WordPressプラグイン内）

1. **WordPress管理画面で [HubSpot] → [設定] を開く**
2. 上部タブから **[ドメイン]** を選択
3. [**WordPressドメインでHubSpotコンテンツをホストする]**をオンにする  
   → これでWPサイト内の特定パスにHubSpotページを出せるようになります！
4. 必要なルーティング情報を入力👇

| 項目 | 内容 |
| --- | --- |
| ドメイン | 使用しているドメイン |
| **WordPressパス** | HubSpotページを表示させるパス（例：`hs`や `hs/*`） |
| **HubSpot パス** | 上記と同じパスが自動で入ります |

💡 アスタリスク（\*）を使うことで、**複数ページをまとめてルーティング**できます。また、  
例：`/hs/*` → `/hs/sample-page`, `/hs/another-page` などすべてを対象に。

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/%E8%A8%AD%E5%AE%9A-%E2%80%B9-%E3%81%A1%E3%82%83%E3%82%8D%E3%81%AE%E3%82%B5%E3%82%A4%E3%83%88-%E2%80%94-WordPress-04-16-2025_11_01_PM.png?width=3182&height=1362&name=%E8%A8%AD%E5%AE%9A-%E2%80%B9-%E3%81%A1%E3%82%83%E3%82%8D%E3%81%AE%E3%82%B5%E3%82%A4%E3%83%88-%E2%80%94-WordPress-04-16-2025_11_01_PM.png)

---

🔍 補足：

[**WordPressドメインでHubSpotコンテンツをホストする]**のトグルをオンにしても反応がないときは、WordPressの他のプラグインと干渉している可能性があります⚡ 私が検証したタイミングでは、**CloudSecure WP Security**との相性が悪いようでした…

---

### 

### ✅ 完了するとどうなるか

設定がうまくいけば…

📍 上記で設定したURL（HubSpotで作成したページ）にアクセスすると、  
そのページは**HubSpotで作成・公開されたコンテンツ**として表示されます！

* ドメインは WordPressサイトのドメインのまま
* 表示されるコンテンツはHubSpot側のもの
* 表面的にはWordPressサイトの1ページのように見える👀

訪問者にはまったく違和感なく、**自然な形でHubSpotページが組み込まれます**✨

![Desktop-screenshot-04-16-2025_11_09_PM](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/Desktop-screenshot-04-16-2025_11_09_PM.png)

## 📝まとめ

今までは、リバースプロキシという高い壁があって、HubSpotでページを使うには  
別ドメインやサブドメインを用意するのが当たり前でした。

でもこのベータ機能があれば、  
**WordPressサイトの構造を変えることなく、特定のサブディレクトリだけをHubSpotでホスティングできる**ようになります！

![ChatGPT Image 2025年4月13日 20_16_10](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8813%E6%97%A5%2020_16_10.png)

「ちょっとしたキャンペーンLPだけ作りたい」「一部だけHubSpotで運用したい」  
そんなニーズにぴったりの機能です🐾

> 🧪 ベータ機能のため、動作が不安定だったり、動作/設定方法が今後変更になる可能性もあります。  
> 本格運用の前には、テスト環境などでの確認をおすすめします！
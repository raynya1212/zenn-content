---
title: "【HubSpot】カードリンク（ブログカード）を作ってみよう 🔗"
emoji: "📝"
type: "tech"
topics: [HubSpot, CMS, Serverless]
published: true
published_at: 2025-03-16
---

# 【HubSpot】カードリンク（ブログカード）を作ってみよう 🔗

2025-03-16 · Reina

### やりたかったこと 💬

HubSpotのブログやページで、URLを入力すると自動でOGP情報（タイトル・画像）を取得して、カードスタイルで表示するモジュールを作りたかった！

⭐いわゆる [カードリンク（ブログカード）](https://rank-quest.jp/column/column/blogcard/#:~:text=%E3%83%96%E3%83%AD%E3%82%B0%E3%82%AB%E3%83%BC%E3%83%89%E3%81%AF%E3%80%81%E3%83%AA%E3%83%B3%E3%82%AF%E3%81%AE%E8%A8%AD%E7%BD%AE%E6%96%B9%E6%B3%95%E3%81%AE1%E3%81%A4%E3%81%A7%E3%81%99%E3%80%82%E3%83%AA%E3%83%B3%E3%82%AF%E5%85%88%E3%81%AE%E3%82%A2%E3%82%A4%E3%82%AD%E3%83%A3%E3%83%83%E3%83%81%E7%94%BB%E5%83%8F%E3%82%84%E3%82%BF%E3%82%A4%E3%83%88%E3%83%AB%E3%80%81%E8%AA%AC%E6%98%8E%E6%96%87%E3%81%AA%E3%81%A9%E3%82%92%E3%81%BE%E3%81%A8%E3%82%81%E3%81%A6%E3%82%AB%E3%83%BC%E3%83%89%E5%BD%A2%E5%BC%8F%E3%81%A7%E8%A1%A8%E7%A4%BA%E3%81%97%E3%81%BE%E3%81%99%E3%80%82) みたいなやつ

### 🛠 やったことまとめ

#### ① サーバーレス関数を使って OGP 情報を取得

👇の記事にも書かれているように、フロントエンド側だけだと [CORS エラー](https://developer.mozilla.org/ja/docs/Glossary/CORS)が起きてしまうのでサーバーサイドの処理が必要のよう… （あんまりわかってないけど）

https://zenn.dev/tomi/articles/2021-03-22-blog-card


今回は、[HubSpot のサーバーレス関数](https://developers.hubspot.jp/docs/guides/cms/content/data-driven-content/serverless-functions/overview) を使って、指定したURLの OGP データを取得する流れを作った！
- HubSpot のサーバーレス関数（Node.js）を設定
- fetch でページの OGP 情報（og:title, og:image）を取得
- CORS の問題を回避するためにサーバーレス関数経由でリクエスト

📌 serverless.json の設定
```json
{"runtime":"nodejs18.x","version":"1.0","endpoints":{"fetch-ogp":{"method":"GET","file":"fetch-ogp.js"}}}
```

📌 fetch-ogp.js のコード（OGP取得関数）
```js
exports.main = async (context, sendResponse) => {
  const url = context.params.url;
  if (!url) {
    sendResponse({ statusCode: 400, body: "URLが必要です" });
    return;
  }

  try {
    const response = await fetch(url, {
      headers: { "User-Agent": "Mozilla/5.0" }
    });
    const html = await response.text();
    sendResponse({
      statusCode: 200,
      body: html,
      headers: { "Content-Type": "text/html", "Access-Control-Allow-Origin": "*" }
    });
  } catch (error) {
    sendResponse({ statusCode: 500, body: "OGP取得失敗" });
  }
};
```

このエンドポイントを https://自分のドメイン/_hcms/api/fetch-ogp?url=取得したいページのURL で呼び出せるようにした！

#### ② カスタムモジュールで OGP カードを表示する

HubSpotのカスタムモジュールで、取得したOGPデータをカードスタイルで表示！

カスタムモジュールのフィールドは
- ページURL（URL）
- ページテキスト（テキスト）
- サムネイル画像（画像）
の3つで構成

📌 module.css
```css
/* 📌 OGPカード全体 */
.ogp-card {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: #333;
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
  width: 100%;
  max-width: 600px; /* 最大幅を設定 */
  margin: 10px auto; /* カード同士に余白をつけて中央寄せ */
  border: 1px solid #ddd; /* グレーの枠線 */
}

/* 📌 OGPカードのレイアウト */
.ogp-card a {
  display: flex;
  width: 100%;
  align-items: center;
  text-decoration: none;
}

/* 📌 サムネイル画像 */
.ogp-card .og-image {
  width: 130px; /* 画像サイズ大きめ */
  height: 130px;
  object-fit: cover;
  border-radius: 8px 0 0 8px;
}

/* 📌 テキストエリア */
.ogp-card .card-text {
  padding: 10px 15px;
  flex-grow: 1;
  display: flex;
  align-items: center;
}

/* 📌 タイトル */
.ogp-card .og-title {
  font-size: 18px;
  font-weight: bold;
  line-height: 1.4;
  text-decoration: none;
  border-bottom: 2px solid transparent;
  transition: border-color 0.2s ease-in-out;
}

/* 🎯 📱 スマホ対応（横並び → 縦並びに変更） */
@media (max-width: 600px) {
  .ogp-card a {
    flex-direction: column; /* 縦並びにする */
    align-items: flex-start;
  }

  .ogp-card .og-image {
    width: 100%; /* 幅いっぱいに */
    height: auto;
    border-radius: 8px 8px 0 0;
  }

  .ogp-card .card-text {
    padding: 12px;
  }

  .ogp-card .og-title {
    font-size: 16px; /* スマホでは少し小さく */
  }
}
```

📌 module.js（Portal ID は伏せてます！）
```js
document.addEventListener("DOMContentLoaded", function () {
  console.log("window.fetchOGP の状態:", window.fetchOGP);

  if (typeof window.fetchOGP !== "function") {
    console.error("fetchOGP 関数が定義されていません！");
    return;
  }

  const HUBSPOT_DOMAIN = window.location.origin;
  const PORTAL_ID = "XXXXXX";
  const FUNCTION_PATH = "/_hcms/api/fetch-ogp";

  document.querySelectorAll(".ogp-card").forEach((ogpCard, index) => {
    let pageUrlData = ogpCard.dataset.pageUrl;
    console.log(`取得したページURLデータ (${index + 1}):`, pageUrlData);

    let finalUrl = "";
    const hrefMatch = pageUrlData.match(/href=(https?:\/\/[^\s,]+)/);
    if (hrefMatch) {
      finalUrl = hrefMatch[1];
    } else {
      console.warn(`ページURLを解析できませんでした (${index + 1})。デフォルトの値を使用します。`);
      finalUrl = pageUrlData;
    }

    console.log(`最終的なページURL (${index + 1}):`, finalUrl);
    if (!finalUrl || finalUrl.trim() === "") {
      console.error(`ページURLが空です！リクエストを送信できません (${index + 1})。`);
      return;
    }

    const API_URL = `${HUBSPOT_DOMAIN}${FUNCTION_PATH}?portalId=${PORTAL_ID}&url=${encodeURIComponent(finalUrl)}`;

    const fallbackTitle = ogpCard.dataset.fallbackTitle;
    const fallbackImage = ogpCard.dataset.fallbackImage;

    window.fetchOGP(API_URL, fallbackTitle, fallbackImage, ogpCard);
  });
});

window.fetchOGP = async function (apiUrl, fallbackTitle, fallbackImage, ogpCard) {
  try {
    console.log(`OGP取得開始: ${apiUrl}`);

    const response = await fetch(apiUrl);
    if (!response.ok) {
      throw new Error(`HTTPエラー: ${response.status}`);
    }

    const textData = await response.text();
    const parser = new DOMParser();
    const doc = parser.parseFromString(textData, "text/html");

    let title = doc.querySelector('meta[property="og:title"]')?.content || fallbackTitle;
    let image = doc.querySelector('meta[property="og:image"]')?.content || fallbackImage;

    ogpCard.querySelector(".og-title").textContent = title;
    ogpCard.querySelector(".og-image").src = image;
    ogpCard.querySelector(".og-link").href = apiUrl;

    console.log(`OGP取得成功: タイトル="${title}", 画像="${image}"`);
  } catch (error) {
    console.error("OGP情報の取得に失敗しました", error);

    ogpCard.querySelector(".og-title").textContent = fallbackTitle;
    ogpCard.querySelector(".og-image").src = fallbackImage;
  }
};
```

### 

### 💡 まとめ

結果的に、モジュールの ［URL］フィールドに指定されたページの OGP 情報から ページタイトル・サムネイル画像を引っ張ってきて表示させるカスタムモジュールを作れた🌥

もし、OGPで取得できなかった場合はフィールドに手動で指定することもできるのでそっちの情報を出すことも可能

![](https://blog.ryamagami.com/hs-fs/hubfs/card-link.png?width=1920&height=1232&name=card-link.png)

✅ HubSpotのサーバーレス関数を使ってOGP情報を取得
✅ カスタムモジュールでOGP情報をカード形式で表示（複数設定にも対応！）
✅ レスポンシブ対応・デザイン調整

サーバーレス関数は HubSpot CLI 使うことが前提だから、ちょっととっつきにくいけど HubSpotのカスタムモジュールって、サーバーレス関数を組み合わせるとこんなこともできるんだな〜って実感💡✨ また他にもいろいろ試してみたいなって思った🔥

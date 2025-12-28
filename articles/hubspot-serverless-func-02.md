---
title: "【HubSpot】はじめてのHubSpotサーバーレス関数（中編）：公開APIと連携してみよう 🧪"
emoji: "🧪"
type: "tech"
topics: [HubSpot, Serverless, API]
published: true
published_at: "2025-03-26"
---

# 【HubSpot】はじめてのHubSpotサーバーレス関数（中編）：公開APIと連携してみよう 🧪

2025-03-26 · Reina

### ✅ はじめに
前編では、HubSpotのサーバーレス関数を使って「Hello World」を返す関数をつくってみました。

関数をフォルダで管理したり、serverless.json でエンドポイントを指定したり、
HubSpotでちょっとしたサーバー処理ができることを体験できたと思います🧪

中編ではいよいよ、外部の公開API（The Cat API）を fetch してデータを取得することにチャレンジします！

サーバーレス関数の本領発揮は、やっぱり「どこかのAPIと連携してデータを使う」とき✨

今回は
🐱 かわいい猫の画像を外部APIから取得して
🎨 その画像をHubSpotのページに表示する
というところまでをゴールに進めていきます！

「サーバーとやり取りできるってこういうことか〜！」と実感できる、やさしくて実用的なステップなので、
ぜひ一緒に楽しみながらやってみましょう🐾💻🌸

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8826%E6%97%A5%2022_43_42.png?width=700&height=700&name=ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8826%E6%97%A5%2022_43_42.png)

---
## 🐱 The Cat API を使うよ
今回使うのは、The Cat APIという公開APIです。
アクセスするたびに、いろんな猫ちゃんの画像を返してくれるという、癒しのAPI🐾

公式でも “Cats as a service” というキャッチコピーを使っていて、遊び心も満点です！

https://thecatapi.com/


## 🌐 こんなAPIもあるよ（気軽に使える公開APIの例）
JSONPlaceholder：ダミーの投稿・ユーザー情報など（テスト用途にぴったり）

Dog API：犬画像が返ってくるAPI 🐶

Bored API：退屈な時にやることを提案してくれるAPI🎲

OpenWeatherMap：天気API（APIキー必須だけど無料プランあり）

## 🧪 今回使うエンドポイント
https://api.thecatapi.com/v1/images/search

このURLに GET リクエストを送ると、ランダムな猫画像が1件返ってきます。

**レスポンス例**
```json
[
  {
    "id": "abc123",
    "url": "https://cdn2.thecatapi.com/images/abc123.jpg",
    "width": 1200,
    "height": 800
  }
]
```
画像のURLは url に入っています。これを使えば、画像としてページに表示することもできます🐱

### 🔑 認証は…今回はなしでOK！
The Cat API には無料のAPIキーがありますが、
公式ドキュメントの Quick start にもあるように、
APIキーなしでも動作すると明記されています。

🐾 今回は、ドキュメントにも載っている Quick start（APIキーなし） の方法で進めてみます！

https://developers.thecatapi.com/view-account/ylX4blBYT9FaoVd6OhvR?report=bOoHBz-8t

---
## 📝 関数を作ってみよう！
ではさっそく、Cat API を呼び出すサーバーレス関数を作ってみましょう！

### 📁 フォルダ構成を用意する
まずは、以下のようなフォルダを用意します👇（基本的な流れは前編と同じ！）
```
fetch-cat/
└── fetch-cat.functions/
    ├── fetch-cat.js
    └── serverless.json
```

### 🐾 fetch-cat.js（関数の本体）
```js
// fetch-cat.js
exports.main = async (context = {}, sendResponse) => {
  try {
    const response = await fetch('https://api.thecatapi.com/v1/images/search');
    const data = await response.json();

    sendResponse({
      statusCode: 200,
      body: JSON.stringify(data),
    });
  } catch (error) {
    sendResponse({
      statusCode: 500,
      body: JSON.stringify({ error: 'Cat API fetch failed' }),
    });
  }
};
```

#### 🔍 解説
- fetch() でCat APIにアクセス

- await response.json() でレスポンスの中身を取り出す

- そのまま sendResponse() で返すことで、ページからアクセスできるようにします

#### 🐌 さらに解説！
**✅ await ってなに？**
```
const response = await fetch('https://api.thecatapi.com/v1/images/search');
```
await は「この処理が終わるまで待つ」という意味！
👉 APIにリクエストして、返事が返ってくるまで待つ って意味

**✅ response.json() ってなに？**
```
const data = await response.json();
```
APIの返してきたレスポンス本文を、JSONとして取り出すという意味
👉 The Cat API は JSON 形式でデータを返してくれるから、それをちゃんと使える形にするために .json() を使ってる🐱

**✅ sendResponse({ statusCode: 200, body: ... }) は？**

statusCode: 200 は「成功したよ！」っていうHTTPの合図
body に返すデータ（この場合は猫画像の情報）を文字列で渡してる
👉 JSON データを返すには、JSON.stringify(...) で文字列に変換する

**✅ catch (error) { ... } の中身は？**
```
catch (error) {
  sendResponse({
    statusCode: 500,
    body: JSON.stringify({ error: 'Cat API fetch failed' }),
  });
}
```
try { ... } で fetch の処理をして
もしエラー（通信失敗、APIの返答が不正など）が起きたら
catch に入り、500（サーバーエラー）とエラーメッセージを返す
👉 問題があった時にユーザー側で「うまくいかなかった」ってわかるようになってる 🐾

### 📄 serverless.json（関数の設定ファイル）
```json
{
  "runtime": "nodejs18.x",
  "version": "1.0",
  "endpoints": {
    "fetch-cat": {
      "method": "GET",
      "file": "fetch-cat.js"
    }
  }
}
```
これで /fetch-cat.functions/ の中にある fetch-cat.js を
/_hcms/api/fetch-cat というURLで呼び出せるようになります！

---
## 🌎 関数を呼び出して確認しよう！
関数と設定ファイルの準備ができたら、さっそくHubSpotにアップロードして、実際に動かしてみましょう🧪✨

### 🚀 アップロード
```bash
hs upload
```
コマンドを使って、デザインマネージャー上にアップロードしましょう
![](https://blog.ryamagami.com/hs-fs/hubfs/serverless-func11.png?width=1700&height=814&name=serverless-func11.png)

### 🌐 URLでアクセス
アップロードが完了したら、以下のURLにアクセスします👇

```
https://あなたのドメイン/_hcms/api/fetch-cat
```
成功していれば、こんな👇JSONが表示されるはずです🎉
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/serverless-func12.png?width=1700&height=316&name=serverless-func12.png)

✅ 実際の画像URLをブラウザで開くと…かわいい猫ちゃんに会えます🐱💕
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/serverless-func13.png?width=1300&height=940&name=serverless-func13.png)

### ❓うまくいかない時は…
- 関数名や serverless.json の "file" が間違ってないか確認

- 関数がアップロードされた先のパスと、URLのパス（fetch-cat）が一致してるか確認

- 開発者ツールのconsole にエラーが出ていれば、catch の部分で返されたメッセージを参考にする

---
## 🎨 取得した画像をページに表示してみよう！
せっかく猫ちゃんの画像URLが取れたので、それをページに表示してみましょう！
ここでは、HubSpotのページ内に JavaScript を使って猫画像を埋め込む方法を紹介します💡

### ✅ どんな仕組み？
HTMLに表示エリア（div）を用意しておいて

JavaScriptの fetch() で /fetch-cat 関数を呼び出して

取得した画像URLを <img> タグに反映する
という流れです🐾

### ✨ 表示の方法
今回はテスト用のページテンプレートを作ってそこに表示させるコードを入れてこうと思います🐾（もちろん既存のページテンプレートやカスタムモジュールで応用してもOK！）

1. デザインマネージャーを開く
2. 新しいテンプレートを作成
3. 📄 名前：fetch-cat.html とか！
4. テンプレートに以下を貼り付けて保存＆プレビュー✨

### 🧪 テンプレート例 
👇はボイラープレート テンプレートを使用した場合
```html
{% extends "./layouts/base.html" %}

{% block body %}
<div style="padding: 2rem; text-align: center;">
  <h1>🐱 Cat API テストページ</h1>
  <div id="cat-container">
    <p>Loading a cute cat...</p>
  </div>

  <script>
    fetch('/_hcms/api/fetch-cat')
      .then(response => response.json())
      .then(data => {
        const container = document.getElementById('cat-container');
        const img = document.createElement('img');
        img.src = data[0].url;
        img.alt = 'Cute cat';
        img.style.maxWidth = '100%';
        img.style.borderRadius = '8px';
        container.innerHTML = '';
        container.appendChild(img);
      })
      .catch(err => {
        document.getElementById('cat-container').innerText = 'Failed to load cat 😿';
        console.error(err);
      });
  </script>
</div>
{% endblock %}
```

#### 🔍 解説

| 行 | 意味 |
|----|------|
| fetch('/_hcms/api/fetch-cat') | 作成したサーバーレス関数を呼び出す |
| data[0].url | Cat APIのJSONから画像URLを取り出す |
| document.createElement('img') | 新しく `<img>` タグを作る |
| container.innerHTML = '' | 「Loading...」のテキストを消して表示を切り替える |
| container.appendChild(img) | div の中に画像を追加する |
| .catch() | 失敗した場合のフォールバック処理（テキスト表示＋consoleにエラー） |

※ 新しいテンプレートファイルを作成した場合は、<dody> 内にある {% dnd_area "dnd_area" ... を削除した上で（ドラッグアンドドロップエリアは不要なので）、<div ...> </div> の間のコードを使ってね
![](https://blog.ryamagami.com/hs-fs/hubfs/serverless-func14.png?width=1300&height=1004&name=serverless-func14.png)

テンプレートが書けたら変更を公開して、プレビュー！

👇のように かわいい猫画像がロードされたら大成功です！🎉👏

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/fetch-cat-2.gif?width=1920&height=1064&name=fetch-cat-2.gif)

---
## 🐱 おわりに
今回は、前編で作ったサーバーレス関数をベースに、
外部の公開API（The Cat API）をfetchしてデータを取得するという体験をしてみました🐾

ただデータを取得するだけでなく、取得した画像をページに表示するところまでやってみると、
「HubSpot CMSでも、動的な表示ができるんだ！」という実感が持てたのではないでしょうか？🖼️✨

💡 今回のポイントをふりかえると…

- fetchの使い方（await / response.json()）を体験できた

- サーバーレス関数の返り値を、JavaScriptでページに反映する方法を知れた

- エラーハンドリングや初期表示（Loading）など、実用のヒントもちょこっと登場◎

次回の後編では、いよいよHubSpot APIと連携して、
より実践的な使い方にステップアップしようと思います～

すこしずつ、「できる」が増えていく感覚を大事にしながら、
ぜひ引き続きチャレンジしてみてくださいね🧪🐱🌸

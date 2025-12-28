---
title: "【HubSpot】はじめてのHubSpotサーバーレス関数（後編）：HubSpot APIで企業一覧を表示してみよう 🧪"
emoji: "🧪"
type: "tech"
topics: [HubSpot, Serverless, API]
published: true
published_at: "2025-03-30"
---

# 【HubSpot】はじめてのHubSpotサーバーレス関数（後編）：HubSpot APIで企業一覧を表示してみよう 🧪

2025-03-30 · Reina

### 📝 はじめに

前編では「Hello World」を表示するところから、
中編では公開API（Cat API）を使って猫の画像を表示するところまで、
少しずつHubSpotのサーバーレス関数に慣れてきました🐾

そしていよいよ、今回の後編では
「HubSpot API」を使って、CRMのデータをウェブサイトに動的に表示してみます！

具体的には、特定の会社レコードのリストに含まれる「会社名」一覧を、
サーバーレス関数を通じて取得し、ページ上に表示するという流れです。

手動で「取引企業一覧」、「取引実績」や「導入企業一覧」をページに追加・更新するのって、意外と面倒だったりしませんか…？
取引先が増えたり減ったり、うっかり入れ忘れていたり…。

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8830%E6%97%A5%2018_47_02.png?width=700&height=466&name=ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8830%E6%97%A5%2018_47_02.png)

HubSpotのCRMを使っていれば、会社レコードの情報はすでに登録されているはず。

🐾 今回はこうします！

HubSpotの会社レコードに「この企業は表示したい！」という情報をプロパティとして持たせておき、
そのプロパティが true の会社だけを HubSpot API の検索機能で取得して表示する、という方法を取ります。

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8830%E6%97%A5%2023_01_22.png?width=700&height=466&name=ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8830%E6%97%A5%2023_01_22.png)



✅ 今回使うのは 
```
POST /crm/v3/objects/companies/search
```
エンドポイント！

カスタムプロパティを使えば
より柔軟に、実運用に近い形でデータを扱うことができます✨

＊この記事では、カスタムプロパティ「ウェブサイトで表示する (show_on_website)」の値が「はい (true)」の場合、ページに表示させる という例で解説します🌏

![](https://blog.ryamagami.com/hs-fs/hubfs/serverless-func22.png?width=1700&height=1144&name=serverless-func22.png)

また、HubSpot APIをサーバーレス関数で使うには、認証まわりの設定が必要です。
この記事ではその手順も一緒に説明していきます🛡️

それではさっそく、準備からはじめてみましょう🧪

### 🧩 （事前準備）会社レコードにカスタムプロパティを追加しよう！


今回の関数では、HubSpotの会社レコードに
```
show_on_website = true
```
 という条件で検索をかけています。
そのためには、あらかじめ以下のようなカスタムプロパティを用意しておく必要があります👇

#### ⚙️ 作成手順
- 画面右上の「⚙️ 設定」へ
- 左メニューから「データ管理」→「プロパティ」を開く
- ドロップダウンから「会社プロパティ」選択
- 「プロパティを作成」ボタンをクリック
- 以下のように設定👇

| 項目 | 値 |
|---|---|
| ラベル | Webサイトに表示する（など任意） |
| 内部値 | show_on_website |
| フィールドタイプ | 1 つのチェックボックス |

✅ プロパティを設定したら…
会社レコードの中で「この会社をサイトに載せたい！」というレコードだけ
「はい✅」の値にしておくことで、関数からその情報を絞り込めるようになります！

### 📂 今回のファイル構成

今回も、 `.functions` フォルダの中に関数を用意していきます。
作成方法の詳細は[前編の記事](https://blog.ryamagami.com/hubspot-serverless-func-01)をご覧ください 📝

```
📁 get-company-list.functions/
├── 📄 get-company-list.js ← 今回作る関数ファイル
└── 📄 serverless.json ← シークレットの指定などを記述する設定ファイル
```

このあとで紹介する `get-company-list.js` の中で、HubSpot APIにアクセスして
会社名一覧を取得する関数を作成していきます！

### 🔐 HubSpot APIを使う前に、認証の準備をしよう！

今回の後編では、HubSpot APIを使ってCRMの「会社レコード」にアクセスします。
そのためにはHubSpotの[非公開アプリ（Private App）](https://developers.hubspot.jp/docs/guides/apps/private-apps/overview)を作成して、アクセストークンを取得しておく必要があります。

ただし、トークンをそのままコードに書いてしまうのはNG✋🏻
代わりに、HubSpot CLIのsecrets機能を使って、安全に関数内で使えるようにしていきます🛡️

#### ✅ ステップ1：非公開アプリを作ろう
- HubSpotのポータル画面で「⚙️設定」→「連携」→「非公開アプリ」へ
- 「非公開アプリを作成」ボタンをクリック
- アプリ名（例：`serverless-api-demo`）を入力
- スコープに `crm.objects.companies.read` を追加
- アプリを作成したら、「アクセストークン」をコピーしておく

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/serverless-func20.png?width=1300&height=538&name=serverless-func20.png)

#### ✅ ステップ2：CLIでシークレットを登録する

次に、コピーしたトークンをHubSpot CLIを使ってシークレットとして登録します。

① hs secrets add コマンドを実行
```bash
hs secrets add
```
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/serverless-func17.png?width=1500&height=68&name=serverless-func17.png)

② シークレット名を指定（例 : `SERVERLESS_API_DEMO`）
③ あとはプロンプトに従って、先ほどコピーしたアクセストークンを貼り付けるだけ！
［SUCCESS］が表示されたら完了です✨

![](https://blog.ryamagami.com/hs-fs/hubfs/serverless-func18.png?width=1500&height=82&name=serverless-func18.png)

シークレットについて詳しい説明はこちら → [サーバレスリファレンス / シークレット](https://developers.hubspot.jp/docs/reference/cms/serverless-functions#%E3%82%B7%E3%83%BC%E3%82%AF%E3%83%AC%E3%83%83%E3%83%88)

##### 💡補足： シークレット名にはルールがあります

シークレット名にはアルファベットと数字、アンダースコア(_)のみが使えます。
たとえば `serverless-api-demo` のようにハイフン(-)が入っている名前は使えないので注意！

OKな例：
```
HUBSPOT_ACCESS_TOKEN
crm_token
SERVERLESS_API_DEMO
```
NGな例：
```
serverless-api-demo
```

#### ✅ ステップ3：`serverless.json` にシークレットを追記する

関数がこのシークレットを使えるように、 `.functions` フォルダ内の `serverless.json` に以下のように記載します👇

```json
{
  "runtime": "nodejs18.x",
  "version": "1.0",
  "secrets": ["SERVERLESS_API_DEMO"],　// 👈ここで先ほど追加したシークレットを指定する 
  "endpoints": {
    "get-company-list": {
      "method": "GET",
      "file": "get-company-list.js"
    }
  }
}
```

関数では `process.env.SERVERLESS_API_DEMO` の形で使用します。
これで、安全にトークンを使ってAPIを呼び出せる準備が完了しました✨
次はいよいよ、関数を作って実際に会社レコードにアクセスしていきます🐾

### 🧪 サーバーレス関数で、会社名一覧を取得してみよう！

このセクションでは、いよいよ HubSpot API を使って
特定の条件に一致する会社レコードの「会社名」を取得して表示する関数を作っていきます！

#### ✨ 今回の目標（まとめ）
- `get-company-list.js` という関数を作る
- 関数内で `POST /crm/v3/objects/companies/search` を呼び出す
- `show_on_website = true` の会社レコードを抽出し、会社名一覧を配列で返す
- ページ上で fetch → `<ul>` リストで表示！

#### 📂 フォルダ・ファイル構成（再掲）

```
📁 get-company-list.functions/
├── 📄 get-company-list.js
└── 📄 serverless.json
```

#### 🐾 ステップ1：API側で条件に合致する会社レコードを取得しよう

`get-company-list.js` で、条件（カスタムプロパティ : `show_on_website` の値が `true` ）に合致する会社レコードを検索してレコード名を取得します。

📄 get-company-list.js のサンプル

```js
const hubspotToken = process.env.SERVERLESS_API_DEMO;

exports.main = async (context, sendResponse) => {
  try {
    const searchResponse = await fetch('https://api.hubapi.com/crm/v3/objects/companies/search', {
      method: 'POST',
      headers: {
        Authorization: `Bearer ${hubspotToken}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        filterGroups: [
          {
            filters: [
              {
                propertyName: 'show_on_website',
                operator: 'EQ',
                value: true
              }
            ]
          }
        ],
        properties: ['name'],
        limit: 20
      })
    });

    const data = await searchResponse.json();

    const companyNames = data.results
      .map(company => company.properties.name)
      .filter(Boolean);

    sendResponse({
      statusCode: 200,
      body: JSON.stringify(companyNames)
    });

  } catch (error) {
    console.error('エラー:', error);
    sendResponse({
      statusCode: 500,
      body: JSON.stringify({ error: '会社名の取得に失敗しました' })
    });
  }
};
```

💡 `get-company-list.js` でやっていること：

| 行数 | 説明 |
|---|---|
| `fetch(...)` | HubSpotの会社検索APIを呼び出し |
| `filterGroups` / `show_on_website = true` | 条件（※ `show_on_website` はカスタムプロパティの内部値） |
| `properties` | 取得したい情報（ここでは会社名のみ） |
| `.map(...).filter(Boolean)` | 配列の中から会社名だけを抽出＆nullを除去 |

ここまで用意できたら、一度 `hs upload` を使ってHubSpotのデザインマネージャー側にアップロードしておきましょう⛅

##### 🔐 補足：シークレットの呼び出し方

```js
const hubspotToken = process.env.HUBSPOT_ACCESS_TOKEN;
// headers: {
//   Authorization: `Bearer ${hubspotToken}`,
//   'Content-Type': 'application/json'
// }
```

上のコードで、CLIに登録したシークレットを取得しています。
そしてその値を、APIのリクエストヘッダーに設定しています👇

```http
Authorization: Bearer <TOKEN>
Content-Type: application/json
```

こうすることで、トークンをコードに直接書かずに、安全に認証付きのAPIリクエストができるようになります💡

#### 🐾 ステップ2：クライアント側から関数を呼び出して、会社名を表示しよう！

サーバーレス関数が完成したら、
次はページ上からこの関数を呼び出して、取得した会社名を表示してみましょう！

💡 ここでやること

| ステップ | 内容 |
|---|---|
| 1 | ページテンプレート or カスタムモジュール内に `<div id="company-list">` を用意 |
| 2 | JavaScript を使ってサーバーレス関数を `fetch()` で呼び出す |
| 3 | 返ってきた配列を `<ul>` のリストとしてHTMLに追加 |

📜 ページで使う HTML/JavaScript の例

```html
<div id="company-list">Loading...</div>
<script>
fetch('/_hcms/api/get-company-list')
  .then(response => response.json())
  .then(names => {
    const list = document.createElement('ul');
    names.forEach(name => {
      const li = document.createElement('li');
      li.textContent = name;
      list.appendChild(li);
    });
    document.getElementById('company-list').textContent = '';
    document.getElementById('company-list').appendChild(list);
  })
  .catch(err => {
    console.error(err);
    document.getElementById('company-list').textContent = '取得に失敗しました';
  });
</script>
```

💡 処理の流れ：

| 行 | 意味 |
|---|---|
| `fetch(...)` | 作成したサーバーレス関数を呼び出す |
| `.then(response => response.json())` | レスポンスをJSONとして読み込む |
| `document.createElement('ul')` | リストの要素を作成する |
| `li.textContent = name` | 各会社名をリスト項目としてセット |
| `.appendChild(...)` / `<div id="company-list">` | `#company-list` の中にリストを追加 |
| `.catch(...)` | エラー時にフォールバックテキストを表示＆ログ出力 |

👇のように Loading ... の後に カスタムプロパティ `show_on_website` を `true` にしている会社レコードの名前が表示されたら成功🎉💕

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/get-company-list.gif?width=2772&height=1425&name=get-company-list.gif)

#### 🖌️ 補足：おすすめなテスト方法
- HubSpotのページテンプレートを1つ作って、上のコードを貼ってプレビューすれば動作確認できるよ！
- コンソールにエラーが出ているときは、DevTools（開発者ツール）の `Console` タブをチェック👀✨

### ✨ まとめ：HubSpot APIとサーバーレス関数で、実務に近い動的表示を実現！

これまで3回にわたって、HubSpotのサーバーレス関数にチャレンジしてきました。
- 🧪 前編では「Hello World」で関数のしくみを体験し
- 🐱 中編では Cat API を使って、外部APIの連携とページ表示を学び
- 🏢 そして今回の後編では、HubSpot CRMの会社データを動的に取得し、ページに表示するというところまで進んできました！

#### 💡 今回できるようになったこと

✅ HubSpotの非公開アプリを使って、API認証の準備ができた
✅ CLIのシークレット機能で、安全にトークンを使う方法を理解した
✅ `POST /crm/v3/objects/companies/search` で、カスタムプロパティを条件にデータを取得
✅ JSON形式で会社名を返す関数を作成
✅ JavaScriptで関数を呼び出し、ページに企業一覧を表示！

#### 🐾 小さな工夫で、大きなメンテナンス性！

「取引企業一覧」や「導入実績」など、つい手動で更新してしまいがちな情報も、
サーバーレス関数＋カスタムプロパティの仕組みを使えば、
CRM上の更新だけでページが連動してくれるようになります✨

HubSpot CMS × サーバーレス関数は、
「ちょっとした動的コンテンツ」を実現するのにぴったりなツールです💡

### ☕ おつかれさまです！

ここまで読み進めてくれた方、ほんとうにおつかれさまでした！
最後は、実装後のコーヒーブレイクを楽しむ女の子（with 猫ちゃん🐈）のイラストでしめたいと思います☕

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8830%E6%97%A5%2023_42_52.png?width=700&height=700&name=ChatGPT%20Image%202025%E5%B9%B43%E6%9C%8830%E6%97%A5%2023_42_52.png)

このシリーズがちょっとでもサーバレス関数やHubSpot CMS開発の興味を持つきっかけになったら嬉しいです🍀

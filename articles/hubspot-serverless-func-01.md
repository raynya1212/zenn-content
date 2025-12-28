---
title: "【HubSpot】はじめてのHubSpotサーバーレス関数（前編）：まずは動かしてみよう"
emoji: "🧪"
type: "tech"
topics: [HubSpot, Serverless]
published: true
published_at: "2025-03-24"
---

# 【HubSpot】はじめてのHubSpotサーバーレス関数（前編）：まずは動かしてみよう 🧪

2025-03-24 · Reina

### ✅ はじめに
HubSpotでブログやページを作っていると、「ちょっと動的な処理を入れたいな…」「外部サービスと連携したいな…」と思うこと、ありませんか？
そんなときに便利なのが **HubSpotのサーバーレス関数**。
JavaScript / Node.js で書けて、サーバー側での処理を簡単に追加できます。

**できること👇**
- 🔄 外部システムからデータを取得して動的に表示
- 📝 フォーム送信を受け取ってHubDBやCRMに保存
- 🧮 複雑な処理をサーバー側で実行
- 📤 別システムへデータ連携

公式ドキュメント：[Serverless functions overview](https://developers.hubspot.jp/docs/guides/cms/content/data-driven-content/serverless-functions/overview)

この記事では **Hello World を返す最初の関数を動かすところまで** を解説します。

「設定とか難しそう…」と思った方も、だいじょうぶ。
コピペだけでもOK！ 実際に手を動かしながら、サクッと動かす体験を一緒にしてみましょう🙌

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/DALL%C2%B7E%202025-03-24%2020.04.40%20-%20A%20soft%20and%20cute%20illustration%20of%20a%20young%20woman%20(developer%20or%20blogger%20style)%20sitting%20at%20a%20desk%20with%20a%20laptop%2c%20raising%20her%20fist%20slightly%20in%20a%20small%20let.webp?width=700&height=700&name=DALL%C2%B7E%202025-03-24%2020.04.40%20-%20A%20soft%20and%20cute%20illustration%20of%20a%20young%20woman%20(developer%20or%20blogger%20style)%20sitting%20at%20a%20desk%20with%20a%20laptop%2c%20raising%20her%20fist%20slightly%20in%20a%20small%20let.webp)

---
## ⚙️ 準備しよう
まずは、HubSpotのサーバーレス関数を動かすために必要な準備をしていきます。


### 必要なもの
以下の3つがあればOKです！

- HubSpot アカウント
Content Hub (CMS Hub) Enterprise もしくは CMS開発者用サンドボックスアカウント
- CMS開発者サンドボックスアカウントは一部制限はあるものの無料でつくれるよ～

https://zenn.dev/raynya/articles/f1751300bbf612#5.%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%EF%BC%88cms%E9%96%8B%E7%99%BA%E8%80%85%E3%82%B5%E3%83%B3%E3%83%89%E3%83%9C%E3%83%83%E3%82%AF%E3%82%B9%E3%82%A2%E3%82%AB%E3%82%A6%E3%83%B3%E3%83%88%EF%BC%89

- HubSpot CLI
ローカル開発をするためのコマンドラインツール
- エディタ
エディタ（VSCodeなど）
- 作業ディレクトリ
作業をすすめるためのフォルダを用意

📍 VSCodeでHubspot CLIの拡張機能をいれると、２と３がまとめて用意できるのでおすすめ！また今回は特定の作業ディレクトリ内で進めるので、特定のディレクトリに対してインストールしておくのが良いです

https://blog.ryamagami.com/hubspot-cli-vscode

### 🔐 HubSpot CLIと接続しよう

HubSpot CLIが使えるようになったら、ターミナルで hs init (初回認証) もしくは hs auth (追加のアカウントを認証) でアカウントを接続します。

詳しくは👇を参照です：
https://developers.hubspot.jp/docs/guides/cms/setup/getting-started-with-local-development#2.-%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E9%96%8B%E7%99%BA%E3%83%84%E3%83%BC%E3%83%AB%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B

https://blog.ryamagami.com/hubspot-cli-vscode#section-4


📍この記事では、VSCodeの拡張機能を使用する前提にて解説します

---
## 🧪 サーバーレス関数を体験しよう
今回は hello-world という名前の関数を作って、/_hcms/api/hello-world にアクセスしたときに
「Hello from HubSpot Serverless Functions!」というメッセージが返る処理を作ってみます！

### 📁 フォルダ構成を作る

まず、ローカルに hello-world というフォルダを作成します。
その中に、HubSpotのサーバーレス関数用フォルダとして hello-world.functions を作成しましょう～

この .functions フォルダの中に、次の2つのファイルを作成します：

hello-world.js：関数の本体

serverless.json：この関数のルートや設定を記述するファイル

※VSCode の場合は👇のアイコンからフォルダやファイルが作れるよ

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/serverless-func06.png?width=932&height=564&name=serverless-func06.png)

📁 フォルダ構成は以下のようになります：
```
hello-world/
└── hello-world.functions/
    ├── hello-world.js
    └── serverless.json
```

---
## 📝hello-world.js に関数を書いてみよう
ひとまずはコピペでOK！👇のコードを先ほど作った hello-world.js に書きます。
```js
// hello-world.js
exports.main = async (context = {}, sendResponse) => {
  sendResponse({
    statusCode: 200,
    body: JSON.stringify({ message: "Hello from HubSpot Serverless Functions!" }),
  });
};
```

### 解説
- `exports.main`：関数のエントリーポイント
- `sendResponse()`：ブラウザに返す内容を定義
- `statusCode: 200`：HTTPステータスコード「成功」
- `body`：返却するJSONデータ

---
## 🧾 serverless.json を書こう
同じフォルダ内の serverless.json には、この関数をどう呼び出すか を定義します👇
```json
{
  "runtime": "nodejs18.x",
  "version": "1.0",
  "endpoints": {
    "hello-world": {
      "method": "GET",
      "file": "hello-world.js"
    }
  }
}
```

### 設定の意味
- `runtime`：Node.jsバージョン
- `endpoints`：この関数で使うエンドポイントの一覧
- `hello-world`：実際のパス（/_hcms/api/hello-world）になる
※ サーバーレス関数は、HubSpot CMSアカウントのドメインのパスを通して公開されます。

それぞれのファイルがちゃんと保存されていることを確認！💾

＊hello-world.js ファイル
![](https://blog.ryamagami.com/hs-fs/hubfs/serverless-func07.png?width=1460&height=424&name=serverless-func07.png)
＊serverless.json ファイル
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/serverless-func08.png?width=1020&height=762&name=serverless-func08.png)


---
## 📤 アップロードしよう！
 hs upload コマンドを使って、Hubspot のデザインマネージャーに作成したファイルをアップロードします📤

今回は "hello world" フォルダを デザインマネージャー側にも作成したいので、
```bash
hs upload hello-world hello-world
```
の形で実行します。

［SUCCESS］が表示されたら、Hubspot のデザインマネージャーの画面をリロードして "hello-world" のフォルダができているかを確認します。

![](https://blog.ryamagami.com/hs-fs/hubfs/serverless-func09.png?width=3028&height=1308&name=serverless-func09.png)


---
## 🌐 動作確認！
アップロードが完了したら、ブラウザで以下のURLにアクセス👇
https://[あなたのドメイン (HubSpot CMS で使っているドメイン)]/_hcms/api/hello-world

👇のように
```
{"message":"Hello from HubSpot Serverless Functions!"}
```
と表示されれば、大成功〜！🎉💻✨

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/serverless-func10.png?width=1700&height=424&name=serverless-func10.png)

---
## 🐣 おわりに
今回は、HubSpotのサーバーレス関数を使って「Hello World」を返すシンプルな関数を作ってみました。
.functions フォルダの中に hello-world.js と serverless.json を用意するだけで、意外とあっさり動いた！と思った方もいるかもしれません🧪

HubSpot CMSはフロント寄りな印象がありますが、
サーバーレス関数を使えば「ちょっとしたバックエンド処理」も自分で組み込めるようになります。

🔄 外部APIを呼び出したり、
📝 フォーム送信データを加工したり、
🗃️ HubDBやCRMと連携することも！

その第一歩として、今回の記事が参考になればうれしいです🌷

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/DALL%C2%B7E%202025-03-24%2021.44.55%20-%20A%20soft%20and%20cute%2c%20chibi-style%20illustration%20of%20a%20young%20woman%20sitting%20with%20a%20closed%20laptop%20beside%20her%2c%20holding%20a%20warm%20drink%20like%20coffee%20or%20tea%2c%20and%20smili.webp?width=700&height=700&name=DALL%C2%B7E%202025-03-24%2021.44.55%20-%20A%20soft%20and%20cute%2c%20chibi-style%20illustration%20of%20a%20young%20woman%20sitting%20with%20a%20closed%20laptop%20beside%20her%2c%20holding%20a%20warm%20drink%20like%20coffee%20or%20tea%2c%20and%20smili.webp)


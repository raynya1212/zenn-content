---
title: "【HubSpot】意外と簡単 ! VSCode で HubSpot CLI を使ってみよう"
emoji: "🚀"
type: "tech"
topics: [HubSpot,HubSpot CMS,HubSpot CLI,vscode]
published: true
published_at: 2025-03-23
---

# 【HubSpot】意外と簡単 ! VSCode で HubSpot CLI を使ってみよう 

2025-03-23 · Reina

HubSpot のページデザインでは、「デザインマネージャー」を使って編集していくことが多いかなと思います。けど、「デザインマネージャー」は手軽な一方 普段エディターを使う勢からしたら、

* 複数ファイルの編集がしづらい
* コード補完や整形が弱くてストレス

といったちょいモヤ😖なことがあるかもしれません。

そこでおすすめなのが、**VSCode × HubSpot CLI** のローカル開発環境です！  
ローカルで編集したファイルをそのままアップロードしたり、リアルタイムにテーマを更新したり…とにかく**効率アップ＆快適！**✨

この記事では、VSCodeを使って  
👉 HubSpotのテーマをローカルで編集し  
👉 CLIコマンドでアップロードして反映させる  
**初心者さん向けの基本ステップ**を、デモつきで紹介していきます～

![DALL·E 2025-03-23 19.46.46 - A cute and bright illustration of a girl coding on a laptop. She is sitting at a desk with a clear, colorful code editor on the screen (like VSCode). ](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/DALL%C2%B7E%202025-03-23%2019.46.46%20-%20A%20cute%20and%20bright%20illustration%20of%20a%20girl%20coding%20on%20a%20laptop.%20She%20is%20sitting%20at%20a%20desk%20with%20a%20clear%2c%20colorful%20code%20editor%20on%20the%20screen%20(like%20VSCode).%20.webp)

## 💡 HubSpot CLI とは？

**[HubSpot CLI](https://developers.hubspot.jp/docs/guides/cms/setup/getting-started-with-local-development)（Command Line Interface）** は、HubSpotのCMS開発をローカル環境で行えるようにするためのツールです。  
テーマやテンプレート、カスタムモジュールなどをコマンドで操作できるようになり、**デザインマネージャーの画面だけに頼らず開発ができる**ようになります。

たとえばこんなことができるよ👇

* HubSpotにあるテーマファイルをローカルにダウンロード（`hs fetch`）
* ローカルで編集した内容をアップロード（`hs upload`）
* ファイルをリアルタイムで同期（`hs watch`）
* サーバーレス関数の管理やログの確認

💡普段 WordPress なんかでサイト制作されている方だと、FTP に近いもの（イコールではないけど）と思うと、少しイメージわくかもです

## ⌨️ VSCodeでHubSpot CLIを操作するメリット

HubSpot CLIはターミナルから操作するツールだけど、**[VSCode で使える公式の拡張機能](https://developers.hubspot.com/docs/guides/cms/setup/install-and-use-hubspot-code-extension)がほんと便利** なのでこれを使っていくことを全力でおすすめします！🔥

## 💻 HubSpot CLI ✖ VSCode を体験しよう

### ✅ 事前準備

操作する端末に、操作に必要なものをインストールします：

* HubSpot のアカウント（CMS Hub / Content Hub が使えるもの）
* [Node.js](https://nodejs.org/en/download)
* [VSCode](https://code.visualstudio.com/download)

VSCode までインストールが完了したら、VSCode の拡張機能に［HubSpot］をインストールします。  
※ サイドバーに HubSpot のスプロケットアイコンが表示されたらOK👌

![vscode_01](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_01.png)

### 🔧 HubSpot CLIをインストール＆認証する

#### ① HubSpot CLI をインストール

まずは HubSpot CLI をインストールします。  
コマンドプロンプト or VSCode でターミナルを開き インストールコマンドを実行します。

* グローバルにインストールする場合 ：`npm install -g @hubspot/cli`
* 特定のディレクトリ（フォルダ）にインストールする場合：(インストールするディレクトリに移動した上で、) `npm install @hubspot/cli`

📍このあたりはお好みで大丈夫ですが、個人的にはフォルダを一個作って その中にインストールする方が後々迷子にならないかなと思います

※ VSCode でターミナルを開く方法👇

![vscode_02](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_02.png)

📍グローバルインストール時にEACCESエラーが発生した場合：👇のドキュメントを参考にすると良い（って[公式ドキュメント](https://developers.hubspot.jp/docs/guides/cms/setup/getting-started-with-local-development#1.-%E4%BD%9C%E6%A5%AD%E3%83%87%E3%82%A3%E3%83%AC%E3%82%AF%E3%83%88%E3%83%AA%E3%83%BC%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B:~:text=%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E6%99%82%E3%81%ABEACCES,%E5%8F%82%E7%85%A7%E3%81%8F%E3%81%A0%E3%81%95%E3%81%84%E3%80%82)が言ってました）


https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally


#### ② アカウント認証を行う

CLI で使う HubSpot のアカウントを認証するには、ターミナルで `hs init` を実行することも可能ですが、VSCode 拡張機能経由で行うこともできます。

拡張機能の画面を開いて ［Authenticate HubSpot Account］をクリック

![vscode_03](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_03.png)

ブラウザを開く旨のポップアップが表示されるので、OKとして HubSpot のパーソルアクセスキー（認証に用いられる情報🔑）が保管されているページに移動します。  
ここでは、［Visual Studio Code を開く］のポップアップが表示されるので ［Visual Studio Code を開く］をクリックして、VSCode に戻ります。

![vscode_04](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/vscode_04.png)

VSCode 上に認証したアカウント名が表示されていれば、完了です💯

![vscode_06](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_06.png)

### 📁 ファイルをアップロードしてみよう

ローカル（自分のパソコンのフォルダの中にあるもの という意味）で用意したファイルを HubSpot CLI 経由でデザインマネージャーにアップロードしてみましょう🔼

今回は試しに👇の画像をデザインマネージャー上にアップロードしてみます

![DALL_E-2025-03-23-18.38.11-A-cute-illustrated-cat-image_-perfect-for-a-beginner-friendly-tech-blog-removebg-preview](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/DALL_E-2025-03-23-18.38.11-A-cute-illustrated-cat-image_-perfect-for-a-beginner-friendly-tech-blog-removebg-preview.png)

1) VSCode でターミナルを立ち上げる

VSCode でターミナルを立ち上げて、作業ディレクトリに移動します。

＊私は /HubSpot\_local  というフォルダ内で HubSpot CLI をインストールしているので、ここに移動＆画像を用意しています

![vscode_08](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/vscode_08.png)

2) ファイルをアップロードする

`hs upload` コマンドを使って、画像をデザインマネージャー側にアップロードします。

hs upload コマンドは、`hs upload <src> <dest>` という形で ファイルのアップロード元 と アップロード先 のパス（位置）を指定して実行します。

＊ src = source , dest = destination という意味

今回は作業ディレクトリ内にある画像ファイルを テーマフォルダ > images フォルダの中に"CLI\_upload\_test.png" としてアップロードするので、👇のような形で実行します。

📍 この時、アップロード後のファイル名も指定してあげないとエラーになってしまうようです。

![vscode_09](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_09.png)

［SUCESS］のレスポンスが返ってきたら、成功です!

3) アップロードされたファイルを デザインマネージャー上で確認する

デザインマネージャーの画面をリロードして、アップロードしたファイルがあるか確認してみます🔍

（発見！無事アップロードされてましたね👏）

![vscode_10](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/vscode_10.png)

### 🎨 既存のテーマを編集してみよう

次は、デザインマネージャー側に既にあるテーマをいじってみましょう。

1) デザインマネージャーのファイルをローカルにコピーする

HubSpot CLI で閲覧しているファイルを編集するには、一度ローカル側にファイルをコピーしてあげる必要があります。

＊例えば、theme.json のファイルを開いてみると ［READONLY］と書かれていますね👀

![vscode_11](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_11.png)

デザインマネージャー上にあるファイルをローカルにコピーするには、`hs fetch` コマンドを使います。

今回はテーマフォルダの中のファイルを編集したいので、`hs fetch <テーマフォルダ名>` の形で実行します。

![vscode_12](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_12.png)

ローカルにある 作業ディレクトリをみて、テーマフォルダがコピーされていたら完了です。

![vscode_13-1](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_13-1.png)

2) 変更の同期を有効する

先ほどと同じようにローカル側で編集 → `hs upload` でアップロード ということもできますが、今回は ローカル側の変更がすぐに同期される `hs watch` コマンドを使ってみます。

＊書き方は `hs upload`と似てて、私は作業ディレクトリ内にある "MeowBitdev" フォルダ を、デザインマネージャー上にある "MeowBitdev" フォルダ に同期したいので、こんな感じ👇

![vscode_14](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_14.png)

3) ファイルを編集して、同期されたかチェックする

`hs fetch`でコピーされたフォルダ（ローカル側のフォルダ）を開いて、試しに theme.json の author とサムネイル画像でも変えてみます（ボイラープレートを使ってるので、デフォルトののままでした）

![vscode_15](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/vscode_15.png)

変更してファイルを保存したら、ファイルをアップロードした旨のメッセージがターミナルに表示されます。（これが出てたら、`hs watch`が効いてる証拠✨）

![vscode_16](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/vscode_16.png)

HubSpot のテーマ一覧画面をみると、テーマのサムネイル画像が変更されていることが確認できます🐈

![image-png](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/image-png.png)

### 📌 よく使うCLIコマンドまとめ

| コマンド | 説明 |
| --- | --- |
| `hs fetch` | デザインマネージャーのファイル (テーマなど) をローカルにコピー |
| `hs upload` | ローカルのファイルをHubSpotにアップロード |
| `hs watch` | ファイルの変更をリアルタイムでアップロード |
| `hs auth` | 追加のHubSpotアカウントを認証する |
| `hs accounts list` | 接続しているアカウントの一覧確認 |

## 🚀 まとめ

今回は、VSCodeとHubSpot CLIを使って、テーマファイルをローカルで編集 → アップロードする流れを紹介してみました🐾

「CLIってちょっとむずかしそう…」って思ってた人も、  
この記事の流れで「あれ、意外といけるかも？」って思えたら嬉しいです🍀

HubSpot の VSCode 拡張機能は、殺風景なターミナル画面よりも直感的に HubSpot CLI を操作できるようになるので、ぜひ取り入れてみてください～

HubSpot CLI にトライしてみたら 気になるところをちょっとずつ触ってみたり、  
カスタムモジュールを作ってみたり、サーバーレス関数にチャレンジしてみたり…  
自分のペースで CMS 開発楽しんでみてくださいね⚙️💟

![DALL·E 2025-03-23 19.54.36 - A soft, pastel-colored illustration of a girl and a cute cat giving each other a high five. The girl is smiling gently and sitting next to a laptop, l](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/DALL%C2%B7E%202025-03-23%2019.54.36%20-%20A%20soft%2c%20pastel-colored%20illustration%20of%20a%20girl%20and%20a%20cute%20cat%20giving%20each%20other%20a%20high%20five.%20The%20girl%20is%20smiling%20gently%20and%20sitting%20next%20to%20a%20laptop%2c%20l.webp)

---

⭐ おすすめリソース

公式ドキュメントはお友達🙌

* [ローカル開発を開始する](https://developers.hubspot.jp/docs/guides/cms/setup/getting-started-with-local-development)
* [Install and use the HubSpot Visual Studio Code extension](https://developers.hubspot.com/docs/guides/cms/setup/install-and-use-hubspot-code-extension)
* [HubSpot CLI commands (v7)](https://developers.hubspot.com/docs/guides/cms/tools/hubspot-cli/cli-v7)

また、👇のサイトでは HubSpot CLI でテーマ作成を行うチュートリアルもあったので、こちらもおすすめです

https://blog.css-net.co.jp/entry/hubspot-cms-quick-start
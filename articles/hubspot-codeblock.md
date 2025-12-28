---
title: "HubSpot ブログにコードブロックをいれたい!! 💻"
emoji: "🧩"
type: "tech"
topics: [HubSpot, HubSpot CMS]
published: true
published_at: "2025-04-20"
---


# HubSpot ブログにコードブロックをいれたい!! 💻

2025-03-06 · Reina

💻備忘録的にまとめているので、割と雑多です

## HubSpot コードブロックがない問題

HubSpot ブログ純正の機能だと、コードブロック というものがないんです（私が知る限りでは）><

一応 [コード書式](https://knowledge.hubspot.com/ja/website-pages/edit-content-in-a-rich-text-module#:~:text=%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%82%92%E3%82%B3%E3%83%BC%E3%83%89%E3%81%A8%E3%81%97%E3%81%A6%E6%9B%B8%E5%BC%8F%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B%E3%81%AB%E3%81%AF%E3%80%81%EF%BC%BB%E3%81%9D%E3%81%AE%E4%BB%96%EF%BC%BD%E3%83%89%E3%83%AD%E3%83%83%E3%83%97%E3%83%80%E3%82%A6%E3%83%B3%E3%83%A1%E3%83%8B%E3%83%A5%E3%83%BC%E3%82%92%E3%82%AF%E3%83%AA%E3%83%83%E3%82%AF%E3%81%97%E3%80%81code%E3%82%B3%E3%83%BC%E3%83%89%E6%9B%B8%E5%BC%8F%E8%A8%AD%E5%AE%9A%E3%82%A2%E3%82%A4%E3%82%B3%E3%83%B3%E3%82%92%E9%81%B8%E6%8A%9E%E3%81%97%E3%81%BE%E3%81%99%E3%80%82%C2%A0) というのは選べるんですけど、なんかこれイマイチなのですよね。。

やっぱり複数行になると👇こういう形 (下のは[Zennでの表示](https://zenn.dev/zenn/articles/markdown-guide#%E3%82%B3%E3%83%BC%E3%83%89%E3%83%96%E3%83%AD%E3%83%83%E3%82%AF)）でおきたくなりますよね💭

![code-block01](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/code-block01.png)

## 【結論】カスタムモジュール作るしかない

結論から言います。カスタムモジュール作るのがいいです。（[もしくは GitHub Gist のような外部サービスを使う](https://info.nextmode.co.jp/blog/hubspot-brog-gist)）

[この辺りのコミュニティ](https://community.hubspot.com/t5/Blog-Website-Page-Publishing/Source-Code-Block-in-a-blog-post/m-p/4919)もdigったのですが、あーーーんまりいい方法がなく、というか結局 prism.js 使う羽目になるので、そしたらカスタムモジュールにした方がいいよね というオチでした。

### モジュールのファイルあるじゃん

なんか都合良いチュートリアルでも落ちてないかな…とおもってたら、普通にHubSpotが出していました（感謝）

モジュールを作る手前までは、👇のチュートリアルに沿って進めればいけるはずです。

 📖 [How to Build a Code Block Web Component](https://developers.hubspot.com/blog/how-to-build-a-code-block-web-component)

で、モジュールの作成なんですが [こちらの Github](https://github.com/TheWebTech/hs-code-block-web-component) にご丁寧に一式揃っているので、それを流用すればいけるはずです。

### HubSpot CLI 使うしかない

モジュールの作成はもちろん UI からもできるんですが、今回は HubSpot CLI を使ってモジュールをアップロードする方がとてもとても幸せになれます。

📎2025/03/06 現在だと、*src/modules/code-block.module* のファイルが動作しました  
![code-block02](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/code-block02.png)

なぜかというと今回のモジュールでは テキストフィールドに対して改行を許可している（許可しないとコードが一行にまとめられてしまうため）のですが、これ、、デザインマネージャーではできなくて HubSpot CLI で fileds.json の `" allow_new_line": false``,` を true にしてあげないといけないんですね💭

※なんか思った形にならなくて、[このサイト](https://www.pensees.co.jp/hubspot-tips/resolve-hassle-of-repeater)見つけて気が付きました汗

HubSpot CLI の使い方については、いろいろとあるので参考になるページおいておきます。

* [Hubspot CLIについて](https://qiita.com/ibetaku/items/3858657fad671a0b7f09)
* [Install and use the HubSpot Visual Studio Code extension](https://developers.hubspot.com/docs/guides/cms/setup/install-and-use-hubspot-code-extension)　👈 VSCode で操作するのが圧倒的楽なのでおすすめ

## 一応形になりました

右往左往してしまいましたが、ひとまずは形👇になりました。いろいろと気になるところはあるので、気が向いたらいじるかも、いじらないかも。。。

技術ブログをHubSpotで運用したいよ～みたいな方はトライしてみてください 🚀

``` html
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>サンプルHTMLページ</title>
</head>
<body>
    <h1>ようこそ！</h1>
    <p>これはサンプルのHTMLページです。</p>
</body>
</html>
```
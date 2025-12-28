---
title: "【HubSpot】SNS共有ボタンをカスタマイズしたい🐦"
emoji: "🔗"
type: "tech"
topics: [HubSpot, Social Sharing]
published: true
published_at: 2025-03-09
---

# 【HubSpot】SNS共有ボタンをカスタマイズしたい🐦
2025-03-09 · Reina

### この記事の目的
HubSpotではページをSNSに共有するための ソーシャルシェアリング (Social Sharing) モジュール があります。

https://knowledge.hubspot.com/ja/design-manager/use-default-modules-in-the-layout-editor#social-sharing


けど、これちょっとアイコンが古臭い？このブログの雰囲気とは少し合わないんですよね💀➿

![sns-share 01](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/sns-share%2001.png?width=700&height=174&name=sns-share%2001.png)

なので今回はこの Social Sharing モジュールをカスタマイズして、自分好みの雰囲気に変えてみようと思います🔮💫

### 変更する内容
今回変更する内容は👇以下の２つ。

1. 対象のSNSにLINEを追加する
2. 共有ボタンのアイコンを変更する

### まずはじめに
デフォルトの`/@hubspot/social_sharing.module`は直接編集不可のため複製する必要があります。

🖌️複製は［モジュールを複製］ボタンから、複製したモジュールは自分のテーマ or 任意のフォルダ (ルートフォルダなど) に置きます。

![sns-share 02](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/sns-share%2002.png?width=1920&height=544&name=sns-share%2002.png)

### Social Sharing モジュールの構造
Social Sharing モジュールのフィールドの構成を見てくと、SNSごとに［Enabled?］と ［Link］２つのフィールドがあることがわかります🔍

![sns-share 03](https://blog.ryamagami.com/hs-fs/hubfs/sns-share%2003.png?width=530&height=736&name=sns-share%2003.png)

動作はプレビューしてくとわかるのですが、

Enabled ? ：該当のSNSアイコンを有効にするかどうか
Link：SNS共有ページに遷移するためのリンク設定
といった構造になっています。

※ 補足
モジュールのHTML構造をみると、［Link］で指定されている値 (custom_link_format) が <a> タグのリンク先に指定されていることが確認できます🔗

![sns-share 04](https://blog.ryamagami.com/hs-fs/hubfs/sns-share%2004.png?width=1828&height=956&name=sns-share%2004.png)

### LINE 共有を追加する
LINE 共有についてはLINE側のドキュメントに方法が掲載されていたので、そちらを使用します。また今回は好きな画像をアイコンにするので、「カスタムアイコンを使用」のセクションを参照します。

https://developers.line.biz/ja/docs/line-social-plugins/install-guide/using-line-share-buttons/#using-custom-icons


#### ＜設定方法＞
1. 既存SNSフィールドグループを複製

![sns-share 05](https://blog.ryamagami.com/hs-fs/hubfs/sns-share%2005.png?width=798&height=758&name=sns-share%2005.png)

2. 名前を「LINE」に変更
3. Linkフィールドを以下に変更：
```text
https://social-plugins.line.me/lineit/share?url={{ social_link_url|urlencode }}
```

![sns-share 06](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Social%20Share/sns-share%2006.png?width=782&height=1008&name=sns-share%2006.png)

4. アイコンを表示するため以下を追加：
```text
{{ render_social_icon('line') }}
```

![sns-share 07](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/sns-share%2007.png?width=1400&height=362&name=sns-share%2007.png)

### SNSアイコンを変更する
SNS アイコンについては、モジュールに添付されている xxx - color.png 画像を参照しているようです。

※ Twitter については、twitter bird と X の二種類が用意されているので、if 文が組まれているようです💭（たぶん）

![sns-share 08](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Social%20Share/sns-share%2008.png?width=1920&height=806&name=sns-share%2008.png)

なので、👇のナレッジベースでも解説されているようにアイコンに使いたい画像を同じ命名規則に則って追加してあげればＯＫです。

https://knowledge.hubspot.com/ja/design-manager/use-custom-social-icons-on-hubspot-pages-or-templates?hubs_content=knowledge.hubspot.com/design-manager/use-custom-social-icons-on-hubspot-pages-or-templates&hubs_content-cta=%E6%97%A5%E6%9C%AC%E8%AA%9E&__hstc=160394018.95bfac050dc09475b7109bd9703e29c1.1741513024770.1766815114056.1766901302668.56&__hssc=160394018.73.1766901302668&__hsfp=1463842514#%E3%82%BD%E3%83%BC%E3%82%B7%E3%83%A3%E3%83%AB%E3%82%A2%E3%82%A4%E3%82%B3%E3%83%B3%E3%82%92%E7%BD%AE%E3%81%8D%E6%8F%9B%E3%81%88%E3%82%8B


今回は icon8.com にあるアイコンを使用しました🎨

https://icons8.com/


### おわりに
モジュールをプレビューすると、無事ラインが追加されててアイコンがかわいいのになりました💟

![sns-share 09](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/sns-share%2009.png?width=1458&height=752&name=sns-share%2009.png)

こんな感じでデフォルトのモジュールを活用することで、案外簡単にカスタムモジュールを作ることもできます🌟🔭
ぜひ自分のテーマにあったボタンにカスタマイズしていきましょう～

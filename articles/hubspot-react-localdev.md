---
title: "【初心者向け】HubSpot CMSでReactモジュール開発！ローカルサーバーではじめる新しいテーマ作り"
emoji: "⚛️"
type: "tech"
topics: [HubSpot,React,HubSpot CMS]
published: true
published_at: 2025-05-01
---



# 【初心者向け】HubSpot CMSでReactモジュール開発！ローカルサーバーではじめる新しいテーマ作り

2025-05-01 · Reina

はじめに 🌿

2025 Spring Spotlightで発表された[「HubSpot CMSでのReactローカルサーバーアップデート」](https://developers.hubspot.com/blog/hubspot-cms-updates-spring-spotlight-2025#:~:text=The%20CMS%20React%20local%20development%20server%20is%20completely%20redesigned%20to%20help%20you%20streamline%20your%20development%20workflows.)…✨そのニュースを見て、もう「これはやってみたいっ！」って感じでした

（なんかよくわからないがすごそう）

https://www.youtube.com/watch?v=8FCrFzOP6_M&t=285s


だけどそのとき、実はReactほぼはじめまして状態。HTML→CSS→HubL/JS まできたけど、Reactの世界には一歩も踏み込めてなかったんだよね（小声）🙈

でも大丈夫。[チュートリアル](https://developers.hubspot.com/docs/guides/cms/quickstart/react-plus-hubl-quickstart)と格闘しながら、少しずつ「React×HubSpot CMS」が見えてきたので、この記事でシェアしてみますっ💻💖

この記事は「[Quickstart:Build a React + HubL theme](\"https://developers.hubspot.com/docs/guides/cms/quickstart/react-plus-hubl-quickstart\")」のチュートリアルをベースに勧めています

---

## この記事でわかること ✏️

* HubSpot CMSでReactを使うってどういうこと？🌀
* Spring Spotlightで追加されたローカルサーバー機能って何？🔧
* React初心者がつまずいたポイント＆解決策💥

---

## 結論 🎯

✨HubSpot CMSは、Reactモジュールを**ローカル開発サーバー（local development server）で編集→確認→アップロード**できるようになって、 **初心者でも「Reactモジュールが作れた！」って実感できるくらい快適で優しい環境になった！！**🌈

Reactの知識がちょっとずつでもあれば、**自分だけの動的なモジュール**が作れちゃうよ〜！

![ChatGPT Image 2025年5月1日 00_20_20](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%881%E6%97%A5%2000_20_20.png)

## Reactってなに？🌀

Reactは、Facebookが開発した「UI（見た目）を部品のように組み立てられるJavaScriptライブラリ」。

* `HTML` を JavaScriptの中に書ける「JSX」っていう記法が特徴的！
* 状態（state）を使って、動きのあるUIがつくれる✨
* コンポーネント単位で管理するから、再利用性バツグン！

HubSpot CMSでは、今まではHTML中心（厳密にいうとHubLって言語）+JS だったけど、Reactを使うとより「インタラクティブな動き」が作りやすくなるということみたい💭

[Intro to React for HubSpot Developers](\"https://developers.hubspot.com/blog/intro-to-react-for-hubspot-developers\") のページがわかりやすくまとまってておすすめ！

## やり方 🛠️

### 前提：

* HubSpot CLI を**グローバルでインストール**しておく（特定ディレクトリへのローカルインストールだと、手順1で実行する `npx @hubspot/create-cms-theme@latest` コマンドが失敗する⚡）
* しかし、`hs init` は**しないでおく！**（すでに `hubspot.config.yml` があるなら一旦削除推奨。`create-cms-theme` の実行でエラーがでるので注意！）

### 1：テーマを作成する

テーマを作るフォルダを用意して、`npx @hubspot/create-cms-theme@latest` を実行！💻

(基本的には指示通りに入力したり、処理を眺めてればOK)

![react_theme_01](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_theme_01.png)

(CLIインストールしてますがな‥)

![react_theme_02](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_theme_02.png)

(hs init をさせられている。ここですでにinit済みの環境だと、hs init できなかった！とすねてくる)

![react_theme_03](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_theme_03.png)

(ここまでくれば安心)

![react_theme_04](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_theme_04.png)

作成されたテーマは👇の構成になっているよ：

```
my-cms-theme/  
│── node_modules/  
├── src/theme/  
│   ├── assets/  
│   ├── components/  
│   │   └── modules/  
│   ├── node_modules/  
│   ├── styles/  
│   ├── templates/  
│   ├── fields.json  
│   ├── Globals.d.ts  
│   ├── package.json  
│   ├── theme.json  
│   └── tsconfig.json  
│── .prettierrc  
│── hsproject.json  
└── package.json  
  
ざっくり説明すると：
```


* `assets/`：画像などのアセット類
* `components/modules/`：ReactモジュールのJSXファイルなど
* `styles/`：CSSファイル
* `templates/`：HubLテンプレートファイル群

### 2：ローカルサーバに接続

作成されたテーマのディレクトリに移動して、`npm run start` を実行。 これでローカルサーバーが立ち上がるよ！🌐

![react_theme_04](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_theme_04.png)

(モジュールやテンプレートとしてなにがあるかがダッシュボードに表示されているね)

![react_theme_06](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_theme_06.png)

### 3：ローカルでいろいろと変更してみよう

モジュールの `index.jsx` を書き換えたり、CSSやテンプレートを変更してみよう✨

まず、テーマを作成した時点で `/components/modules/GettingStarted/index.jsx` というモジュールファイルが含まれているよ。 これを `/MyfirstModule/index.jsx` として複製すれば、オリジナルのモジュールとしてカスタマイズできる！

#### モジュールファイルの構成（例）
```jsx:index.jsx
// モジュール用のフィールドコンポーネントを読み込み
import {
  ModuleFields,
  TextField,
  RichTextField,
  ImageField,
} from '@hubspot/cms-components/fields';

// CMSコンポーネントやCSS、画像を読み込む
import { RichText } from '@hubspot/cms-components';
import logo from '../../../assets/sprocket.svg';
import styles from '../../../styles/getting-started.module.css';

// 表示コンポーネントを定義
export function Component({ fieldValues, hublParameters }) {
  const { src, alt, width, height } = fieldValues.logo;
  const { brandColors } = hublParameters;

  return (
    <div
      className={styles.wrapper}
      style={{ backgroundColor: brandColors?.color, opacity: brandColors?.opacity }}
    >
      <img src={src} alt={alt} width={width} height={height} />
      <h1>{fieldValues.headline}</h1>
      <RichText fieldPath="gettingStarted" />
      <div className={styles.buttons}>
        <a href="https://github.com/HubSpot/cms-react/tree/main/examples">Examples</a>
        <a href="https://github.hubspot.com/cms-react/">Read the Docs</a>
      </div>
    </div>
  );
}

// 初期値の定義
const richTextFieldDefaultValue = `
  <div>
    <div>
      <span>Deploy to your theme by running <pre>npm run deploy</pre> from the root directory</span>
    </div>
  </div>
`;

// フィールドの構造
export const fields = (
  <ModuleFields>
    <ImageField name="logo" label="Logo" default={{ src: logo, height: 100, alt: 'HubSpot logo' }} resizable={true} />
    <TextField name="headline" label="Headline" default="Getting Started with CMS React" />
    <RichTextField name="gettingStarted" label="Getting Started" default={richTextFieldDefaultValue} />
  </ModuleFields>
);
// モジュールのラベル
export const meta = {
  label: 'Getting Started with CMS React',
};
```

たとえば、見出しの Getting Started with CMS React を変更したいときは、<TextField /> の default="..." を書き換えればOK🍪

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/react-theme01%20(1).gif?width=1600&height=824&name=react-theme01%20(1).gif)

#### CSS ファイル
CSSファイルは import styles from '../../../styles/getting-started.module.css'; のように読み込まれているので、 これを複製して背景色やフォントを変えてみると、見た目の違いがすぐに体感できるよ👩‍🎨

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/react-theme02%20(1).gif?width=1600&height=824&name=react-theme02%20(1).gifL)

#### テンプレートファイル
ページテンプレートは templates/page.hubl.html にあり、これは普段どおりHubLで書かれているから安心！ このテンプレートを複製＆編集して、新しく作ったモジュールを表示するように変更すれば、 Reactモジュールがページ上でどう表示されるかもばっちり確認できるよ✏️

(見慣れた光景)
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/react_theme_07.png?width=2414&height=1298&name=react_theme_07.png)

書き換えた内容はリアルタイムでローカルサーバーに反映されるから、確認もらくらく！

### 4：HubSpot にアップロードしてみよう
hs project upload で、テーマ全体をCMSにアップロード！ Reactモジュールも含めてプロジェクトとして反映されるよ📦
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/react_theme_09.png?width=3422&height=1390&name=react_theme_09.png)

### 5：ページを作ってみよう
今回作ったテーマで新しいページを作成。
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/react_theme_08.png?width=1500&height=1234&name=react_theme_08.png)
モジュールが表示されていれば大成功！🎉
![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/react_theme_10.png?width=1500&height=820&name=react_theme_10.png)

### 注意ポイント ⚡
わたしがつまづいたところをいくつか紹介：

- HubSpot CLI のバージョン：HubSpot CLI v7 に移行していたんだけど、テーマ作成のコマンドは hs init を求めてくるので地味に対応していないのかも？（なのでまずは v6 で進めるのがよさそう）

- 文字化け：モジュール単体のプレビューではOKなのに、ページテンプレートに埋め込むと日本語や絵文字が `&#12345;` みたいに化けちゃうことが…！→本番ページでは問題なしだったから焦らなくてOKだった！

- ReactのJSがビルド＆アップロードされてなかった：hs project uploadを忘れると何も表示されない！

### まとめ ✨
HubSpotでReactがこんなにサクッと動くなんて…ちょっと前では考えられなかったよね！✨ ローカル開発サーバー機能のおかげで「コード書く→確認→修正」がスムーズになって、 React初心者の私でも、ちゃんと自分のモジュールが作れたよ〜！！🧁💻

この記事が、これからReact×HubSpot CMSをはじめる誰かの背中をちょっとでも押せたらうれしいな〜っ🌱

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%881%E6%97%A5%2000_29_09%20(1).png?width=700&height=466&name=ChatGPT%20Image%202025%E5%B9%B45%E6%9C%881%E6%97%A5%2000_29_09%20(1).png)

https://developers.hubspot.com/blog/intro-to-react-for-hubspot-developers

https://developers.hubspot.com/docs/guides/cms/quickstart/react-plus-hubl-quickstart

https://developers.hubspot.com/blog/hubspot-cms-updates-spring-spotlight-2025

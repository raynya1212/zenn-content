---
title: "画像付きプロフィールカードを作ろう【HubSpot × React DAY7】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot,React,HubSpot CMS]
published: true
published_at: 2025-05-08
---

# 画像付きプロフィールカードを作ろう【HubSpot × React DAY7】

2025-05-08 · Reina

## はじめに 🌿

社員紹介ページやチームメンバー紹介など、「写真＋名前＋ひとこと紹介」をまとめて表示したいときってあるよね。   
そんなときに活躍するのが、**画像付きプロフィールカード**！

今回は、**HubSpot CMS × React** を使って、ページ編集画面から画像・名前・紹介文を自由に入力できるプロフィールカードを作っていきます🐱💻

![パソコンでプロフィールカードを作る猫のイラスト](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/profilecard_intro_illustration.png)

---

## この記事でわかること ✏️

* HubSpotの `ImageField` を使った画像フィールドの作り方
* CMS編集画面で画像・テキストを編集できるモジュール構成
* `RepeatedFieldGroup` を使って複数人表示する方法
* `dangerouslySetInnerHTML` の意味と使いどころ

---

## 結論 🎯

`ImageField` と `RichTextField` を組み合わせれば、   
**社員紹介カードが簡単に作れる！**   
さらに `RepeatedFieldGroup` を使えば**複数人対応**もできちゃう✨

---

## Step1：1人分のプロフィールカードを作ろう 🧸

まずは基本の1人分プロフィールカードを作ってみよう！   
画像・名前・役職・紹介文をセットで表示していくよ。

👇 まずは完成イメージの動作はこちら

![react_day07](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_day07.gif)

### サンプルコード（1人表示）
```jsx:components/modules/07_ProfileCard/index.jsx
// フィールドの読み込み
import {
  ModuleFields,
  TextField,
  ImageField,
  RichTextField,
} from '@hubspot/cms-components/fields';
// デフォルト画像の読み込み
import image from '../../../assets/charo.png';

// fieldValues の宣言
export function Component({ fieldValues }) {
  const { src, alt, width, height } = fieldValues.image;
// 実際にページに返される情報
  return (
    <div
      style={{
        backgroundColor: '#faf0e6',
        borderRadius: '12px',
        padding: '24px',
        maxWidth: '400px',
        textAlign: 'center',
        margin: '0 auto',
        boxShadow: '0 4px 12px rgba(0,0,0,0.1)',
      }}
    >
      <img
        src={src}
        alt={alt}
        width={width}
        height={height}
        style={{ borderRadius: '50%', marginBottom: '16px' }}
      />
      <h2>{fieldValues.name_hi}</h2>
      <div style={{ fontStyle: 'italic', color: '#666' }}>
        <span>{fieldValues.position}</span>
      </div>
      <div
        style={{ marginTop: '12px', fontSize: '14px', color: '#333' }}
        dangerouslySetInnerHTML={{ __html: fieldValues.bio }}
        // HTML文字列を表示させるための書き方
      />
    </div>
  );
}
// フィールドの設定
export const fields = (
  <ModuleFields>
    <ImageField
      name="image"
      label="プロフィール画像"
      default={{ src: image, height: 100, alt: 'プロフィール画像' }}
      resizable={true}
    />
    <TextField
      name="name_hi"
      label="名前"
      default="サンプル ちゃろ"
    />
    <TextField
      name="position"
      label="役職"
      default="フロントエンドエンジニア"
    />
    <RichTextField
      name="bio"
      label="紹介文"
      default="<p>Reactと猫が好きなエンジニアです。</p>"
    />
  </ModuleFields>
);

// モジュールのメタ情報
export const meta = {
  label: '07_プロファイルカード',
};
```

---

## Step2：複数人のプロフィールを繰り返し表示しよう 🐾

応用編として、今度はチーム紹介のように**複数人分のカード**を表示してみましょう。   
Reactでのループ処理とCMSフィールドを組み合わせるだけで、かなり柔軟に使えるモジュールに！

👇 複数表示のモジュールの動きはこんな感じ

![react_day07_1](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_day07_1.gif)

### サンプルコード（繰り返し対応）

```jsx:components/modules/07_ProfileCard/index.jsx
import {
  ModuleFields,
  TextField,
  ImageField,
  RichTextField,
  RepeatedFieldGroup,
  // 追加で繰り返しフィールドを読み込む
} from '@hubspot/cms-components/fields';
import image from '../../../assets/charo.png';

export function Component({ fieldValues }) {
  const { cards } = fieldValues;

  return (
    <div style={{ display: 'flex', flexWrap: 'wrap', gap: '20px', justifyContent: 'center' }}>
      {cards?.map((card, index) => {
      // map () を使って配列から表示させる
        const { image, name_hi, position, bio } = card;
        return (
          <div
            key={index}
            style={{
              backgroundColor: '#faf0e6',
              borderRadius: '12px',
              padding: '24px',
              maxWidth: '300px',
              textAlign: 'center',
              boxShadow: '0 4px 12px rgba(0,0,0,0.1)',
            }}
          >
            <img
              src={image.src}
              alt={image.alt}
              width={image.width}
              height={image.height}
              style={{ borderRadius: '50%', marginBottom: '16px' }}
            />
            <h2>{name_hi}</h2>
            <div style={{ fontStyle: 'italic', color: '#666' }}>{position}</div>
            <div
              style={{ marginTop: '12px', fontSize: '14px', color: '#333' }}
              dangerouslySetInnerHTML={{ __html: bio }}
            />
          </div>
        );
      })}
    </div>
  );
}
// RepeatedFieldGroupを追加
export const fields = (
  <ModuleFields>
    <RepeatedFieldGroup
      name="cards"
      label="カード一覧"
      occurrence={{ min: 1, max: 10, default: 1 }}
      default={[
        {
          image: { src: image, alt: 'プロフィール画像', height: 100 },
          name_hi: 'サンプルちゃろ',
          position: 'フロントエンドエンジニア',
          bio: '<p>Reactと猫が好きなエンジニアです。</p>',
        },
      ]}
    >
      <ImageField
        name="image"
        label="プロフィール画像"
        default={{ src: image, height: 100, alt: 'プロフィール画像' }}
        resizable={true}
      />
      <TextField name="name_hi" label="名前" default="サンプルちゃろ" />
      <TextField name="position" label="役職" default="フロントエンドエンジニア" />
      <RichTextField
        name="bio"
        label="紹介文"
        default="<p>Reactと猫が好きなエンジニアです。</p>"
      />
    </RepeatedFieldGroup>
  </ModuleFields>
);

export const meta = {
  label: '07_プロファイルカード（複数対応）',
};
```

繰り返しフィールド、map の書き方については以下記事を参考にしてね：
https://zenn.dev/raynya/articles/hubspot-react-repeater-day5-full


### フィールド名の注意点
`RepeatedFieldGroup` を使うときは、**フィールドの `name` と default のキー名を完全に一致させる必要がある**よ！   
たとえば `name=\"name_hi\"` にしているなら、default の中も `name_hi` にしておかないと、初期値が反映されないので注意👀


---

## 🧠 `dangerouslySetInnerHTML` って何？

`dangerouslySetInnerHTML` は、HTMLタグつきの文字列（たとえば `<p>〜</p>`）をReactでそのまま表示したいときに使う特別な書き方になります。

### なんで必要なの？

Reactは `<p>こんにちは</p>` のようなHTMLタグを含む文字列を、**普通の文字列として表示**しちゃうんだよね。   
だから、**これはHTMLとして描画してね！**ってわざわざ指示してあげないといけない。

```
<div dangerouslySetInnerHTML={{ __html: fieldValues.bio }} />
```

これを使うと、CMSから入力された `<p>` や `<strong>` などのタグがちゃんと適用されて表示されるようになるの。

### 「dangerously」がついている理由
名前がちょっと怖いのは、セキュリティ上の理由から。   
たとえば悪意のある `<script>` タグなんかが混ざってたら危険ですよね(・。・;   
だから[Reactは「これほんとに描画して大丈夫？危なくない？」ってあえて警告っぽくしてあるようです](\"https://ja.react.dev/reference/react-dom/components/common#dangerously-setting-the-inner-html:~:text=%E3%83%9E%E3%83%BC%E3%82%AF%E3%82%A2%E3%83%83%E3%83%97%E3%81%8C%E5%AE%8C%E5%85%A8%E3%81%AB%E4%BF%A1%E9%A0%BC%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%BD%E3%83%BC%E3%82%B9%E3%81%8B%E3%82%89%E6%9D%A5%E3%81%A6%E3%81%84%E3%81%AA%E3%81%84%E9%99%90%E3%82%8A%E3%80%81%E3%81%93%E3%81%AE%E6%96%B9%E6%B3%95%E3%82%92%E4%BD%BF%E3%81%86%E3%81%A8%E3%81%84%E3%81%A8%E3%82%82%E7%B0%A1%E5%8D%98%E3%81%AB%20XSS%20%E8%84%86%E5%BC%B1%E6%80%A7%E3%81%8C%E7%99%BA%E7%94%9F%E3%81%97%E3%81%BE%E3%81%99%E3%80%82\")。

でもHubSpotの `RichTextField` は、CMS側でちゃんと安全に処理してくれるから、基本的には安心して使ってOK（のはず）！

---

## まとめ ✨

Reactモジュールでも、`ImageField` と `RichTextField` を使えば、   
CMSで編集しやすくて見た目もかわいいプロフィールカードが作れちゃう！

`RepeatedFieldGroup` を使って繰り返しフィールドを追加すれば、**チーム紹介や社員紹介ページにもばっちり対応できる**よ！

![プロフィールカードを表示したモニターを見つめる女性のイラスト](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/profilecard_summary_illustration.png)

---

## あわせて読みたい 📖

* [HubSpot CMS × Reactで繰り返しフィールド付きモジュールを作成する方法【Day5】](https://zenn.dev/raynya/articles/hubspot-react-repeater-day5-full)
* [HubSpot CMS Reference|Module and theme fields>Image](https://developers.hubspot.com/docs/reference/cms/fields/module-theme-fields#image)
* [React | 危険を冒して内部 HTML をセットする](https://ja.react.dev/reference/react-dom/components/common#dangerously-setting-the-inner-html)
---
title: "HubSpot CMS × Reactで繰り返しフィールド付きモジュールを作成する方法【Day5】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot, React, HubSpot CMS]
published: true
published_at: 2025-05-06
---

# HubSpot CMS × Reactで繰り返しフィールド付きモジュールを作成する方法【Day5】

2025-05-06 · Reina

### はじめに 🌿
HubSpotのモジュールで、ページエディター側で自由に項目を追加したり複製したりできるのってありますよね。（これこれ↓）

![](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/Imported%20sitepage%20images/repeater%20field.png)

これは「Repeated field（繰り返しフィールド）」っていいます✍
今日はそれをReactモジュールで作ってみます✨

https://developers.hubspot.jp/docs/guides/cms/content/fields/overview#%E3%83%AA%E3%83%94%E3%83%BC%E3%82%BF%E3%83%BC


### この記事でわかること ✏️
- Repeated field groupsの使い方 🧪
- CMSに追加したデータをmap()で表示する方法 💻
- Reactで繰り返しカードを表示するフロー ✨

### 結論 🎯
HubSpot CMS × Reactモジュールでは、
Repeated field groups で入力、map() で表示する！
これが基本！📚

### 今日のモジュール 🛠️
HubSpotのCMSで入力された「カードの繰り返し」を、Reactでmap()を使って表示するモジュールをつくるよ〜シンプルにタイトルと説明文だけを扱う構成！

![](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react_day05.gif)

#### サンプルコード（05_IconCardList.jsx）
```jsx
import {
  ModuleFields, // CMSフィールドを定義するための基本コンポーネント
  RepeatedFieldGroup, // 繰り返し入力用フィールドグループ
  TextField, // テキスト入力フィールド
} from '@hubspot/cms-components/fields';

// CMSフィールド定義
export const fields = (
  <ModuleFields>
    <RepeatedFieldGroup
      name="cards"
      label="カード一覧"
      occurrence={{ min: 1, max: 10, default: 3 }} // 最小1、最大10、初期3つ
      default={[
        { title: 'おすすめトピック', description: 'ぜひチェックしてほしい、注目のコンテンツだよ♪' },
        { title: 'みんなの声', description: 'ユーザーのリアルな感想を集めました📣' },
        { title: 'はじめての方へ', description: 'まずはここから！基本をわかりやすくご案内💡' },
      ]}
    >
      <TextField name="title" label="タイトル" required default="タイトルのサンプル" />
      <TextField name="description" label="説明文" required default="これはサンプルの説明文です。" />
    </RepeatedFieldGroup>
  </ModuleFields>
);

// 表示コンポーネント
export const Component = ({ fieldValues }) => {
  return (
    <div style={{ display: 'flex', flexWrap: 'wrap', gap: '16px' }}>
      {fieldValues.cards?.map((card, index) => (
        <div
          key={index} // Reactが要素を一意に識別するためのkey
          style={{
            border: '1px solid #ccc', // 枠線
            padding: '16px', // 内側の余白
            borderRadius: '8px', // 角の丸み
            width: '250px', // 幅指定
          }}
        >
          <h3>{card.title}</h3> {/* タイトル表示 */}
          <p>{card.description}</p> {/* 説明文表示 */}
        </div>
      ))}
    </div>
  );
};


// メタ情報（HubSpot UIでのラベル）
export const meta = {
  label: '05_アイコンカード（簡易）',
};
```

### Repeated field groupsの役割 📦
HubSpot CMSでは、1つのフィールドだけじゃなくて「複数のデータをまとめて入力できる」ようにしたいとき、`<RepeatedFieldGroup>` を使います。
`<RepeatedFieldGroup>` を使うことでCMSの編集画面では、繰り返しフィールド扱いになって、同じ項目を持つカードを簡単に複製・追加できるようになる📦
でも！実際にページに表示するためには……

### CMSデータの表示：map()の登場 🧪
`<RepeatedFieldGroup>` はあくまでもページエディター側の状態なので、実際に設定した項目をページに表示させるには **map()** で配列を表示する必要がある！

📎 RepeatedFieldGroupでCMSからもらった配列の例：
```jsx
[
  { title: "おすすめ", description: "注目の内容" },
  { title: "新着", description: "最近追加されたよ" }
]
```

🔁 `map()` で変換！
```jsx
cards.map((card, index) => (
  <article key={index}>
    <h4>{card.title}</h4>
    <p>{card.description}</p>
  </article>
));
```
➡ 各オブジェクト → HTMLのカードになる！！

### map() で表示するとは？ 🍎🍌🍊
まずは超シンプルな例から

🧠 データ（配列）
```jsx
const fruits = ['🍎', '🍌', '🍊'];
```

🔁 `map()` を使うと…
```jsx
const result = fruits.map((fruit) => { return `くだもの:${fruit}`; });
```

✅ 結果
```jsx
["くだもの:🍎", "くだもの:🍌", "くだもの:🍊"]
```

Reactではどうなる？
```jsx
<ul>
  {fruits.map((fruit, index) => (
    <li key={index}>くだもの:{fruit}</li>
  ))}
</ul>
```
表示されるのは：
- くだもの:🍎
- くだもの:🍌
- くだもの:🍊

### Reactでの map() ってこういうこと！
|データ | map() で変換 | 結果 |
| ---- | ---- | ---- |
| ['🍎', '🍌', '🍊'] | <li>くだもの：🍎</li> に変える | <ul>...リスト</ul> に！ |

### まとめ ✨
HubSpotのReactモジュールでは、
「入力用（RepeatedFieldGroup）」と「表示用（map）」の2ステップがとっても大事💡
慣れてきたら `IconField` や `ImageField` を追加してもっとリッチにできる。
まずはこの「繰り返し表示 × map()」の基本を、しっかり身につけていこう！

![](https://blog.ryamagami.com/hs-fs/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%884%E6%97%A5%2000_26_41.png?width=700&height=466&name=ChatGPT%20Image%202025%E5%B9%B45%E6%9C%884%E6%97%A5%2000_26_41.png)


### あわせて読みたい 📚

https://ja.react.dev/learn/rendering-lists

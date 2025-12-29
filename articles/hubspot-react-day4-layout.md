---
title: "レイアウトと色を切り替えられるHubSpot×Reactモジュールを作ろう！【HubSpot×React Day4】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot, React, HubSpot CMS]
published: true
published_at: 2025-05-05
---

# レイアウトと色を切り替えられるHubSpot×Reactモジュールを作ろう！【Day4】

2025-05-05 · Reina

## はじめに 🌿

HubSpotのモジュールで簡単にデザインを切り替えられたら便利そうだなって思いませんか？（思って）  
たとえば「横並びか縦並びか」「ライトかダークか」…   
今日はその“切り替え可能な表示”を、HubSpotのCMSフィールドとisland機能で実装してみます！

![HubSpotのUIでレイアウト切り替えを設定している女性のイラスト](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/react-hubspot-layout-switch-intro%20(1).png)

「React強化月間」Day4、いってみよ〜🌟

---

## この記事でわかること ✏️

* CMSフィールドでレイアウトや色を切り替える方法
* islandとpropsを使って動的表示を実現する方法
* `ChoiceField`でハマりやすいポイント

---

## 結論 🎯

islandにpropsを渡して、表示ロジックを分ければ、   
**フィールド値に応じてコンポーネントのデザインを自在に変えられる！**

---

## 今日のモジュール 🧩

今回は、CMSフィールドで指定されたレイアウト（横 or 縦）とカラーテーマ（ライト or ダーク）を使って、   
**1つのカードコンポーネントの見た目をまるっと切り替えられるモジュール**を作ります！

こんな感じ👇で、ページエディター上のドロップダウンで選ばれたレイアウト・カラーテーマに切り替えることができちゃう🔦

![react-day04](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react-day04.gif)

### モジュールの構成
   **`ChoiceField` で選択肢を用意**
   **その値を `Component` から `Island` に渡す**
   **`Island` 側でスタイルを切り替える**


? island ってな方は前回の記事を読んでみてね：
https://blog.ryamagami.com/https/blog.ryamagami.com/hubspot-react\_module\_03

### サンプルコード

```jsx:/components/modules/index.jsx
import { Island } from '@hubspot/cms-components'; // Island（動的コンポーネント）呼び出し用
import CustomLayoutCardIsland from '../../islands/CustomLayoutCard?island'; // Islandのモジュールを読み込み
import { ModuleFields, ChoiceField } from '@hubspot/cms-components/fields'; // フィールド定義用の部品たち

// ページ上で表示するReactコンポーネント（islandを呼び出す）
export function Component({ fieldValues }) {
  // HubSpotエディターで設定された値を取り出す
  const { layout, colorTheme } = fieldValues;

  // Islandを描画し、propsとしてフィールドの値を渡す
  return (
    <Island
      module={CustomLayoutCardIsland}
      layout={layout}
      colorTheme={colorTheme}
    />
  );
}

// エディターで表示されるフィールドの定義
export const fields = (
  <ModuleFields>
    <ChoiceField
      name="layout" // フィールドの内部名
      label="コンテンツ配置" // エディターでの表示名
      required={false}
      locked={false}
      multiple={false}
      display="select"
      choices={[
        ['vertical', '縦並び'],
        ['horizontal', '横並び']
      ]}
      default="horizontal" // 初期値
    />
    <ChoiceField
      name="colorTheme"
      label="カラーテーマ"
      required={false}
      locked={false}
      multiple={false}
      display="select"
      choices={[
        ['light', 'ライト'],
        ['dark', 'ダーク']
      ]}
      default="light"
    />
  </ModuleFields>
);

// HubSpot上でのモジュール名ラベル
export const meta = {
  label: '04_レイアウト切り替えカード',
};

```

```jsx:/components/islands/CustomLayoutCard.jsx
import React from 'react'; // Reactを読み込む

// Island（クライアント側で動く）としてのコンポーネント
export default function CustomLayoutCardIsland({ layout, colorTheme }) {
  // layoutがhorizontalなら横並び、それ以外は縦並びとして判定
  const isHorizontal = layout === 'horizontal';

  // カード全体のスタイル（色・並びなど）
  const containerStyle = {
    padding: '24px',
    borderRadius: '12px',
    display: 'flex',
    gap: '16px',
    alignItems: 'center',
    transition: 'background-color 0.3s ease',
    backgroundColor: colorTheme === 'dark' ? '#1f2937' : '#ffffff', // ダークかライトかで背景色
    color: colorTheme === 'dark' ? '#ffffff' : '#000000', // 同じく文字色
    flexDirection: isHorizontal ? 'row' : 'column', // 横並びか縦並びか
  };

  // サムネイルのスタイル（四角のダミー画像っぽい）
  const thumbnailStyle = {
    width: '100px',
    height: '100px',
    backgroundColor: '#ccc',
    borderRadius: '8px',
  };

  // タイトルのスタイル
  const titleStyle = {
    fontSize: '20px',
    fontWeight: 'bold',
    marginBottom: '8px',
  };

  // 説明文のスタイル
  const descriptionStyle = {
    fontSize: '16px',
  };

  // 実際の描画部分（サムネイル＋テキストを並べる）
  return (
    <div style={containerStyle}>
      <div style={thumbnailStyle}></div>
      <div>
        <h2 style={titleStyle}>カスタムカード</h2>
        <p style={descriptionStyle}>フィールドから見た目を切り替えられるよ！🎨</p>
      </div>
    </div>
  );
}

```

### ポイント① フィールド定義：選べるようにしよう！

```jsx
export const fields = (
  <ModuleFields>
    <ChoiceField
      name="layout"
      label="コンテンツ配置"
      default="horizontal"
      choices={[
        ['vertical', '縦並び'],
        ['horizontal', '横並び']
      ]}
    />
    <ChoiceField
      name="colorTheme"
      label="カラーテーマ"
      default="light"
      choices={[
        ['light', 'ライト'],
        ['dark', 'ダーク']
      ]}
    />
  </ModuleFields>
);
```
ここで選んだ値が、実際の見た目の切り替えに使われるよ！

### ポイント②：モジュールの中でislandに渡す 💌

```jsx
export function Component({ fieldValues }) {
  const { layout, colorTheme } = fieldValues;

  return (
    <Island
      module={CustomLayoutCardIsland}
      layout={layout}
      colorTheme={colorTheme}
    />
  );
}
```
ここでは特に「layout」と「colorTheme」という2つのpropsを渡してる👀   
このpropsが、次のステップでスタイルに効いてくるよ〜！

### ポイント③：Island内でスタイルを変える！🎨

```jsx
export default function CustomLayoutCardIsland({ layout, colorTheme }) {
  const isHorizontal = layout === 'horizontal';

  const containerStyle = {
    padding: '24px',
    borderRadius: '12px',
    display: 'flex',
    gap: '16px',
    alignItems: 'center',
    transition: 'background-color 0.3s ease',
    backgroundColor: colorTheme === 'dark' ? '#1f2937' : '#ffffff',
    color: colorTheme === 'dark' ? '#ffffff' : '#000000',
    flexDirection: isHorizontal ? 'row' : 'column', // ← ここがレイアウト切り替え！
  };

  return (
    <div style={containerStyle}>
      <div style={{ width: '100px', height: '100px', backgroundColor: '#ccc', borderRadius: '8px' }} />
      <div>
        <h2 style={{ fontSize: '20px', fontWeight: 'bold', marginBottom: '8px' }}>カスタムカード</h2>
        <p style={{ fontSize: '16px' }}>フィールドから見た目を切り替えられるよ！🎨</p>
      </div>
    </div>
  );
}
```


🧡 **ここが今回のステップアップポイント！**   
[前回](https://blog.ryamagami.com/hubspot-react_module_03)まではテキストや表示の切り替えだけだったけど、今回は**スタイル（見た目そのもの）**を制御してるところが新しいポイント🎨


**補足：三項演算子での条件式、= がないのはなぜ？**
```
flexDirection: isHorizontal ? 'row' : 'column'
```

この行では三項演算子（〇〇 ? △△ : □□）を使って、  
isHorizontal が true かどうかでレイアウトを切り替えています！

他のスタイルでは colorTheme = 'dark' のように比較をしていましたが、  
この isHorizontal はすでに true / false の boolean型の値。

つまり、**あらためて `===` で比較しなくても条件として使える**✨

---

## 注意ポイント ⚡

ChoiceFieldの書き方がちゃんとできてなくて、エラーたくさん出しちゃった😿

[公式ドキュメント](\"https://developers.hubspot.com/docs/reference/cms/fields/module-theme-fields#choice\")にReactでの書き方例が追加されているから、それを参考に書くのがおすすめ！

---

## まとめ ✨

islandにpropsを渡すだけで、フィールドに応じた動的なデザイン切り替えが実現できるよ！   
HubSpot CMSでもここまでReactらしい開発ができるなんて、ちょっと感動じゃない？🥺

![ReactとHubSpotの連携を象徴する、レイアウト構成図とコード風背景のあるコラボ風イラスト](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/react-hubspot-collaboration-summary%20(1).png)

次はもっと応用的な構成に挑戦してみるね！🪄
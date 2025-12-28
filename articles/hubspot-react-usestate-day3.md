---
title: "HubSpot CMS React × useState｜動かない原因とisland構成での対応【Day3】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot, React, HubSpot CMS]
published: true
published_at: 2025-05-04
---

# HubSpot CMS React × useState｜動かない原因とisland構成での対応【Day3】

2025-05-04 · Reina

![ChatGPT Image](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%883%E6%97%A5%2014_31_23%20(1).png)

### はじめに 🌿
HubSpotのReactモジュールで「クリックで表示が切り替わるUI」を作ってみたい！
そんな思いで useState を使ったモジュールにチャレンジしたら、思ったように動かない...！？
今回は、Reactの状態管理（useState） と HubSpot CMS の相性、そしてそれを解決するための island構成 について、実体験ベースでまとめます✨

### この記事でわかること ✍️
- `useState` を HubSpot Reactモジュールに使うときの注意点
- island構成ってなに？どう書けばいいの？
- Reactの状態管理がちゃんと動くまでの道のり

### 今日のモジュール 🛠️
クリックするたびに、設定した3つのフレーズが順番に切り替わるUIを作りたい！
そのために `useState` を使ってみたけど……？

### `useState` ってなに？🌱
Reactでは「状態」を管理するために useState という仕組みを使います。
これを使うと、ボタンのクリックや入力フォームの変化に応じて、表示を変えたりできます。

`const [状態の変数, 状態を変更する関数] = useState(初期値);`

たとえば：

`const [count, setCount] = useState(0);`
この場合、count は現在の値（状態）、setCount は値を更新する関数です。


### useStateを使ったモジュールを書いてみた👀  
最初はこんなコードを書いてみました：

```javascript
export const Component = () => {
  const [index, setIndex] = useState(0);
  const phrases = ['Reactは楽しい！', '状態がすべてを変える', 'useStateマスターへの道'];

  return (
    <>
      {phrases[index]}
      <button onClick={() => setIndex((i) => (i + 1) % phrases.length)}>次へ</button>
    </>
  );
};
```

でも、クリックしても表示が変わらない！！😿  

### Dev Slackでヒントを発見 🔍
え〜どうしてなんだろう〜といろいろ探してみたところ、HubSpot Developers Slack でこんなコメントを発見👇

> And when using methods like useState, useEffect, etc, your module needs to be in an island to enable client side interactivity ([what is an island](https://www.patterns.dev/vanilla/islands-architecture/)) as CMS React is server side rendered (SSR) by default.

HubSpotのCMS ReactはSSR（サーバーサイドレンダリング）だから、クライアント側での動作（useStateなど）はisland構成に分けて書く必要がある！ということに気がつきました💬

### island構成ってなに？ 🌍
公式ドキュメントに書かれている説明を噛み砕いてまとめると：

- island とは、Reactコンポーネントの中でも **クライアント側でインタラクションが必要な部分だけを切り出した構成** のこと。
- HubSpot CMSではページ全体はSSRで描画され、インタラクティブな部分だけ後からJavaScriptで **hydrate（復元）** される仕組み。

参考：
- 🔗 [HubSpot CMS Reference – React Islands](https://developers.hubspot.com/docs/reference/cms/react/islands)
- 🔗 [React – hydrateRoot](https://react.dev/reference/react-dom/client/hydrateRoot)

### 正しい構成で書き直してみた 💡

#### 📁 フォルダ構成のイメージ：
```text
src/theme/components/
├── modules/
│   └── 03_ChangePhrases/
│       └── index.jsx
└── islands/
    └── ChangePhraseIsland.jsx ← useState などを使うファイル
```

> ⚠️ islandのファイル名に「03_」など数字から始めるとビルドでエラーになります！（体験談）

#### 🔊 モジュール側（例：03_ChangePhrases/index.jsx）
```javascript
// Islandとして切り出したコンポーネントを読み込むためのimport
import { Island } from '@hubspot/cms-components';

// islandファイルのパスに?islandをつけてインポート（ここがポイント！）
import ChangePhraseIsland from '../../islands/ChangePhraseIsland.jsx?island';

// HubSpot用のフィールド定義を読み込む
import { ModuleFields, TextField } from '@hubspot/cms-components/fields';

// モジュールのメインコンポーネント（HubSpotがこの関数を実行）
export function Component({ fieldValues }) {
  // ページエディターで設定されたフィールドの値を受け取る（propsとして）
  const { phrase1, phrase2, phrase3 } = fieldValues;

  // islandコンポーネントを表示し、propsとしてフレーズを渡す
  return (
    <Island>
      <ChangePhraseIsland phrase1={phrase1} phrase2={phrase2} phrase3={phrase3} />
    </Island>
  );
}

// モジュールエディターで表示される入力フィールドを定義
export const fields = (
  <ModuleFields>
    <TextField name="phrase1" label="フレーズ1" />
    <TextField name="phrase2" label="フレーズ2" />
    <TextField name="phrase3" label="フレーズ3" />
  </ModuleFields>
);

// このモジュールの名前（エディター上に表示される）
export const meta = {
  label: '03_クリックで言葉を切り替え',
};
```

#### 💡 island側（例：ChangePhraseIsland.jsx）
```javascript
// ReactのuseStateフックを使うためにインポート
import React, { useState } from 'react';

// propsとして受け取ったフレーズ3つを元に、表示を切り替えるコンポーネント
export default function ChangePhraseIsland({ phrase1, phrase2, phrase3 }) {
  // 受け取ったフレーズ3つを1つの配列にまとめる
  const phrases = [phrase1, phrase2, phrase3];

  // 現在表示中のフレーズのインデックスを状態として管理（初期値は0番目）
  const [index, setIndex] = useState(0);

  return (
    <div style={{ textAlign: 'center', padding: '1rem' }}>
      {/* 現在のインデックスに対応するフレーズを太字＆大きめで表示 */}
      <h2>{phrases[index]}</h2>

      {/* ボタンをクリックすると次のインデックスに切り替わる（ループ） */}
      <button onClick={() => setIndex((i) => (i + 1) % phrases.length)}>
        次のフレーズ ▶︎
      </button>
    </div>
  );
}
```

こんな感じ👇で、ページエディターで設定されたフレーズが「次のフレーズ ▶」ボタンで切り替わるようになりました✨

![ChatGPT Image](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react-day03_1.gif)

### 注意ポイント ⚡
- ✅ のついた import を忘れずに！
- ✅ island は「クライアントJSが必要な処理」だけ切り出す
- ✅ ファイル名の先頭は英字にする（数字始まりはビルドNG）
- ✅ モジュール側で `Island` コンポーネントを使い、props経由で値を渡す

### まとめ ✨
以前 どこかで islandsの説明をなんとなくみてはいたけど、今回のチャレンジで「HubSpot CMS Reactで useState を使うには island が必要」というのがようやく腹落ちしました。
最初はなんで動かないのかわからず苦戦したけど、ドキュメントとSlackとにらめっこして、ちゃんと動くモジュールが作れたときの感動は忘れません…！！（泣）

![ChatGPT Image](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B45%E6%9C%883%E6%97%A5%2014_41_18%20(1).png)

次は island に渡すフィールドをもっと自由に扱って、表示やデザインを切り替えるようなモジュールに挑戦してみたいと思います！（Day4へつづく…🌟）

### あわせて読みたい 📖
- 【Day1】propsって何？HubSpot×Reactでキャッチコピー表示してみた — https://blog.ryamagami.com/hubspot-react_module_01
- 【Day2】チェックひとつで背景色が変わる！BooleanFieldで学ぶ条件レンダリング — https://blog.ryamagami.com/hubspot-react_module_02

---
title: "islands × props を使って、ON/OFF制御を実装してみた【HubSpot × React DAY9】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot, React, HubSpot CMS]
published: true
published_at: 2025-05-01
---


# islands × props を使って、ON/OFF制御を実装してみた【HubSpot × React DAY9】

2025-05-10 · Reina

## はじめに 🌿

今日は[DAY3](https://blog.ryamagami.com/https/blog.ryamagami.com/hubspot-react_module_03)と[DAY4](https://blog.ryamagami.com/https/blog.ryamagami.com/hubspot-react_module_04)で扱った `islands` を、**もっとシンプルなモジュールで深掘り**してみます！

「ある条件でだけ特定のパーツを表示したいな〜」という場面、あるよね？   
今回は `props` を使って **表示・非表示を切り替えるモジュール** を作ってみました💡

![チェックボックスで子コンポーネントの表示を切り替えるislands × propsの構造図](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/day9_illustration_props-toggle-flow.png)

---

## この記事でわかること ✏️

* islands経由でpropsを渡す方法 💡
* 「子コンポーネント＝モジュールの一部として分けた表示パーツ」を操作するイメージ 🧸
* islandsの役割と基本的な仕組み 🧠

---

## 結論 🎯

**islands経由でpropsを渡すだけで**、Reactモジュール内の表示ON/OFFはとっても簡単に実装できる！

---

## 前提 🏗

HubSpot CMSでReactモジュールを使うとき、基本は **SSR（サーバーサイドレンダリング）** で動作しています。   
なので、たとえば `useState` を使ってUIの動きや表示を変える場合、**その処理をislandsに切り出す必要がある**んです。

今回のテーマ「表示のON/OFF」も、クライアントサイドの動作なので、**islandとして切り分けたパーツに処理を書きます**🧩

---

## やり方 🛠️

### Step1：親モジュールでBooleanフィールドを定義する 🗂️

```jsx:index.jsx
export const fields = (
  <ModuleFields>
    <BooleanField
      name="isVisible"
      label="子コンポーネントを表示する？"
      default={true}
    />
  </ModuleFields>
);
```

HubSpotのエディター上でチェックON/OFFができるようになります ✅   
この `isVisible` を使って切り替え！

---

### Step2：Islandにpropsとして渡す 📦

```jsx:index.jsx
export function Component({ fieldValues }) {
  const { isVisible } = fieldValues;

  return (
    <Island module={ToggleChildIsland} isVisible={isVisible} />
  );
}
```

islandには `props` を渡せるよ！   
ここで渡した値が、クライアント側の表示切り替えロジックに活用される仕組み。

---

### Step3：表示切り替えロジックをisland側に書く 🌈

```jsx:islands/ToggleChildIsland.jsx
import React from 'react';

export default function ToggleChildIsland({ isVisible }) {
  if (!isVisible) {
    return <p>（子コンポーネントは現在非表示です）</p>;
  }

  return (
    <div className="p-4 bg-green-100 rounded-xl">
      <p>✅ 子コンポーネントが表示されています！</p>
    </div>
  );
}
```

「子コンポーネント」って言うと難しそうだけど、ここでは   
👉 `ToggleChildIsland` という**モジュール内のサブパーツ（小さなReact関数コンポーネント）**のこと。   
表示・非表示を `props` で制御してるってこと！

---

## 注意ポイント ⚡ islandsを解説するよ

> 🧭 **「islands」って何？**   
> HubSpot CMSのReactモジュールでは、**クライアント側のJS処理が必要な箇所だけを "island" として切り出す構成**が推奨されています。

つまり `islands` を使うことで：

* ページ全体をJSでクライアントレンダリングせずに、
* **一部だけを React で動的に**制御できる！

---

### 🌍 Islandのメリット（公式ドキュメントより）

| 概要 | 説明 |
| --- | --- |
| SSRとの共存 | ページの読み込みは高速なSSR、動的部分はクライアントでJS実行できる |
| パフォーマンス向上 | クライアント側のJSは必要最小限に抑えられる |
| Reactだけで完結 | island内部は完全なReactとして書ける（useState等もOK！） |

📚 参考：[HubSpot公式ドキュメント – islands](https://developers.hubspot.com/docs/reference/cms/react/islands)

### 使い方のポイント


  `Island` コンポーネントをimportして使う（`@hubspot/cms-components`）

  propsは `Island` 経由で渡すこと！


  island側のファイルは `.jsx?island` の形でimportするのを忘れずに！

`import MyIslandComponent from './MyIslandComponent.jsx?island';`


この形じゃないと、**クライアントJSとして認識されない**ので注意⚠️

---

## まとめ ✨

islandsとpropsを組み合わせれば、**表示のON/OFFはとてもシンプルに実装可能！**   
Reactの知識を活かしながら、HubSpot CMSに自然に溶け込ませる方法がわかってきたかも🐾

islands構成の理解を深めつつ、次のモジュール開発にもつなげていこう〜！

![表示ON/OFFはとてもシンプルに実装できる！と伝えるキャラクターとHubSpot×Reactロゴ](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/day9_illustration_summary-message.png)

---

## あわせて読みたい 📖

* [[HubSpot CMS React × useState｜動かない原因とisland構成での対応【Day3】](/hubspot-react_module_03)
* [レイアウトと色を切り替えられるHubSpot×Reactモジュールを作ろう！【Day4】](https://blog.ryamagami.com/hubspot-react_module_04)
* [HubSpot CMS Reference | Islands](https://developers.hubspot.com/docs/reference/cms/react/islands)
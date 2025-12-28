---
title: "アコーディオン付きFAQを作ってみた！【HubSpot × React DAY10】"
emoji: "⚛️"
type: "tech"
topics: [HubSpot, React, HubSpot CMS]
published: true
published_at: 2025-05-11
---

# アコーディオン付きFAQを作ってみた！【HubSpot × React DAY10】

2025-05-11 · Reina

## はじめに 🌿

FAQリストって、たくさんあると見づらくなりがち…。

![アコーディオンを開こうとするちゃろさん（困り顔）](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/faq-opening-illustration.png)  
**アコーディオン形式で「質問をクリックすると回答が開く」UI**なら、ユーザーも編集者も嬉しい！   
今回は HubSpot の **Reactモジュール + islands + RepeatedFieldGroup** を使って、動きのあるFAQモジュールを作ってみたよ✨

![react-day10](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/MeowBit.dev/Blog%20images/react-day10.gif)

---

## この記事でわかること ✏️

* アコーディオン形式のFAQリストを実装する方法
* `useState` の状態管理と islands の関係
* `RepeatedFieldGroup` で自由にQAを追加する方法

---

## 結論 🎯

HubSpot CMS × React で、**アコーディオンUIをつけたいなら islands が必須！**   
さらに `RepeatedFieldGroup` を使えば、質問・回答を管理画面から自由に増やせて便利💡

---

## やり方 🛠️

### Step1：フィールド定義で RepeatedFieldGroup を使おう ✍️

```jsx:index.jsx
export const fields = (
  <ModuleFields>
    <RepeatedFieldGroup
      name="faqs"
      label="FAQリスト"
      occurrence={{ min: 1, max: 20, default: 3 }}
      default={[
        {
          question: 'サンプル質問：ちゃろさんって？',
          answer: '<p>スコティッシュのかわいい女の子！</p>',
        },
      ]}
    >
      <TextField name="question" label="質問" default="サンプル質問" required />
      <RichTextField name="answer" label="回答（リッチテキスト）" default="<p>サンプル回答</p>" />
    </RepeatedFieldGroup>
  </ModuleFields>
);
```

### Step2：island 対応のクライアントコンポーネントを作成 📦

```jsx:islands/FAQAccordionIsland.jsx

import React, { useState } from 'react';

/**
 * アコーディオン表示のロジックを持つコンポーネント
 */
export default function FAQAccordionIsland({ faqs }) {
  const [openIndex, setOpenIndex] = useState(null);
  const toggle = (i) => setOpenIndex(openIndex === i ? null : i);

  return (
    <section className="faq-list" style={{ maxWidth: 800, margin: '0 auto' }}>
      {faqs?.map((faq, index) => (
        <div key={index} style={{ marginBottom: 16, border: '1px solid #ddd', borderRadius: 8, overflow: 'hidden' }}>
          <h3
            onClick={() => toggle(index)}
            style={{ margin: 0, padding: 12, cursor: 'pointer', backgroundColor: '#f7f7f7' }}
            aria-expanded={openIndex === index}
          >
            {faq.question}
          </h3>
          {openIndex === index && (
            <div style={{ padding: 12, borderTop: '1px solid #eee', backgroundColor: '#fff' }}>
              <div dangerouslySetInnerHTML={{ __html: faq.answer }} />
            </div>
          )}
        </div>
      ))}
    </section>
  );
}

// ポイント：モジュール本体から ?island付きでインポートすると、
// このコンポーネントがクライアント上でハイドレートされるようになります 💡 
```

#### ここで使っている仕組みは？

  `useState` で「開いている質問のインデックス（番号）」を記録してるよ   
  たとえば `openIndex = 2` なら、3つ目のQAだけ開いている状態✨


  `toggle(i)` 関数で、同じ質問をもう一度クリックしたら閉じる・別の質問をクリックしたらそっちが開くという切り替えロジックになってる


  `openIndex = i` を使って、「この質問が開いてるかどうか」を判定してるのがポイント！


  islands を使ってるのは、**この動き（状態の切り替え）はクライアント側の JavaScript で動かさないといけないから💡**

---

### Step3：Island でクライアント側にハイドレートしよう 🏝️

```jsx:index.jsx
import { Island } from '@hubspot/cms-components';
// Island コンポーネントとして切り出した部分を読み込む（?island がポイント）
import FAQAccordionIsland from '../../islands/FAQAccordionIsland.jsx?island';

/**
 * HubSpot CMS モジュール: Island を使ってクライアントコンポーネントをハイドレート
 */
export function Component({ fieldValues }) {
    return (
      <Island module={FAQAccordionIsland} faqs={fieldValues.faqs} />
    );
  }
```

---

## 注意ポイント ⚡

* **dangerouslySetInnerHTML を使うときはサニタイズに注意！**   
  [※ 詳しくはDAY07で説明しているよ](https://blog.ryamagami.com/hubspot-react_module_07#section-9)

---

## まとめ ✨

FAQをアコーディオン表示にするだけで、**見やすさ＆操作性がグッとUP！**   
HubSpot CMS × React × islands の組み合わせで、動きのあるUIがどんどん作れるよ。   
「よくある質問」や「用語集」など、応用もいろいろできるから、ぜひ活用してみてね💡

![アコーディオンFAQを指さしながらにっこり笑うちゃろさん](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/faq-charo-smile-summary.png)

---

## あわせて読みたい 📖

* [HubSpot CMS React × useState｜動かない原因とisland構成での対応【Day3】](https://blog.ryamagami.com/https/blog.ryamagami.com/hubspot-react_module_03)
* [islands × props を使って、ON/OFF制御を実装してみた【HubSpot × React DAY9】](https://blog.ryamagami.com/hubspot-react_module_09)
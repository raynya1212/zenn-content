---
title: "可愛い注意書きボックスを作ろう！HubSpotカスタムモジュールでふわもこデザイン"
emoji: "💭"
type: "tech"
topics: [HubSpot,HubSpot CMS]
published: true
published_at: 2025-04-29
---


# 可愛い注意書きボックスを作ろう！HubSpotカスタムモジュールでふわもこデザイン

2025-04-29 · Reina

## はじめに 🌿

記事の中で注意や補足を入れたいとき、テキストだけでもいいけど、   
**可愛いデザインのボックス**でまとめられたら、もっと読みやすくなるよね！

今回は、**HubSpotカスタムモジュール**を使って、   
「選ぶだけでデザインが切り替わる」ふわもこ注意ボックスを作ったので紹介するよ 💻

---

## この記事でわかること ✏️

* HubSpotカスタムモジュールで可愛いボックスを作る方法💻
* 実際に動くサンプルと、コード紹介✨
* デザインを変えたくなったときに編集するポイント🎨

---

## 結論 🎯

HubSpotのカスタムモジュールを使えば、注意書きや補足メモを**簡単に**、**統一感のある可愛いデザイン**で作れる！

（こんな感じで「メッセージタイプ」と「本文」を指定するだけで、デザインと表示できるモジュールを作った！）

![message-box](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/message-box.png)

ローカル開発（HubSpot CLI）でも、デザインマネージャー上でも、どちらでも作成できるよ✨

## やり方 🛠️

### Step1：ローカルにカスタムモジュールを作成しよう 🌟

ここではHubSpot CLIを使ったローカル開発を前提に説明するよ。    
（もちろん、デザインマネージャー上で直接作成してもOK！）

作成コマンド例：

`hs create module custom/message_box`

### Step2：fields.json を作成しよう 🗂️

モジュールフィールドは、次のように設定するよ：

```json
[{"name":"box_type","label":"メッセージタイプ","type":"choice","choices":[["note","補足（Note）"],["warning","注意（Warning）"],["tip","ヒント（Tip）"],["info","リソース紹介（Info）"],["question","質問形式（Question）"]],"default":"note","required":true}, {"name":"content","label":"本文","type":"richtext","required":true}]
```

### Step3：module.html を作成しよう 🖥️

ボックスの種類に応じて、絵文字アイコンが切り替わるようにするよ：

```html
{% set icon_map = {
    'note': '📝',
    'warning': '⚠️',
    'tip': '💡',
    'info': '🔗',
    'question': '❓'
  } %}
  
  <div class="custom-message-box custom-message-box--{{ module.box_type }}">
    <span class="custom-message-box__icon">{{ icon_map[module.box_type] }}</span>
    <div class="custom-message-box__content">
      {{ module.content }}
    </div>
  </div>
```

### Step4：module.css を作成しよう 📂

ふわっとやさしいパステルカラーに🌸：
```css
.custom-message-box {
    padding: 1.25rem 1.5rem;
    border-radius: 1.2rem;
    display: flex;
    align-items: flex-start;
    gap: 1rem;
    margin: 2rem 0;
    font-size: 1rem;
    line-height: 1.8;
    box-shadow: inset 0 1px 4px rgba(0, 0, 0, 0.04), 0 4px 12px rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
  }
  
  .custom-message-box__icon {
    font-size: 1.8rem;
    flex-shrink: 0;
    margin-top: 0.3rem;
    opacity: 0.9;
  }
  
  .custom-message-box--note {
    background: linear-gradient(to bottom right, #eaf6ff, #f5fbff);
    border-left: 5px solid #64b5f6;
  }
  
  .custom-message-box--warning {
    background: linear-gradient(to bottom right, #fff1e6, #fff7f0);
    border-left: 5px solid #ffa726;
  }
  
  .custom-message-box--tip {
    background: linear-gradient(to bottom right, #eafaf1, #f4fff7);
    border-left: 5px solid #81c784;
  }
  
  .custom-message-box--info {
    background: linear-gradient(to bottom right, #f4f4f7, #ffffff);
    border-left: 5px solid #90a4ae;
  }
  
  .custom-message-box--question {
    background: linear-gradient(to bottom right, #f6f0fa, #fdfaff);
    border-left: 5px solid #ba68c8;
  }
  
  .custom-message-box__content p {
    margin: 0.2rem 0;
  }
  
```


### Step5：meta.json を作成しよう 📝

モジュールメタ情報も忘れずに設定しておこう：

```json
{"global":false,"content_types":["LANDING_PAGE","SITE_PAGE","BLOG_LISTING","BLOG_POST"],"host_template_types":["PAGE","BLOG_LISTING","BLOG_POST"],"is_available_for_new_content":true}
```

ポイント：「is_available_for_new_content」がtrueだと、新しいページ作成時にも使いやすいよ！

"content_types" にモジュールを使いたいページタイプを指定していないと、ページエディターでモジュールが表示されないので注意！

## 注意ポイント ⚡

* `choices`は**配列の配列形式**で書かないとエラーになる
* fields.json全体は**配列 `[]` から始める**

ファイルをアップロードする際に、エラーが出たら、まずJSON形式をしっかり確認しよう！

## まとめ ✨

カスタムモジュールを使えば、注意書きや補足情報も、   
**デザインを統一して見やすく整理**できるようになる！

小さな工夫だけど、記事全体のクオリティがぐっと上がるよ✏️   
これからもモジュールを活用して、どんどんブログを育てていこう🌸

![ChatGPT Image 2025年4月29日 16_03_24 (1)](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8829%E6%97%A5%2016_03_24%20(1).png)

---

## あわせて読みたい 📖

* [HubSpotでカスタムモジュールを作ってみよう](https://blog.ryamagami.com/hubspot-custom-module)
* [HubSpot CMSリファレンス｜モジュールとテーマのフィールドの概要](https://developers.hubspot.jp/docs/guides/cms/content/fields/overview)
---
title: "【HubSpot】テンプレートにブログ執筆者の情報を追加する方法 🪩"
emoji: "🪩"
type: "tech"
topics: [HubSpot, Blog]
published: true
published_at: "2025-03-22"
---

# 【HubSpot】テンプレートにブログ執筆者の情報を追加する方法 🪩

2025-03-22 · Reina

📍 完全備忘録おじさん記事です

### 結論 : 公式ドキュメントはつよつよ

HubSpot のブログ記事テンプレートに、[執筆者](https://knowledge.hubspot.com/ja/blog/create-and-manage-your-blog-authors)の情報を表示させる方法は👇にあるよ。

📘 [developers.hubspot.com / Blog templates](https://developers.hubspot.com/docs/guides/cms/content/templates/types/blog#boilerplate-markup)

具体的には👇だよ。

```html
{{ blog_author.name }}
{{ blog_author.bio }}
{{ blog_author.avatar }}
```

📘 コミュニティ記事：[ブログの記事ページに執筆者を表示させたい](https://community.hubspot.com/t5/%E8%B3%AA%E5%95%8F-%E3%83%87%E3%82%A3%E3%82%B9%E3%82%AB%E3%83%83%E3%82%B7%E3%83%A7%E3%83%B3/%E3%83%96%E3%83%AD%E3%82%B0%E3%81%AE%E8%A8%98%E4%BA%8B%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%AB%E5%9F%B7%E7%AD%86%E8%80%85%E3%82%92%E8%A1%A8%E7%A4%BA%E3%81%95%E3%81%9B%E3%81%9F%E3%81%84/td-p/961038)

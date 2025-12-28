---
title: "AI-900 対策ノート：分類モデルの評価指標まとめ"
emoji: "📊"
type: "tech"
topics: [AI-900, Machine Learning, Classification, Azure, AI]
published: true
published_at: 2025-04-13
---

# ✨AI-900対策ノート：分類モデルの評価指標まとめ 📊

2025-04-13 · Reina  
※ 試験対策用のメモ ✍️

## 1️⃣ 混同行列（Confusion Matrix）
モデルが「何を当てて何を間違えたか」を表にしたもの。

TP / FP / FN / TN の関係を理解するのが大事。

## 2️⃣ 精度（Accuracy）
全体のうちどれくらい当たったか。
- 不均衡データでは注意 ⚠️

## 3️⃣ 適合率（Precision）
陽性と予測した中で正解だった割合。
- 誤報を減らしたい場合に重要（例：スパム判定）

## 4️⃣ 再現率（Recall）
実際の陽性のうち、正しく見つけられた割合。
- 見逃しが許されない場合に重要（例：病気の診断）

## 5️⃣ F値（F1-score）
Precision と Recall のバランスをとった指標。

## 🎯 使い分け早見表
| シーン | 重視する指標 | 理由 |
|--------|--------------|------|
| 病気の検査 | Recall | 見逃し厳禁 |
| スパムメール判定 | Precision | 誤検出を減らすため |
| 全体性能ざっくり | Accuracy | 全体の当たり具合 |
| 不均衡データ | F1-score | バランス良い測定 |

![ChatGPT Image](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8813%E6%97%A5%2021_40_25.png)

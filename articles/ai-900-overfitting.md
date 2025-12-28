---
title: "AI-900 対策ノート：過学習（Overfitting）とは？"
emoji: "🎯"
type: "tech"
topics: [AI-900, Machine Learning, Overfitting, Azure, AI]
published: true
published_at: 2025-04-13
---

# ✨AI-900対策ノート：過学習（Overfitting）とは？

2025-04-13 · Reina  
※ 試験対策用のメモ ✍️

過学習とは、**訓練データに対しては高精度なのに、未知データには弱い状態**のこと。

## 🎯 過学習とは？
- モデルが「一般的な傾向」ではなく「ノイズや例外」にまで過度に適応してしまう状態。

### 📌 例：
猫の画像分類で、**背景のソファの色**まで覚えてしまう → 背景が変わるとうまく分類できない。

## 🛡 過学習を防ぐ方法
| 対策方法 | 説明 |
|----------|------|
| ① データ量を増やす | 多様なデータで学習し一般化能力を向上させる |
| ② シンプルなモデルを使う | パラメータが多すぎると過学習しやすい |
| ③ 正則化（Regularization） | L1 / L2 などのペナルティを付けて極端な重みを防ぐ |
| ④ 交差検証（Cross-validation） | 複数パターンで検証して過学習を検知 |

## ✅ 試験に出やすいポイント
- 「訓練データにだけ合っている」のは過学習
- 正則化は頻出キーワード
- 逆は未学習（Underfitting）

![ChatGPT Image](https://23823306.fs1.hubspotusercontent-na1.net/hubfs/23823306/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8813%E6%97%A5%2022_02_30.png)

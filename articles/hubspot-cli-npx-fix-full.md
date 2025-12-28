---
title: "【HubSpot】HubSpot CLIのhsコマンドが使えない？npxで解決できた話 🐧✨"
emoji: "🛠️"
type: "tech"
topics: [HubSpot,HubSpot CLI, Node.js]
published: true
published_at: 2025-04-18
---

# 【HubSpot】HubSpot CLIのhsコマンドが使えない？npxで解決できた話🐧✨

2025-04-18 · Reina

![ChatGPT Image](https://blog.ryamagami.com/hubfs/AI-Generated%20Media/Images/ChatGPT%20Image%202025%E5%B9%B44%E6%9C%8818%E6%97%A5%2023_33_35.png)


### 🪨 背景：インストールできたのにが使えない！

```bash
npm install @hubspot/cli
```

UbuntuでHubSpot CLIを使いたくて、公式手順通りにでインストールを実行💡
ところが…

```bash
hs init
```

と打ってみても「コマンドが見つかりません」と言われてしまい、まったく動いてくれない。。（泣）
（これでは泣いてしまう）

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/npx-01.png?width=1094&height=136&name=npx-01.png)


```bash
npm list -g @hubspot/cli
```

を見ても、グローバルには何もインストールされていない様子🤔

### 🔎 原因：ローカルインストール + パスが通っていない
今回のケースでは、 `-g` オプションをつけなかったため、CLIはプロジェクトディレクトリ内にローカルインストールされていました。
その結果、`hs` コマンドは`node_modules/.bin/hs` に配置されていて、グローバルなパスからは見えない状態になっていたのです👀

つまり：
- インストール自体は成功している（でもローカルにしかいない）
- ターミナルから と打っても見つからない（PATHにない）

### ✨ 解決方法： `npx`を使えばそのまま実行できる！

そんなときに活躍してくれたのが、Node.jsの標準コマンドである `npx`です🌈

```bash
npx hs init
```

 を実行すると、 がプロジェクト内の  にある を自動で見つけて、実行してくれるんです！🎉
まるでおまじないみたいな便利さ…✨
（成功したー！✨）

![](https://blog.ryamagami.com/hs-fs/hubfs/MeowBit.dev/Blog%20images/npx-02.png?width=1494&height=282&name=npx-02.png)



### 🙋‍♀️ `npx` って何をしてるの？

簡単に言うと、`npx` はこういうことをしてくれるコマンドです👇

- パッケージのコマンドを探す → の中をチェック
- 一時的にパスを通す → 今だけ使えるようにする
- 必要ならその場でインストール（※） → パッケージがない場合は自動で入れてくれる場合も

※ ただし今回のように既にインストール済みのローカルCLIを使うケースがメインです！

### 🧁 まとめ

UbuntuでHubSpot CLIを使おうとしたときに  が動かず焦ったけど、
実はローカルにインストールされていたため、パスの問題で見えなくなっていただけでした。

でも `npx hs` と実行するだけで、ローカルのCLIをそのまま呼び出すことができて無事解決！🎉

#### ✨ ポイントまとめ：
- `npm install` `-g`（なし）はローカルインストールになる
- `hs` コマンドが見つからないときは `npx hs` で呼び出してみよう
- `npx` はパスを通すのが面倒なときも、とっても便利！

# Zenn CLI

* [📘 How to use](https://zenn.dev/zenn/articles/zenn-cli-guide)# zenn-content

## 記事の作成
下記コマンドで記事作成ができる。

```
npx zenn new:article
```

### 冒頭部分のformat
```
---
title: "" # 記事のタイトル
emoji: "😸" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: [] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
ここから本文を書く
```

### preview

```
$ npx zenn preview # プレビュー開始
```

### 公開するにはpushするだけ

# Man is to Computer Programmer as Woman is to Homeworker? Debiasing Word Embeddings

---

## 資料

- https://arxiv.org/pdf/1607.06520.pdf

## 目次

- 動機（目的、課題）
- 手法
- 評価（よしあし）

## 動機

機械学習に含まれる偏見を取り除きたい。

- 機械学習は、知らぬ間にデータの偏見を増大させてしまうリスクを抱えている。
- 例えば、word embedding は文章をベクトル化するツールだが、Google News の記事を学習させるとき、文章を取り込むので、男女の偏見の記事を学習すると、その偏見を取り込んでしまう。
- さらに、偏見を含んだ embeddingを使用すると、その偏見が拡散＆増幅することがあるので、不安になる。

## 手法

性の偏見を取り除く手法、並びに関連性は保つ方法（女性→女王など）を提案する。幾何学的に２つのことが言える

1. word embeddingでは偏見情報のベクトルの方向を捉えることができる。
2. 性に関するワード（neutral word）は線形分離可能である。

```bash
## どうやって捉えるのか？
```

## 評価

提案手法の評価

- クラウドワーカーによる評価→標準的な評価検証を実施（ex読む。）
- 大幅に性の偏見を削減できた。
- クラスタの関連性を保つための効果的なプロパティも担保できた。結果。性偏見の増加もなくすことができた。

## 手法ほりさげ

### 手法について

- 専門的な用語での説明が多いので、それを前にはなす。

embeddingされた偏見の確認方法と、その数値化（定量化）について。
確認する方法

- identifying the gender subspace

### 定量化する方法

- direct bias
- indirect bias

debiasing algorithm について２つステップから構成されている。

1. 部分空間からベクトルを捉える方法。
    1. identify gender subspace
2. 削減する方法
    1. neutralize and equalize
    2. soften

3. 偏見的な単語について
    1.determining gender neutral word

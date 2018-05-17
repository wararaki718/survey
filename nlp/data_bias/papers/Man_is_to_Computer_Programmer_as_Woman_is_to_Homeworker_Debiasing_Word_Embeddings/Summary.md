# Man is to Computer Programmer as Woman is to Homeworker? Debiasing Word Embeddings

論文要約しましょう。

---

## Summary

学習済みの機械学習モデル（Word2Vec）に含まれるバイアス成分を捉えて削減する手法の提案。

→ abstractをまとめる。

---

## Preliminary

- gender neutoral words
  - 性差別に関連する単語の辞書
- cos類似度の紹介

### Embedding

- w2vNEWS embedding（https://code.google.com/archive/p/word2vec/）
- d=300
- mikrovさんの2013年の論文。(あとで確認する。)
- pre-trainingにgoogle news corpus（normalizeもしてるよ）
  - 50,000語以上から、フィルターで取り除いて、26,377語残す
  - 以下の条件は取り除く
    - 出現回数が20回より下。
    - upper-case
    - 数字
    - 句読点
- 性の偏見のデータセットも与える。（gender stereotypes are also present in other embedding data-sets）

### Crowd Experiments

- 一般人による評価をする。
  - クラウドソーシングで、一般の人（アメリカ人）に依頼。
  - Amazon Mechanical Turk
- 2typeの実験（なぜそのタスクで評価すると論文の主張が正当化させるのか？論文の主張がなにか？）
  - 単語の評価。embeddingに偏見の単語が含まれているかどうか。 
  - 類推した単語の評価。
  - これらから、ratingから性能をみる。情報集約して、precisionとrecallで評価。
- 10人で多数決。半分以上でアノテーション。

あとは、文化や人種も考慮して、不快に思わないバイアスを用意している。

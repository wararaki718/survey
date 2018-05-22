# Man is to Computer Programmer as Woman is to Homeworker? Debiasing Word Embeddings

論文要約しましょう。

---

## Summary

学習済みの機械学習モデル（Word2Vec）に含まれるバイアス成分を捉えて削減する手法の提案。

→ abstractをまとめる。

---

## 3.Preliminary

- 使用しているembeddingについての説明
  - one-hot vectorを入れる。
  - gender neutoral wordsの定義
    - 性差別に関連する単語の辞書（flight attendant, shoesなど）
  - she-heのペアのデータを作って、これを評価する。
    - 作り方は7章参照
- 類似する単語を拾ってくるのに、cos類似度を使用。

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
- 一般性は失ってないぜ。（gender stereotypes are also present in other embedding data-sets）

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

## 4.Gender stereotypes in word embeddings

仮説

- w2vにあるバイアスを理解する
- 人の意見と数値バイアスがどれくらい近いのかを調べる

２つの方法で評価できる。

- embeddingが持つ、occupation wordsの偏見を評価する。
- embeddingが出したアナロジーに対して人が偏見かどうか判断する。

探索的な分析をして次のmetricsのモチベーションにつなげる

Figure.1 Glove使ったときのshe-heのペア一覧。</br>
Figure.2 類推で出て来る結果と、debiasingした結果</br>
Figure.3 間接バイアスの例</br>
Figure.4 どのembeddingでも同じような傾向が出る。</br>

### Occupational stereotype

Figure.1のような一覧をcrowdworkerで評価する。

### Analogies exhibiting stereotypes

### Indirect gender bias

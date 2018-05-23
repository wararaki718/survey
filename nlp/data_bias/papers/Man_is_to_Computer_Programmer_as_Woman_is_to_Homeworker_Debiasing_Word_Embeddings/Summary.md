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

職業のstereotypeについて、定義とその説明を行う。

男性と女性でword2vecで近しい職業をリスト化する。
そのリストをcrowdworkerに男性よりか、女性よりかそれともニュートラルか評価してもらう。10人のcrowdworkerによってgender stereotypeを評価する。評価について、0-10の幅でレートを付ける。
男女軸で職業ワードを見積もると、Spearman係数0.5で、強い相関が見られた。
このことから、w2vの幾何学的バイアスは、一般群衆のgender stereotypeも調整されているといえる。そこで、相関があるこの職業単語を使うことにする、なぜなら人が簡単に解釈できて、共通のgender stereotypeを捉えることができるから。
また、stereotypeではない他の単語もタスクでつかう。気づいてほしいのは、たとえば womanとmanのような性別のペアが使用できる。sheとheを選んだのは、よく出て来る単語で、語義（単語の意味）の変化があまりないから選んだ。manとwomanも同じ理由で使うことができる。

職業ごとの上のshe-heの向きを、Word2VecのembeddingとGloVeのembeddingで見積もってみた。結果、高い相関が見られたので、embeddingが変わっても偏見を持つし、古典的なコーパス訓練やword2vecに限った話ではないといえる。

Glove
- J. Pennington, R. Socher, and C. D. Manning. Glove: Global vectors for word representation. In EMNLP, 2014.

### Analogies exhibiting stereotypes

### Indirect gender bias

# Learning, Prediction and Optimisation in RTB Display Advertising

ADテクのチュートリアル</br>
上海交通大学の先生がまとめたもの。

## 資料

---

元スライドと参考資料

- http://wnzhang.net/slides/cikm-16-rtb-full.pdf
- https://arxiv.org/pdf/1610.03013.pdf
  - 教科書

## 目次

---

- RTB system
- Auction mechanisms
- User response estimation
  - CTR推定についてはここにかかれている。
- Learning to bid
- Conversion attribution
- Pacing control
- Targeting and audience expansion
- reverse price optimization

## Real-Time Bidding(RTB) system

---

広告の説明とRTB systemの紹介。
広告は経済的制約のなかで企業とユーザーをベストマッチさせるもの。</br>
従来の広告は無駄が多い（町の看板など）。</br>
インターネットユーザーと広告主をベストマッチさせるアルゴリズムの設計する。

仕組みの例：

### Sponsored Search

- Google検索などで出てくる広告
- 広告主はキーワードごとに課金する。
- ユーザーがキーワードを検索したとき、adランキングのオークションを開催する。
- リスティング広告

### Display Advertising

- ユーザーの情報に対して課金する
- ユーザーと広告を仲介する仕組み。

### Internet Advertising Frontier (RTB based Display Avertising)

RTBとは

- オンラインで広告が閲覧されるたびに、評価・売買がそれぞれ＆即座に実施される。
- キーワードの購入や広告を束ねる代わりに、広告主が直接ユーザーを買う仕組みのこと。
- 閲覧したサイト情報を元に、別のサイトへ広告を掲載される。
  - 例えば、ロンドンのホテルのサイトを見た後に、facebookに行くとロンドンのホテル関連の広告が表示される。
- 個人レベルでリアルタイムに広告の売買と反映が行われる。

システムのフロー

![RTB_System](img/RBT_mechanism.png)

## Auction mechanisms

---

オークションの仕組み

- オークションの仕組み
  - 表示枠に対して掲載する広告を決めるための仕組みを説明。
    - 価格とprivate valueの掛け合わせで評価する。（GoogleAdWordsは、private valueにSQが設定されている。）
  - どうやって釣り合いを保つか。掛け金を最適化するか。ランキングをどのように決めるか？

## User Response Estimation

---

CTR推定の話。</br>
CTR予測で使用するアルゴリズムの説明が中心になっている。

以下の条件のときに機械学習によるCTR推定をやると有効。

- ２つの特徴空間が大きい。(>10 millions)
  - Bloom filter を使えば新しい特徴が検知可能。 (5>instances)
- データの発生数が多い。（>10 millons daily）
- クリックされた/されないのラベルがアンバランスであるとき。
  - 殆どの場合、click / non-click = 0.3%ぐらい
  - negative down samplingを実施して、トレーニング時にデータの偏りをなくす。
  - calibration
    - ラベルの推定を確率の推定にマッピングする。

### 線形モデル

線形モデルで紹介するものは以下の通り。

- Logistic Regression
  - with SGD learning
  - Sparse splution（Logistic Regression with FTRL）
- Online Bayesian Probit Regression

### 線形モデルの良し悪し

- 良い
  - 効果的＆拡張できる
  - 大きな特徴やデータを調査できる
- 悪い
  - 上位の組み合わせの特徴を定義しない限り、特徴の相互作用を捉えることができない。
  - モデルに限界がある。（特徴が独立した仮説になっている。）

### 非線形モデル

以下の非線形モデルを紹介する。

- Factorisaztion Machines
  - Field-aware Factorisation Machines
- Gradient Boosting Decision Trees
- Combined Models
  - GBDT+LR
  - GBDT+FM
  - Factorization Machine + Neural Network
- Deep Neural Network
  - Product-based Neural Networks (PNN)
  - Convolutional Click Prediction Model
- Unbias Learning

実験結果を見ると、PNNのパフォーマンスが良い。

## Learning to bid

---

入札に関する処理について紹介。

## その他

---

### 用語の説明

AD関連用語

- http://please-sleep.cou929.nu/listing-adnetwork-auction-logic.html

ブルームフィルタ（blume filter）

- 特殊なデータ構造（あとで調べる。）
- http://haslab.uminho.pt/cbm/publications/scalable-bloom-filters

キャリブレーション

- スコアを適切にスケーリングして、確率値に落とし込む手法
- 各クラスに所属する確率を推定したい場合に使う。（異常検知など、異常クラスの確率で評価するときに、確率値で最適化しておきたい。）
- http://scikit-learn.org/stable/modules/calibration.html

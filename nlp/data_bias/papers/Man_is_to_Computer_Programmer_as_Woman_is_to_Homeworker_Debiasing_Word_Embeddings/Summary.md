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

Occupational stereotypeを決めに行く話。

男性と女性でword2vecで近しい職業をリスト化したものを使う。

- そのリストをcrowdworkerに男性よりか、女性よりか、それともニュートラルか評価してもらう。
- 10人のcrowdworkerによってgender stereotypeを評価する。
  - 評価について、0-10の幅でレートを付ける。
  - 男女軸で職業ワードを見積もると、Spearman係数0.5で、強い相関が見られた。
    - このことから、w2vの幾何学的バイアスは、一般群衆のgender stereotypeも調整されているといえる。
    - そこで、相関があるこの職業単語を使うことにする、なぜなら人が簡単に解釈できて、共通のgender stereotypeを捉えることができるから。
- また、stereotypeではない他の単語もタスクでつかう。
  - 気づいてほしいのは、たとえば womanとmanのような性別のペアが使用できる。
  - sheとheを選んだのは、よく出て来る単語で、語義（単語の意味）の変化があまりないから選んだ。manとwomanも同じ理由で使うことができる。

職業ごとの上のshe-heの向きを、Word2VecのembeddingとGloVeのembeddingで見積もってみた。結果、高い相関が見られたので、embeddingが変わっても偏見を持つし、古典的なコーパス訓練やword2vecに限った話ではないといえる。

Glove

- J. Pennington, R. Socher, and C. D. Manning. Glove: Global vectors for word representation. In EMNLP, 2014.

### Analogies exhibiting stereotypes

- 類推は、word embeddingの質とその偏見の評価をするのに役立つ。
- embeddingで類推を生成する方法と、その類推を使って偏見の規模を測る方法について話す。　（細かな話は appendinx Aを参照）
- 単語のペアを１組与えて、そのペアと対応する単語セットを生成してもらう。
  - 例えば、he-sheが与えられたら、heに対応する単語xとsheに対応する単語yを生成して評価する。
  - 一般的に、例えば、he-sheとking-(?)が与えられたとき、(?)の単語を推定してもらう仕組みがあるが、ここでは上記の例のように修正した。
- もととなる単語のペア（a, b）と生成された単語のペア（x, y）があるとき、以下の以下の計算式でスコアリングする。
  - コサイン類似度

![analogies_exhibiting_stereotype_scoring](img/analogies_exhibiting_stereotype_scoring.png)

- δ : 類似度の閾値（２つの単語の距離の閾値）
- 直感的なスコアリングの指標として、２つのペア同士の向きを近さを見る。
  - 意味的に明確にするために、２つの単語が離れすぎないようにする。
- 閾値 δ について
  - 全実験で、δ=1をよく使った。
  - 全embeddingが正規化されると、閾値は angle<=π/3 に相当する。これはオリジナルのペアよりも近いと意味する。
  - 実際にランダムな2つのembeddingのベクトルよりも、類推で構成された単語ペアのほうが著しく近似しているといえる。
- embeddingと種となる単語が与えられたとき、トップスコアの類推ペアを出力する。
  - 同じ単語が出た場合、余分なものとして削除する。

類推の判定結果について

- stereotypeやbiasは文化から大きく影響される。
  - 今回の実験で雇ったUSのcrowdworkerの類推の評価に基づくと。
- 各類推について、2つのyes/no questionを出題した。
  - 出力された単語のペアが直感的かどうか？
  - gender stereotypeを反映しているか？
- 各類推の判定は１０人のcrowdworkerが行う。
  - 判定の人数がそのままrateとして反映する。
- 全体的に、150の類推のうち72のペアが、性別的にふさわしいと判定された。(5人以上の評価がある。)
  - 図（リスト）の説明
    - 生成された偏見性が見られる類推の評価を Figure.2のトップに
    - 直感的に作成した性別的にふさわしい類推の評価を Figure.2の下部に置く。
    - リスト全体の類推とレーティングについては、Appendix Gで詳しく紹介。

### Indirect gender bias

- 直接的なbiasを分析すると、性別の特性を持つ単語とgender neutral wordで類似性がはっきりと現れる。
- gender biasも gender neutral words との間で幾何学的にも影響がある。
- indirect gender biasのテストについて
  - gender neutral wordsでペアを作る。（例えば、softballとfootball）
  - 全職業ワードをgender neutral wordsで作成したペアの向きに反映させる。
  - 反映させたワードから、近しい単語を見つける。（Fig.3参照）

例

- bookkeeperとreceptionistは、footballよりもsoftballに近しい。
- 近しいことから、間接的に bookkeeper, receptionist, softballは女性から連想される。
  - ここで大切なことは、多くのペア（男性的or女性的偏見を含む）は論理的に性を連想させるような処理はしていないことに注目してほしい。
  - たとえば、footballとfootballerは男性のバイアスが強いが、生徒は別の要因で類似性を正当化する。(section5で、計測方法とgender biasへの間接的な効果について詳しく説明する。)

## 5.Geometry of Gender and Bias

- embeddingの幾何学上にあるバイアスを研究。
  - genderの向きを確認する。
  - 整えられた集団のバイアスの範囲内で、バイアスをそれぞれ数量的に表現する。
- 「direct bias」と「indirect bias」の測定方法を、前章よりもはっきりと観察して数値化する。

### 5.1 Identifying the gender subspace

- 言語の使用は、複雑で扱いづらい ＆ 独立した個々の単語ペアは、常に推定通りの振る舞いをしない。
  - 例えば、manという単語はいくつか異なる使われ方をする。
    - 「！」マークをつけた表現で「oh man!」のような使われかた。（oh man! => なんてこったい）
    - 性別として使われる man
    - man the stationのような同士として使う。（man the station => 部署を受け持つ）
- 偏見の推定をより強固にするために、複数のペア同士を比較して集計する。

- she-heやwoman-manのような、いくつかのdirectionsを組み合わせて,gender direction（g ∈ R^d）を明確にする。
  - このdirectionは、direct biasとindirect biasの単語と関連性を数量化するのに役立つ。

- 英語について２つ言える
  - genderのペアとなる言葉の数が多くある
  - embeddingごとに違いがあることを考える。
- データを見る前に、大まかな同ベクトルの違いについて、次式でイメージしてもらう。

![fomula](img/formula.png)

- 実際には、genderのペアの違いに類似点はない。いくつかの理由がある。
  - 異なるgenderペアに関連した異なるバイアスがある。
  - 多義性について言える。この場合（上記の式の場合）、grandfatherが法令関係で使用されていることが要因になる。
  - 単語数のランダム性で、違いの原因になっている。（単語の偏りかな。）

- 単語に対応するベクトルの対がgenderの群衆概念と一致することを実験的に検証した。
  - クラウドワーカーに２つの単語リストを作るように依頼した
    - gender化（すでに性別に紐付いている）単語の一覧を作成。（例えば、waitress, menswear）
    - 性のstereotypeと捉えてしまうような単語一覧を作成（例えば、sewing, football）
  - 作成した各リストから、分類タスクのために、最も頻繁に出現した単語（男性50単語、女性50単語）を使う。
    - 各候補のペア（例えば、she-he）に対して、クラウドワーカーが女性と定義した（偏見）単語を正確に分類するのは、word vectorがheよりもsheが近くにある場合とする。
  - Table.5は、各性別のペアに対する定義＆偏見ワードの分類精度になる。
    - 精度は高く、これらのペアがgenderの直感的な意味を捉えていると意味する。

Table.5
![table.5](img/table.5.png)

- genderの部分空間を特定するために、10個のgenderペアの差分ベクトルを取り、その主成分を計算する。
- Figure.6では、これらのベクトルの分散の大部分を説明する単一の方向がある。

Figure.6
![figure.6](img/Figure.6.png)

- 最初の固有値は残りの固有値よりもかなり大きい。
  - 注意してほしいのは、ノイズのあるベクトル10個の有限サンプルのランダム性から、固有値が減少することが予想される。
- しかしFig.6でも示されているのは、無作為なサンプリングで観察された減少は、より緩やか＆均一である。
- よって、単一ベクトルgで示されたトップの主成分は、genderの部分空間として捉えることができると仮定する。
- 一般的に、gender部分空間は高次元＆一般的な部分空間で分析とアルゴリズムが機能することができる。

### 5.2 Direct bias

- direct biasを計測するために、最初に問題の妥当性に対するgender-neutral wordを明らかにする。
  - gender-neutral wordの生成は Section7で詳しく説明する。
- gender-biasの定義を以下数式で表現できる

![gender_bias](img/gender_bias.png)

- 数式の記号について
  - 与えられた gender-neutral wordを **N** とする
  - 上記で学んだ gender-direction を **g** とする
  - **c** は、どれくらい厳密かを決めるためのパラメータ
    - もし **c** = 0 である時、|cos(w-g)|^c = 0 なので、**w** と **g** が重複しない場合 0 となる。そうでないときは 1 になる。
- このような厳密なバイアスの測定は、紹介からの大学入学の例のように、ここではembeddingに対して候補に対する性別以外の若干の好みを導入することに容認できない望ましい設定がある。
- **c** = 1でより緩やかなbiasになる。
  - その提案は、我々がシンプルなものを好んで選ぶように、頻度によって単語の重みの定義を拡張することは自然なことだと思う。
  - 例えば、w2vNEWSで、**N** に327職種を設定したとき、**DirectBias1**=0.08となり、多くの職種語がgender directionの実質的な成分を持つことが確認できた。

### 5.3 Indirect bias

- 不幸にも、上記の定義だけではindirect biasを捉えることはできない。
  - これを見るには、gender-pairsとなる単語をembeddingから完全に削除することを想像する。
    - 髭や子宮といった単語はgenderの特徴となるが、ペアにはならない。
  - 受付のようなgender neutralな単語は、サッカーよりもソフトボールが近いという、indirect genderな関係性がある。（see Figure.3）

![figure.3](img/figure.3.png)

- introductionで議論したように、類似性がgenderに起因するという真実の根拠を得ることは、微妙である可能性がある。

- 識別された gender subspace **g** は、任意の単語のペアの類似度が　**g** への貢献を定量化することを許可する。
- 与えられた単語ベクトル **w ∈ R_d** を **w_g + w⊥** として分解できる。
  - ここで、**w_g = (w・g)g** としたとき、貢献度を **w⊥ = w - w_g** とする。
    - 注意してほしいのが、全word vectorは単位の長さに正規化されている（単位の長さとは?）
  - gender components を類似したも word vector **w** と **v** で定義すると、以下の式で表現できる。

![word_vector_similarity](img/word_vector.png)

- 数式について
  - **(w⊥ v⊥)/(|w⊥|_22 |v⊥|_2)** は２つのベクトルの内積。
    - これは、gender subspaceから突き出たベクトルを、再正規化する。
    - この計測法は、gender 部分空間を除去する操作のために、内積がどれくらい（元の内積値のいち部として）変化するかを定量化する。
    - データにノイズがあるため、各ベクトルは 0 ではない要素 **w⊥** を持つ ＆ **β** は定義される。
    - **β(w, v)=0**のとき、よく似た単語が理由で 0 になっている。（性別が理由ではない。）
    - もし、**w_g = 0 = v_g** は **β(w,v)=0**になり。**w⊥ = 0 = v⊥** は **β(w,v)=1** になる。

- Figure.3 の事例では、最も極端な単語でfootballとsoftballの向きを調べた。
  - ５つの極端な単語（高ポジティブまたは低ネガティブを反映した football - softballに関連する単語。）をテーブルに表示した。
  - 受付やウエイトレス、専業主婦などの単語はfootballよりもsoftballのほうが近い。
  - **β** の数値では、これらの単語とsoftballは実質、67%、35%、38%だった。
  - これは、これらの言葉の softball の embedding 上の見せかけの類似性は、embeddingにおける gender bias によって大きく説明できることを示唆している。
  - businessmanとmaestroはfootballに近いが、これもindirect biasによるものが大きく、**β** の数値はそれぞれ 31% と 42% だった。

![figure.3](img/figure.3.png)

## 6.Debiasing algorithms

- debiasing algorithm は、単語のペアではなく単語の集合で定義されるので、人種や宗教的な bias のような他の bias も考慮できる。
- 中和できる単語の集合を持っていると仮定できる。
  - 単語は、リストや第７章で説明する embedding からくるものとする。
  - 多くの場合、この集合をより小さくするように、中和できない gender 固有の単語をリスト化するほうが簡単。

- ２つのステップで debiasingをおこなう。
  - 1st step
    - gender subspace を認識する。
    - これは、embedding の向きを確認して bias を捉える。
  - 2nd step
    - **Neutralize and Equalize** or **Softn** の２つのオプションを定義する。
    - **Neutralize**
      - gender subspace 内で 0 となる gender neutral word を確保する。
    - **Equalize**
      - 部分集合の外の集合と完全に等しくする。
      - 等距離のneutral wordの属性に対して、各同一の集合の全単語と強制的に同一にする。
        - 例えば、{grandmother, grandfather}と{guy, gal}の２つの同一の集合があるとき、babysitに同一化された後、{grandmother、grandfather}と{guy, gal}は同距離のままになっている。しかし、おそらく galやguyよりもgrandparentsのほうが近くなっている。
        - これは、そのようなペアが neutral word を配慮して、いろんな bias を表示したくないアプリケーションに適している。
      - Equalizeの不利益な点は、アプリケーションに役立つような確実な区別を削除してしまうこと
        - 例えば、高確率でgrandmotherよりもgrandfatherの表現が割り当てられた言語モデルを望むようであれば、これは grandfather が grandmother と平等の意味を持っていないので、この区別を取り除く。
    - **Soften**
      - Softenアルゴリズムは、できるだけ元のembeddingの類似性を保ちながら、これらの集合の差を削減する
      - これを制御するパラメータとトレードオフの関係である。
  - アルゴリズムを定義する。いくつかの表記法を導入すると便利。
    - subspace **B** は、k個の直行の単一ベクトルで **B={b_1,...,b_k}⊂R^d** で定義される。
    - k = 1 のとき、部分空間は単純な向きになる。
    - ベクトル **v** を部分空間 **B** に投影するとき、以下の式になる。

![simple_direction](img/simple_direction_subspace.png)

- これは、**v-v_B**が直行部分空間上に投影されていることを意味する。

### Step 1: Identify gender subspace

- gender subspace を明らかにする。
- 各集合の平均を以下の式で求める。
  - word set: **W**
  - defining sets: **D_1, D_2,...D_n ⊂ W**
    - embedding **\{w ∈ R^d\}_w∈W** と同様。
  - 整数値 **k >= 1**

![mean](img/mean.png)

- bias subspace を **B** としたとき、SVD(C)の最初の **k** 番目の行を以下のようにする

![svd_calc](img/svd_calc.png)

### Step 2a: Hard de-biasing (neutralize and equalize)

- neutralizeと同一化を行う。
- additional neutralize word set: **N ⊆ W**
- family of equality sets: **ε = {E_1,E_2,...,E_m}**
  - 各 E_i は **W** に属する。
- 各 **w ∈ N** に対して、**w** を再 embedding すると

![re_embedding](img/re_embedding.png)

- 各集合 **E ∈ ε** は以下の通り、

![set_e](img/re_embedding2.png)

- 最終的に、部分空間 **B** と新しい embedding **\{w ∈ R^d\}** が出力される。

- **B** の外にある単語の集合は、単純な平均 **v** と単一の長さによるベクトルの調整で、等しく同一化する。
  - ２つの因子 **w_B** と **w⊥B=w-w_B** に分けることを想像すると、簡単に理解できる。
  - 後者の **w⊥B** は、平均ですべて同一化する。
- 私達がなぜ中心にいるかを動機づけるために、機械学習では一般的だという事実を越えて、バイアスの方向 (k=1)とE={male, female}のような性別のペアを考える。
  - 議論したように、両方の単語は、性の方向では positive(female)であるものは、femaleが大きな影響を持っていることが起因している。
  - このようなことについて、例えば、男性の看護師や、男性の護衛、女性が男性のによって暴行を受けているなどのテキストが頻出などの理由から推測できる。
  - しかしながら、femaleは性の因子を強く持つので、両者を中心にした後、原点から対象的にバランスが取れている。
  - 代わりに、centering なしでベクトルの因子をバイアスの方向に拡張した場合、maleとfemaleは確実に同じ embedding を持ち、father-male や mother-female のような類推が失われる。
- soften の代替手法を定義する前に、neutralizing と equalizing で完全にペアのバイアスを取り除くことに注意したい。

#### Observation 1

### Step 2b: Soft bias correction

## 7.Determining gender neutral words

## 8.Debiasing results

## 9.Discussion

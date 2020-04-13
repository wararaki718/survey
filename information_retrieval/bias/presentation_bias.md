# Recommend における Presentation bias について

以下記事のメモ

https://www.quora.com/What-does-the-concept-of-presentation-feedback-bias-refer-to-in-the-context-of-machine-learning

## presentation bias

presentation bias (feedback bias) とは、ユーザーの検索結果やレコメンド結果の表示された情報がフィードバックに影響しているという問題のこと。

以下の実験では、異なる３種の検索エンジンを使って、２つの検索結果を並び替えたものを、ユーザーに評価してもらい、どちらの結果がよいかという実験を行った。同じ結果でも、表示順位によって良し悪しが決まることがわかった。
- https://onlinelibrary.wiley.com/doi/abs/10.1002/asi.20941

ほかにも、presentation bias のサブクラスには、position bias が存在する。position bias はアイテムの表示順位のはなし。

## 対応案・アイデア

これらの問題に対して、各企業はいろいろなアプローチをとってきた。
- 最もシンプルなものは、Machine Learning をシステムから取り除く
- presentation discount system（linkedin）
- collaborative competitive filtering: Learning recommender using context of user choice
  - https://dl.acm.org/doi/10.1145/2009916.2009959
  - アイテム間の競合をスコアリングの計算に含める。
- ランダム性を取り入れる
  - Minimally Invasive Randomization for Collecting Unbiased Preferences from Click through Logs
  - http://www.cs.cornell.edu/people/tj/publications/radlinski_joachims_06a.pdf
- multi-arm bandit algorithm を導入する
  - http://www.cs.cornell.edu/people/tj/publications/radlinski_etal_08a.pdf
  - モデル選択を行う代わりに、表示するアイテムの探索を犠牲にする。
- Thompson Sampling
  - https://www.richrelevance.com/blog/
- Negative Sampling などの設計も重要

これらを踏まえると、bandit 以外は実現できそうかな。

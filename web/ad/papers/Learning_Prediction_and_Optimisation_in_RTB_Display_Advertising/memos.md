# memo

アルゴリズムの紹介があったけど、それぞれの特徴を掴んでいない（理解していない。）</br>
各アルゴリズムの仕組みと、なぜADに最適なのか、その良し悪しを調査する。

Unbias Learningと聞きなれない用語が出てきた。</br>
偏見となる特徴を何かしらの方法で検知＆取り除き、偏見を含まない特徴で学習する手法なのかな？

## 線形モデル

- Logistic Regression
  - with SGD learning
  - Sparse splution（Logistic Regression with FTRL）
- Online Bayesian Probit Regression

## 非線形モデル

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

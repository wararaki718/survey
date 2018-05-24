# CTR予測モデルの性能評価

## 資料

http://tech-blog.fancs.com/entry/ctr-prediction-model-evaluation

## 動機・目的

CTR予測モデルを評価する方法についてのまとめ

AUC＆ROC曲線とLogarithmic Lossを紹介している。
どちらとも良い評価指標だが、良し悪しがある。そのことについてまとめている。

## AUC&ROC

yahooの論文でも使用された（ https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=0ahUKEwis2cnf_dPMAhWDraYKHb6SB9MQFggpMAE&url=http%3A%2F%2Fi.yimg.jp%2Fi%2Fdocs%2Fresearch_lab%2Farticles%2Fyutagami_deim2013.pdf&usg=AFQjCNH7pJo1MJ-1eTy1Ym1Xf-d5H6UWqg&bvm=bv.121658157,d.dGY&cad=rja ）

メリット　：テストデータの違いの影響を受けにくい</br>
デメリット：モデルのキャリブレーションが評価されない。

そもそも２値分類に使われる評価指標なので、CTRの性能評価には向かない。</br>
クリックされそうな求人と、クリックされなさそうな求人を測ることができるが、実際にクリックされる確率は測れない。

## Logarithmic Loss

kaggleのCTR予測コンペでも使われた

メリット　：モデルのキャリブレーションが評価される</br>
デメリット：テストデータによって値が大きくなる。

実際のCTRと比較するので、推定したCTRが実際のCTRが近くなる。</br>
Logarithmic Lossは、テストデータの平均CTRに影響を受けるので、テストデータによっては値が上下する。（値が変わると影響するので、キャリブレーションも考慮される。）

## モデルのキャリブレーション

予測されたCTRが実際のCTRとどれだけ一致しているのか見ること。

## まとめ

予測の代表的な評価指標として、AUC＆ROC曲線とLogarithmic Lossの２つの評価指標をまとめた。</br>
扱うデータによって、評価指標を決めることが大事。
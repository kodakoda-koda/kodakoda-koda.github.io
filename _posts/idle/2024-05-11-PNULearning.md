---
categories:
  - 雑談
date: 2024-05-11 00:00:00 +0900
math: true
tags:
  - Python
  - 半教師あり学習
title: PNU Learningの検証と考察
parse_block_html: true
published: true
---

半教師あり学習手法を調べた際に，「PNU Learning」たるものを知ったので調べてみた

# 半教師あり学習手法のPNU Learningの効果と考察

## PNU Learning

半教師あり学習とは，教師ラベルが一部にのみ付与されている場合で学習し，アノテーションコストを削減する方法．
その中の分類手法としてPNU Learningがある．

PNU Learningでは，損失関数を独自で定義し，それを用いて学習する．
損失関数$R_{PNU}$は以下のように表される．

$$
\begin{equation*}
R_{PNU}(g)=\left\{ \,
    \begin{aligned}
    (1-\eta)R_{PN}(g)+\eta R_{PU}(g) ~~ &\text{if}~~\eta\ge0 \\
    (1+\eta)R_{PN}(g)-\eta R_{NU}(g) ~~ &\text{otherwise}
    \end{aligned}
\right.
\end{equation*}
$$

ここで，$\eta$はハイパーパラメータ．

$R_{PN}$，$R_{PU}$，$R_{NU}$はそれぞれ以下のように表される．

$$
\begin{aligned}
&R_{PN}(g):=p(y=+1)R^+_P(g)+p(y=-1)R^-_N(g) \\
&R_{PU}(g):=p(y=+1)R^+_P(g)+R^-_U(g)-p(y=+1)R^-_P(g) \\
&R_{NU}(g):=p(y=-1)R^-_N(g)+R^+_U(g)-p(y=-1)R^+_N(g)
\end{aligned}
$$

$p(y=+1)$，$p(y=-1)$は訓練データにおける正例と負例をもつ割合で，$R_P^+$，$R_P^-$は正例を正例と予測した際の損失と負例と予測した際の損失である．

これで学習することによって，教師ラベルが少ない場合にもうまく学習できるらしく，**むしろ全てラベルがある場合よりも性能が良い場合もあるという**．

## 検証と考察

[PNU Learning: An Introduction to Semi-supervised Learning with Limited Labeled Data](https://medium.com/@lalf_klein/pnu-learning-an-introduction-to-semi-supervised-learning-with-limited-labeled-data-5312dcc13d52)では，全てラベルがある教師あり学習よりも，PNU Learningを用いると精度がよくなることを確認している．

でも，そんなわけなくね？ \
もしそんなことが可能ならもっとPNU Learningが話題になっているはずだし，もっと色々なところで使われているはず．

ということで検証してみた．
上の記事ではCVの分類を行っていたので，NLPの分類をやらせてみる．

### 実験
実験コードは[kodakoda-koda/PNU-Learning](https://github.com/kodakoda-koda/PNULearning)で公開している．
使用したデータは[knowledgator/events_classification_biotech](https://huggingface.co/datasets/knowledgator/events_classification_biotech/tree/main)で，その中でクラス数の多い2クラスを使用した．モデルは[google-bert/bert-base-uncased](https://huggingface.co/google-bert/bert-base-uncased)を使用．

### 結果
結果を以下の図で示す．

<img width="90%" src="/assets/img/PNULearning/output.png" alt="image_error">

上の記事では，アンラベル率が9割でも性能がよかったのに対して，手元の結果では，5割程度ならさほど劣化していないが，9割では明確な精度の劣化が見られた．
この結果だけを見ると，やはりPNULearningは万能ではないことがわかる．

ただ，9割もアンラベルしているのにこのくらい精度が出ているのはすごいと思う．ハイパーパラメータを調整したりだとかの改良をすればもっと良くないような気もする．

## まとめ
今回はPNU Learningの検証と考察を行った．お遊び程度でやったことなのできちんとした実装と検証を行なっていないので，これだけでPNU Learningがダメだと決めつけるのはよくないが，逆にPNU Learningが必ずうまくいくとも限らないことがわかった．

今後，時間があったら追加検証をしてみたいと思う．

# 参考文献
- [Semi-Supervised Classification Based on Classification from Positive and Unlabeled Data](https://arxiv.org/abs/1605.06955)
- [PNU Learning: An Introduction to Semi-supervised Learning with Limited Labeled Data](https://medium.com/@lalf_klein/pnu-learning-an-introduction-to-semi-supervised-learning-with-limited-labeled-data-5312dcc13d52)

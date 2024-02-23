---
categories:
  - ツール
date: 2024-02-23 00:00:00 +0900
math: true
tags:
  - Python
  - Pytorch
title: Pytorchのgatherの挙動について
parse_block_html: true
published: true
---

インターンで実装を行っているときに，torchのgatherの挙動で少し困ったのでまとめてみた．

# Pytorchのgatherの挙動
## サンプルコード
まずサンプルコードとその出力結果を確認する
```python:sample.py
import torch
input = torch.tensor([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])
indices = torch.tensor([
    [1, 2, 0],
    [0, 1, 2],
    [2, 0, 1]
])
result1 = torch.gather(input=input, dim=0, index=indices)
result2 = torch.gather(input=input, dim=1, index=indices)
print(result1)
print(result2)
```
これに対する出力結果が
```python:output
tensor([[4, 8, 3],
        [1, 5, 9],
        [7, 2, 6]])
tensor([[2, 3, 1],
        [4, 5, 6],
        [9, 7, 8]])
```
となる．

## 解説
gatherとは「集める」の意味で，torch.gatherは各次元にそってindexの値を集める．

dim=0のときは，dim=0にそって各indexの値をその位置に出力するようになっている．
<img width="75%" src="/assets/img/torch-gather/dim0.png" alt="image_error">

dim=1のときも同様．
<img width="75%" src="/assets/img/torch-gather/dim1.png" alt="image_error">

## おわりに
まとめたことによって理解できた気がする．
図にしてみるとなんとなくわかるが，また今度やるときには忘れてそう．
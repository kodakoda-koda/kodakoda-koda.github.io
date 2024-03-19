---
categories:
  - ツール
date: 2024-01-29 00:00:00 +0900
math: true
tags:
  - Bibtex
  - テンプレート
  - Latex
  - Notion
title: BibtexからNotionに追加できる文献管理ツールを作った（改良した）
parse_block_html: true
published: true
---

BibtexからNotion Databaseに論文を追加し，管理，出力できる文献管理ツールを作った（改良した）．

# Bib2NotionDB

[<img width="75%" src="/assets/img/Bib2NotionDB/thumbnail.png" alt="image_error">](https://github.com/kodakoda-koda/Bib2NotionDB)

インストール方法や使用法はレポジトリのReadMeに書いてあるので，ここでは機能のみを紹介する．

## Bibtexから文献情報を追加
```
bn run -f <Bibtexファイルのpath>
```
または
```
bn run -s """Bibtex文字列"""
```
を実行することで，Notion内に文献情報を追加することができる．

### 例
[Reformer: The Efficient Transformer](https://arxiv.org/abs/2001.04451)のBibtexファイルを用意して，
```
bn run -f reformer.bib
```
を実行することで，
<img width="75%" src="/assets/img/Bib2NotionDB/run.png" alt="image_error">
のようにNotion内のデータベースに追加される．

## 文献管理
追加した文献にNotion上で，
<img width="75%" src="/assets/img/Bib2NotionDB/manage.png" alt="image_error">
のようにカテゴリやステータスをつけることで，
<img width="75%" src="/assets/img/Bib2NotionDB/by.png" alt="image_error">
のようにそれぞれのカテゴリやステータスごとに確認することができる.

## Bibtexファイルを出力（改良点）
Notion上で
<img width="75%" src="/assets/img/Bib2NotionDB/cite.png" alt="image_error">
のように文献リストを追加し，
```
bn download -f <出力先> -c <文献リスト>
```
を実行すると，参考文献リストを出力することができる.

### 例
上の画像のように文献リストが付与されているとき，
```
bn download -f ~/Desktop/sample.bib -c JSAI2023
```
を実行すると，

<img width="100%" src="/assets/img/Bib2NotionDB/bibfile.png" alt="image_error">

のようにデスクトップに，JSAI2023というラベルをつけていた論文の文献リストが出力される．

## 参考リンク
このツールは[notion-scholar](https://github.com/thomashirtz/notion-scholar)というツールを改良して作成しています．

---
categories:
  - BLOG
date: 2024-01-11 00:00:00 +0900
math: true
tags:
  - Latex
title: Rebuttal Letterの日本語説明付きLatexテンプレートを作成した
parse_block_html: true
published: true
---

論文のレビューが返ってきたとき，それらの返答を送るRebuttal Letterの日本語説明付きLatexテンプレートがなかったので作った．

# Rebuttal LetterのLatexテンプレート

## プリアンブルにパッケージをインポート

```latex
\documentclass[10pt]{article}
\usepackage[a4paper]{geometry}
\usepackage[utf8]{inputenc}
\usepackage{fullpage}
\usepackage{xifthen}
```

## マクロを定義
レビュワー，指摘項目，返答を記入できるマクロを定義．

```latex
\newcommand{\reviewer}[1]{\section*{Reviewer~#1}} % レビュワー
\newenvironment{question}[1][0] % レビュワーからの指摘
   {\noindent {\textbf{Question~#1} } ---\ }
   {\par }
\newenvironment{reply} % 返答
   {\medskip \noindent \begin{sf}\textbf{Reply}:\  }
   {\medskip \end{sf}}
```

## 本文
### 謝辞
まず最初にレビュワーに対して，時間を割いてレビューしてくれたことに感謝を述べる．
```latex
\section*{Response to the reviewers} % 謝辞
We thank the reviewers for the assessment of our work. 
Given the limited space, we address the concerns that are most critical from each reviewer. 
```

### 指摘項目とそれに対する返答
どのレビュワーのどの指摘なのかがわかるように，それぞれ番号を記入する．
ここで注意すべきは，指摘項目は「そのまま」コピペすること．

```latex
\reviewer{1} % レビュワー番号を記入
\begin{question}[1] % 項目番号を記入
    \lipsum[1] % サンプルテキストを削除し，ここに指摘された部分を「そのまま」コピペする
\end{question}
\begin{reply}
    \lipsum[2] % サンプルテキストを削除し，返答を記入
\end{reply}
```

## 全体像
全体像を示すと，以下のようになる．
```latex
\documentclass[10pt]{article}
\usepackage[a4paper]{geometry}
\usepackage[utf8]{inputenc}
\usepackage{fullpage}
\usepackage{xifthen}

\usepackage{lipsum} % サンプルテキスト（削除して良い）

\newcommand{\reviewer}[1]{\section*{Reviewer~#1}} % レビュワー
\newenvironment{question}[1][0] % レビュワーからの指摘
   {\noindent {\textbf{Question~#1} } ---\ }
   {\par }
\newenvironment{reply} % 返答
   {\medskip \noindent \begin{sf}\textbf{Reply}:\  }
   {\medskip \end{sf}}

% \twocolumn % 2列で書きたい場合にはコメントアウトを外す
\begin{document}

\section*{Response to the reviewers} % 謝辞
We thank the reviewers for the assessment of our work. 
Given the limited space, we address the concerns that are most critical from each reviewer. 

\bigskip
\hrule

\reviewer{1} % レビュワー番号を記入
\begin{question}[1] % 項目番号を記入
    \lipsum[1] % ここに指摘された部分を「そのまま」コピペする
\end{question}
\begin{reply}
    \lipsum[2] % 返答を記入
\end{reply}

% 以下同様
\begin{question}[2]
    \lipsum[3]
\end{question}
\begin{reply}
    \lipsum[4]
\end{reply}

\bigskip
\hrule

\reviewer{2}
\begin{question}[1]
    \lipsum[5]
\end{question}
\begin{reply}
    \lipsum[6]
\end{reply}

\end{document}
```

これをコンパイルすれば，以下が得られる．
<p align="center">
<img width="50%" src="/assets/img/Rebuttal-Letter/Rebuttal_Letter.png" alt="image_error">
</p>

---
categories:
  - BLOG
date: 2024-07-18 00:00:00 +0900
math: true
tags:
  - Tools
  - JavaScript
  - Git
title: gitのコミットメッセージを生成AIに作成させる
parse_block_html: true
published: true
---

最近，きちんとしたコミットメッセージを書くことの重要性を感じたけど，毎回書くのが面倒だったので，AI に生成させることにした．

# コミットメッセージ自動生成ツール commitgen の改良

## 背景

個人で開発や研究を行っていると，コミットメッセージをいちいち書くのが面倒になってしまって，「fix」だけになってしまうことはよくあると思います．
しかし，誰しもいつかはチームで開発を行うことになります（多分）．
来たるその時に備えて，きちんとコミットメッセージを書くことの癖をつけておく必要があります．
ただ，実際にそれをやるのはやはり面倒．

そこで，コミットメッセージを生成 AI に考えさせて自動化できないかと思い調べてみると，いくつか既にありました．

- [WhatTheCommit](https://whatthecommit.com/)
- [auto-commit](https://github.com/m1guelpf/auto-commit/)
- [commitgen](https://github.com/shyamagu/commitgen)

WhatTheCommit は有名らしいのですが，コードを解析してコミットメッセージを考えてくれるわけではなさそうでした．対して auto-commit は，ChatGPT を用いてコードを解析して自動でコミットメッセージを考えてる，結構しっかりとしたツールです．commitgen も同様にコードの解析までやってくれますが，個人で開発されているツールなので，いくつか不安な部分があるのと機能が限定的です．

私は自分好みに機能をカスタムして使用したかったので，いろいろといじりやすそうだった commitgen を採用することにしました．

## 改良点

### バグ修正

- エラーメッセージがコミットメッセージになることを防ぐ

  commitgen では，ChatGPT の API キーが設定されていながったり，何も変更がなかった場合，「API キーが設定されていません．」や「変更はありません．」のようにメッセージを返す仕様になっていました．
  しかしこれだと，この「API キーが設定されていません．」という文がそのままコミットメッセージになってしまいます．
  そこで，そのような場合にはエラーを吐くしように変更しました．

  ```javascript
  console.log("APIキーが設定されていません．") -> throw new Error("APIキーが設定されていません．")
  ```

### 機能追加

- 対応言語に英語を追加

  デフォルトでは日本語にのみの対応だったので，英語にも対応させた．

- モデルに Gemini を追加

  commitgen では ChatGPT を使用することが想定されていましたが，ChatGPT は API 経由で使用すると有料（？）なので，現時点では無料で使用できる Gemini をモデルとして選択できるように変更．

- コミットメッセージを採用するかを選択可能にする

  生成 AI はしばしば誤りを生成するので，チェックせずに生成文をコミットメッセージに採用するのは危険．一度コミットメッセージを表示し，それを採用するか田舎を[y/n]で答える仕様にした．

## 使用方法

1. レポジトリをクローンする

   ```bash
   git clone git@github.com:kodakoda-koda/commitgen.git
   ```

2. .env ファイルを作成し，API キーを記述する

   - OpenAI を使用する場合
     ```bash
     OPENAI_API_KEY={your OpenAI API Key}
     ```
   - Azure OpenAI を使用する場合
     ```bash
     AOAI_API_KEYAOAI_API_KEY={your Azure OpenAI API Key}
     AOAI_ENDPOINT={your Azure OpenAI API Endpoint}
     AOAI_MODEL={your Azure OpenAI Service ChatGPT Model name}
     ```
   - Google AI を使用する場合

   ```bash
   GOOGLE_API_KEY={your Google AI Studio API Key}
   ```

3. 実行テスト

   正しく実行できるがテストするため，以下のコマンドを実行する．

   ```bash
   node {absolute path of the clone folder}/commitgen.js "en" "Updated test.js".
   ```

   ここで，{absolute ...}にはクローンしたフォルダへの絶対パスを入れてください．
   おそらく，「feat: Updated test.js」といった文が生成されます．エラーが発生していなければとりあえず大丈夫です．

4. git alias に登録

   以下のコマンドを実行して.gitconfig を編集します．

   ```bash
   git config --global --edit
   ```

   エディタが開いたら，一番したに以下を書き加えてください．

   ```
   [alias]
       ccg = !bash {absolute path of the clone folder}/scripts/ccg.sh {absolute path of the clone folder} {language code}
       ccgtest = !bash {absolute path of the clone folder}/scripts/ccgtest.sh {absolute path of the clone folder} {language code}
   ```

   {absolute ...}にはフォルダへの絶対パス，{language ...}には"en"か"ja"を入れてください．
   ccg は，add した変更を踏まえてコミットメッセージを生成しコミットし，ccgtest は，コミットせずにコミットメッセージを表示します．

5. 実行
   以下のように実行すれば，行った変更に対するコミットメッセージを考えてくれます．
   ```bash
   git add .
   git ccg
   ```

## 例

ある README ファイルに hogehoge という文を追加したときをやってみます．
行った変更を表示させると以下のようになる．

```bash
git add .
git diff --staged

> diff --git a/README.md b/README.md
> index fb7880f..3c70775 100644
> --- a/README.md
> +++ b/README.md
> @@ -1 +1,3 @@
> -# TITLE
> \ No newline at end of file
> +# TITLE
> +
> +hogehoge
> \ No newline at end of file
```

これに対して，git ccg を実行すると

```bash
git ccg

> feat: Added hogehoge to README.md
> Do you accept this commit message? [y/n]: y
> feat: Add hogehoge to README.md
>  1 file changed, 3 insertions(+), 1 deletion(-)
```

と yes か no で答えるように求められます．
yes を選択すると，コミットが行われます．

## まとめ

今回は，commitgen というコミットメッセージを生成 AI を使用したツールを改良しました．
初めて JavaScript を触りましたが，0 から書いたわけではないのでなんとか上手くできました．
本当はこの変更をプルリクしてマージしたりしなきゃいけないのですが，いろいろめんどそうなので，やめました．
何か文句言われたらちゃんとプルリク投げます．
今後はプロンプトエンジニアリングをしっかりしてもっと精度良くコミットメッセージを生成できるようにしたいです．

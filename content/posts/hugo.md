---
title: "hugo + Github Pagesの感想"
date: 2021-01-22T19:00:24+09:00
tags: [ "hugo", "Github Pages", "Github Actions" ]
---

このブログは「hugo + Github Pages + Github Actions」でできています。  
作り方は、以下２つみれば完璧にできます。  

* https://gohugo.io/getting-started/quick-start/  
* https://qiita.com/peaceiris/items/d401f2e5724fdcb0759d

この記事ではその感想を書きます。

## hugo

### 良いとこ
* 慣れたから簡単
* 開発環境が軽い
* テンプレート豊富
* Disqusと連携してコメントかけるところも好き

### 悪いとこ
* hugoをインストールしないといけないところ。まぁ記事増やすだけなら無しでも行けるそうではあるが。

トータル、好き。  
Amplify Consoleと組み合わせて、サクッと、コーポレートサイト作るみたいなのやってみたい。

## github.io

### 良いとこ
* 慣れれば簡単
* 無料
* 使ってるとエンジニア感を醸し出せる

### 悪いところ
* 仕様に若干の無理矢理感があり、直感的じゃない

有り難いが、あんまり好きになれない。  
リポジトリにコミットされたHTMLがそのままホスティングされる仕様なので、generateしたコードをコミットしなければならない。  
ソースコードとgenerateされたコードを分けるための苦肉の作が、`gh-pages`ブランチが公開用のブランチとして利用される仕様。

直感的じゃなくて、分かりづらい。 業務では、あんまり使わないように生きてくかも。

## Github Actions
**大好き**

以上

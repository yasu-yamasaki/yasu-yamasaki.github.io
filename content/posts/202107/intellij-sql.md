---
title: "IntelliJで、ソースコードに埋め込まれた文字列のSQLをハイライト、補完が効くようにする方法"
date: 2021-07-26T14:00:24+09:00
tags: [ "Intellij", "Tech", "Kotlin" ]
---

## 背景

jdbcTemplateの引数のSQLって、intellijで補完効きますよね。

![SQL](intellij-sql-2.png)

でも、下記のように、SQLだと認識できない文字列だと、補完は効きません。

![SQL](intellij-sql-1.png)

今回は、これをIntellijにSQLだと認識させられるようにします！


## 前提
以下を依存関係に追加  
https://mvnrepository.com/artifact/com.intellij/annotations

なくても、いけるかもしれない。

## SQLだと認識させる方法

`org.intellij.lang.annotations.Language`アノテーションを適当な場所につけて、
valueに"SQL"をいれる。

![SQL](intellij-sql-3.png)

以下のように、引数にもつけられるので、JdbcTemplateを拡張したら、ハイライトつかなくなった！みたいな場合も安心。

![SQL](intellij-sql-4.png)
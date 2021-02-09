---
title: "Interfaceの戻り値を制限したい"
date: 2021-02-09T14:00:24+09:00
tags: [ "Kotlin", "Tech" ]
---

エンジニアリングとはなにか。  
それは「曖昧さ」を減らし、「具体性」・「明確さ」を増やす行為です。  
Interfaceの戻り値を制限し、 世の中の情報エントロピーを減少させましょう！

...

## 背景

最近、私はこんなInterfaceに出会いました。

```kotlin
interface CoffeeMilkAnalyzer {
    fun getCoffeeContentRate(target: CoffeeMilk): BigDecimal
}
```

そこで私が思ったのは、 `getCoffeeContentRate` って、

* 少数？（0 <= x <= 1）
* パーセント？（0 <= x <= 100）

まぁ十中八九、前者なんですが、若干、お前のこと信じてるぞ。みたいな気分になりますよね？  
今日はそんな曖昧性と戦うため、３つの対策を紹介します。

### 対策1：コメントを入れる

```kotlin
/**
 * @return 0 <= value <= 1
 */
interface CoffeeMilkAnalyzer {
    fun getCoffeeContentRate(target: CoffeeMilk): BigDecimal
}
```
もうこれで、99.99%明確になりました。  
でも、実装者が本当にこのコメント見て実装してくれてるかは謎です。

まぁ、この周辺でバグったら、とりあえずログ出してみる？とかなりますよね。

### 対策2：DataClassを使う

```kotlin

interface CoffeeMilkAnalyzer {
    fun getCoffeeContentRate(target: CoffeeMilk): CoffeeContentRate
    
    data class CoffeeContentRate(
           val value: BigDecimal
    ) {
        init {
            require(value >= BigDecimal.ZERO)
            require(value <= BigDecimal.ONE)
        }
    }
}
```

完璧に制限できてます！  
使う側の実装が若干汚くなっちゃうのが気になります。  

### 対策3：BigDecimalを継承する

```kotlin
interface CoffeeMilkAnalyzer {
    fun getCoffeeContentRate(target: CoffeeMilk): CoffeeContentRate
    
    class CoffeeContentRate(val decimal: String)
        :BigDecimal(decimal) {
        init {
            require(this >= BigDecimal.ZERO)
            require(this <= BigDecimal.ONE)
        }

        override fun toByte(): Byte {
            throw UnsupportedOperationException()
        }
        override fun toChar(): Char {
            throw UnsupportedOperationException()
        }
        override fun toShort(): Short {
            throw UnsupportedOperationException()
        }
    }
}
```

完璧に制限できてます！  
毎回定義が面倒とか、コンストラクタが文字列だけとかの問題は、
共通のMyDecimal型を定義して頑張ればいいかなと思います。  

ただ、   
`toByte` `toChar` `toShort`  
これなんだ？

これについては次回、調べて報告します。

以上

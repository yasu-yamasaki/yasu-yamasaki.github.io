---
title: "internalスコープのclassをconstructor injectionする(Kotlin, Spring)"
date: 2021-05-06T14:00:24+09:00
tags: [ "Kotlin", "Tech", "Spring" ]
---

掲題の件について、若干詰まったのでブログに残す。

...

## やりたいこと

以下のようなinternal classを
```kotlin
@Service
internal class SubService { /* .. */ }
```

以下のようなclassにconstructor injectionさせたい
```kotlin
@Service
class MainService(private val subService: SubService) { /* .. */ }
```

SubServiceとMainServiceは同じパッケージとする。

## え？普通にできないんだっけ？

上記例はコンパイルエラーになる。  
なぜなら、publicなclassのprimaryコンストラクタはpublicスコープであり、
そこにintenalスコープのclassのインスタンスを引数に取れないから。

## 解決策① constructor injectionを諦める

これが一番簡単。だが、そもそも掲題の前提条件を満たしていないように思う。

```kotlin
@Service
class MainService {
    @Autowired
    private val subService: SubService

    /* .. */ 
}
```

## 解決策② MainServiceもinternalスコープにしちゃう

MainServiceもinternalスコープにしてしまえば、primaryコンストラクタもinternalスコープになるので
コンパイルエラーにならなくなる。  
ただ、パッケージ外のMainServiceに依存するクラスがMainServiceをみれなくなるので、
MainServiceをpublicなinterfaceにして、実装クラスをinternalスコープにする。

```kotlin
interface MainService { /* .. */ }

@Service
internal class MainServiceImpl(private val subService: SubService): MainService { 
/* .. */ 
}
```

一瞬SpringがちゃんとMainServiceImplをInjectionしてくれるのかなと不安になったけど、問題なく動作した。  
Spring以外のフレームワークを使っていて、internalスコープのclassを他パッケージにinjectさせたい場合は、
publicなMainServiceFactoryを作れば解決できそう。  

以上
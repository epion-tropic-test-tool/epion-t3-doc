# Project
`ETTT` をカスタマイズするためのプロジェクトの構築方法について説明します。
本章では`Gradle`をベースに説明をします。ビルドシステムは`Gradle`が必須というわけではありませんので好みに応じて`Maven`等をご利用ください。　


## build.gradleの例

```groovy
dependencies {
  // ETTTのCoreライブラリを参照
  compile project('com.zomu.t:epion-t3-core:0.0.1')  
}
```

`ETTT`をカスタマイズするために必要な定義は、`ETTT`のCoreライブラリを参照するのみです。

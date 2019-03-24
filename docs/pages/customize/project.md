# Project
`ETTT` をカスタマイズするためのプロジェクトの構築方法について説明します。
本章では`Gradle`をベースに説明をします。ビルドシステムは`Gradle`が必須というわけではありませんので好みに応じて`Maven`等をご利用ください。　


## build.gradleの例

```groovy

repositories {
    maven {
        // ETTTのCoreライブラリが公開されているリポジトリ
        url  "https://dl.bintray.com/takashno/maven"
    }
}

dependencies {
  // ETTTのCoreライブラリを参照
  compile project('com.zomu.t:epion-t3-core:0.0.1')  
}
```

`ETTT`をカスタマイズするために必要な定義は、`ETTT`のCoreライブラリを参照するのみです。現状はJCenterに対して公開申請を行なっている途中です。そのためMavenのリポジトリを追加する必要があります。
その他必要なライブラリに関しては追加します。

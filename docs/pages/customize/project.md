# Project
`ETTT` をカスタマイズするためのプロジェクトの構築方法について説明します。
本章では`Gradle`をベースに説明をします。ビルドシステムは`Gradle`が必須というわけではありませんので好みに応じて`Maven`等をご利用ください。　


## build.gradleの例

```groovy

repositories {
    maven {
        url "https://dl.bintray.com/epion-tropic-test-tool/maven"
    }
    mavenCentral()
    jcenter()
}

dependencies {
  // ETTTのCoreライブラリを参照
  api 'com.epion_t3:epion-t3-core:0.0.1' 
}
```

`ETTT`をカスタマイズするために必要な定義は、`ETTT`のCoreライブラリを参照するのみです。  
JCenterにてライブラリは公開されています。
ただし最新版を利用したい場合はは`epion-tropic-test-tool` のリポジトリを参照してください。
その他必要なライブラリに関しては、ユーザが適宜追加してください。

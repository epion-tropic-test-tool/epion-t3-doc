# Project

テストを行うためには、シナリオを管理するためのプロジェクトを作成します。  
プロジェクトは、[Basic Directory Structure](/pages/specification/basic_directory_structure) で定義したシナリオ体型に
`ETTT` を実行するためのライブラリや実行ファイル等を組み合わせたものになります。（実行プロジェクト）
`ETTT` では `Gradle` or `Maven` をベースとした実行プロジェクトを推奨しています。

### Create Project

`Gradle` or `Maven` をベースとした `ETTT` の実行プロジェクトのテンプレートを用意しています。  
[GitHubのリポジトリ](https://github.com/epion-tropic-test-tool/epion-t3-scenario-template) からダウンロードもしくはForkを行い作成します。

#### Structure

テンプレートプロジェクトは以下のような構成となっています。

```
root
 |-- result
 |     `-- output result report 
 |-- scenarios
 |     `-- see "Basic Directory Structure"
 |-- build.gradle
 |-- gradlew
 |-- gradlew.bat
 |-- gradle
 |     `-- wrapper
 |           |-- gradle-wrapper.jar
 |           `-- gradle-wrapper.properties

```


#### Dependency Library

`ETTT` を実行するためのライブラリは `build.gradle` もしくは `pom.xml` に定義します。
使用したいカスタム機能があれば、依存関係に追加する必要があります。


build.gradle
```groovy
plugins {
    id 'java'
}

configurations {
    et3
}

repositories {
    maven {
        url "https://dl.bintray.com/epion-tropic-test-tool/maven"
    }
    mavenCentral()
    jcenter()
}

dependencies {
    // Core
    et3 'com.epion_t3:epion-t3-core:0.0.1'
    // Customs
    et3 'com.epion_t3:epion-t3-basic:0.0.1'
    // 利用したいパッケージを追加
}

task execute(type: JavaExec) {
    main = "com.epion_t3.core.application.Application"
    classpath = configurations.et3
    args = [  // 引数を指定
            '-v',
            'v1.0',
            '-m',
            'test',
            '-p',
            'develop',
            '-t',
            'scenario-selenium-sample',
            '-s',
            "${projectDir}/scenario"
    ]
}
defaultTasks 'execute'
```

Gradleに慣れていない方は、少し扱いづらいかもしれないが、
自分で変更する点は主に依存関係（dependencies）と実行引数（args）の部分となる。

##### 依存関係の指定方法
[Gradle](https://gradle.org/) の依存関係定義のため、公式ドキュメントを見てもらった方が確実です。
ここでは、抜粋的に設定方法を説明します。

```
dependencies {
  et3 'GroupId:ArtifactId:Version'

  // omitted 
```

##### 実行引数の指定方法
実行引数については、[How To Use - Run](/pages/use/run.md) の `Option` にて詳細に記載をしているので参照してください。
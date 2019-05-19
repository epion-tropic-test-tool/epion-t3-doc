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

`ETTT` を実行するためのライブラリは `build.gradle` もしくは `pom.xml` に定義します。
使用したいカスタム機能があれば、ここに追加する必要があります。




#### Build Project

Gradleによるビルドを行います。  
極力Wrapperを利用したビルドを行ってください。

```bash
./gradlew build -x test
```

#### Get Executable Jar

`ETTT`の実行可能Jarは以下のパスに存在するので取得します。

```bash
{projectRootDir}/modules/epion-t3/build/libs/epion-t3.jar
```

### Add Custom Package in Executable Jar

ユーザーの方々が作成されたカスタム機能を実行可能Jarに対して追加する方法を説明します。  
起動の仕組みを含め改善予定ではありますが、現状はこの方法による提供のみとなることをご了承ください。  
また、改善要望等は随時[Issue](https://github.com/epion-tropic-test-tool/epion-t3/issues)にて承っております。  

#### Edit Build Gradle

作成したカスタム機能を実行可能Jarに追加するために、Gradleの依存関係を修正します。  
変種対象のGradleの設定ファイルは以下となります。  

```
{projectRootDir}/modules/epion-t3/build.gradle
```

Gradleの依存関係に対して適宜追加してください。

```groovy
dependencies {

    // Core
    compile project(':modules:epion-t3-core')

    // Commands
    compile project(':modules:epion-t3-basic')
    compile project(':modules:epion-t3-selenium')
    compile project(':modules:epion-t3-rest')
    compile project(':modules:epion-t3-rdb')
    
    // Custom Commands
    // ...Add Heare  // (1)

}

jar {
    manifest {
        attributes "Main-Class": "com.zomu.t.epion.tropic.test.tool.application.Application"
    }
    from {
        configurations.compile.collect { it.isDirectory() ? it : zipTree(it) }
    }
}
```

1. この行に対して、カスタム機能のライブラリの依存関係を追加します。

この対応を行った後に、実行可能Jarを作成する手順は通常時に実行可能Jarを作成する手順と同一です。



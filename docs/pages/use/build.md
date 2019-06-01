# Project

テストを行うためには、シナリオを管理するためのプロジェクトを作成します。  
プロジェクトは、[Basic Directory Structure](/pages/specification/basic_directory_structure) で定義したシナリオ体型に
`ETTT` を実行するためのライブラリや実行ファイル等を組み合わせたものになります。（実行プロジェクト）
`ETTT` では `Gradle` or `Maven` をベースとした実行プロジェクトを推奨しています。

## Create Project

`Gradle` or `Maven` をベースとした `ETTT` の実行プロジェクトのテンプレートを用意しています。  
[GitHubのリポジトリ](https://github.com/epion-tropic-test-tool/epion-t3-scenario-template) からダウンロードもしくはForkを行い作成します。

## Structure

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
 |-- pom.xml
 |-- mvnw
 |-- mvnw.cmd
 |-- .mvn
 |     `-- wrapper
 |           |-- maven-wrapper.jar
 |           |-- maven-wrapper.properties
 |           `-- MavenWrapperDownloader.java

```


## Gradle Settings

`ETTT` を `Gradle` で実行するためには `build.gradle` を用意します。  
使用したいカスタム機能があれば、依存関係に追加する必要があります。  
実行するシナリオや、その他オプションについても定義する必要があります。


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

`Gradle` に慣れていない方は、少し扱いづらいかもしれないですが、
自分で変更する点は主に依存関係（dependencies）と実行引数（args）の部分となります。

##### 依存関係の指定方法
[Gradle](https://gradle.org/) の依存関係定義のため、公式ドキュメントを見てもらった方が確実です。  
ここでは、抜粋的に設定方法を説明します。

```
dependencies {
  et3 'GroupId:ArtifactId:Version'

  // omitted 
```

`dependencies` には依存するライブラリを追加します。
`ETTT`では全ての機能がライブラリという形、つまり個別のJarファイルとして存在しています。
利用したい機能のライブラリ（Jar）を`dependencies` へ定義することによって、利用が可能になります。


##### 実行引数の指定方法
実行引数については、[How To Use - Run](/pages/use/run.md) の `Options` にて詳細に記載をしているので参照してください。

## Maven Settings

`ETTT` を `Maven` で実行するためには `pom.xml` を用意します。  
使用したいカスタム機能があれば、依存関係に追加する必要があります。  
実行するシナリオや、その他オプションについても定義する必要があります。

pom.xml
```xml
<?xml version="1.0" encoding="utf-8" ?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.epion_t3</groupId>
    <artifactId>epion-t3-scenario-template</artifactId>
    <version>0.0.1</version>

    <dependencies>
        <!-- Core -->
        <dependency>
            <groupId>com.epion_t3</groupId>
            <artifactId>epion-t3-core</artifactId>
            <version>0.0.1</version>
        </dependency>
        <!-- Customs -->
        <dependency>
            <groupId>com.epion_t3</groupId>
            <artifactId>epion-t3-basic</artifactId>
            <version>0.0.1</version>
        </dependency>
        <!-- 利用したいパッケージを追加 -->
    </dependencies>

    <build>
        <defaultGoal>exec:java</defaultGoal>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.6.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>exec</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <executable>maven</executable>
                    <mainClass>com.epion_t3.core.application.Application</mainClass>
                    <workingDirectory>./</workingDirectory>
                    <arguments>
                        <argument>-v</argument>
                        <argument>v1.0</argument>
                        <argument>-m</argument>
                        <argument>test</argument>
                        <!-- プロファイルの指定 -->
                        <argument>-p</argument>
                        <argument>develop</argument>
                        <!-- 対象シナリオ -->
                        <argument>-t</argument>
                        <argument>scenario-selenium-sample</argument>
                        <argument>-s</argument>
                        <argument>./scenario</argument>
                    </arguments>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

`Maven` に慣れていない方は、少し扱いづらいかもしれないですが、
自分で変更する点は主に依存関係（dependencies）と実行引数（arguments）の部分となります。

##### 依存関係の指定方法
[Maven](https://maven.apache.org/) の依存関係定義のため、公式ドキュメントを見てもらった方が確実です。  
ここでは、抜粋的に設定方法を説明します。

```
<dependencies>
    <dependency>
        <groupId>GroupId</groupId>
        <artifactId>ArtifactId</artifactId>
        <version>Version</version>
    </dependency>
  // omitted 
```

`dependencies` には依存するライブラリを追加します。
`ETTT`では全ての機能がライブラリという形、つまり個別のJarファイルとして存在しています。
利用したい機能のライブラリ（Jar）を`dependencies` へ定義することによって、利用が可能になります。


##### 実行引数の指定方法
実行引数については、[How To Use - Run](/pages/use/run.md) の `Options` にて詳細に記載をしているので参照してください。
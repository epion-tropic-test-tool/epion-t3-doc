# Run

`ETTT`の実行方法について説明します。  
実行方法は、`Gradle`もしくは`Maven`を利用した起動方法を推奨しています。  
推奨する実行方法を行うプロジェクトの作り方は、[Project](pages/use/build.md) を参照してください。

## Options

`ETTT`の起動オプションは以下が用意されています。

|Option|Description|Required|Example|
|:---|:---|:---|:---|
|version(v)|バージョン。現在は`v1.0`のみ対応している。|-|v1.0|
|target(t)|テスト対象としてシナリオIDを指定|-|-|
|scenario(s)|シナリオ置き場のパスを指定|-|-|
|output(o)|ツール実行結果のパスを指定|-|-|
|mode(m)|モード指定。現状は`test`のみ対応している。|-|test|
|profile(p)|プロファイルを指定。複数のプロファイルはカンマ区切りで指定。|-|develop,integration|
|debug(d)|デバッグモード指定|-|-|
|noreport(n)|レポート出力抑制指定|-|値は不要|
|help(h)|ヘルプ表示|-|値は不要|

オプションは、longName(shortName)表記となります。  
longNameの例
```
--output パス
```

shortNameの例
```
-o パス
```

### Output Debug Log

`ETTT`の実行時のログのレベルを変更したい場合には、JVMの引数にて定義を行います。  
起動コマンドに対して以下のように指定を行ってください。  
例として`DEBUG`レベルで起動するものを示します。

```bash
java -jar epion-t3.jar -DloggerLevel=DEBUG {options}
```

## Run with Gradle Task

[実行プロジェクト](pages/use/build.md)を作成している場合には、`Gradle` の機能を利用して実行が可能です。
`build.gradle` を設定済みであれば `gradlew` コマンドを実行するのみで良いです。
`Gradle Wrapper` という機能を利用することにより `Gradle` のダウンロード・インストール等は不要になります。  
`Proxy` による接続を行う場合には設定が必要になる可能性があります。

```bash
$ cd {project_root}
$ ./gradlew
```

Windows以外で実行する場合には、`gradlew` の権限をchmod等で必要に応じて変更してください。

```bash
$ chmod +x gradlew
```

## Run with Maven Build

[実行プロジェクト](pages/use/build.md)を作成している場合には、`Maven` の機能を利用して実行が可能です。
`pom.xml` を設定済みであれば `mvnw` コマンドを実行するのみで良いです。
`Maven Wrapper` という機能を利用することにより `Maven` のダウンロード・インストール等は不要になります。  
`Proxy` による接続を行う場合には設定が必要になる可能性があります。

```bash
$ cd {project_root}
$ ./mvnw
```

Windows以外で実行する場合には、`mvnw` の権限をchmod等で必要に応じて変更してください。

```bash
$ chmod +x mvnw
```

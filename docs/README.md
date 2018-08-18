![ETTT](media/logo-xsmall.svg)

# What Is ETTT?
Epion Tropic Test Tool (略称 : `ETTT`) はテストの効率化を目的として、できるだけ扱いやすくした回帰テストツールです。  
`Epion`には「`次世代`」という意味を込めており、`Tropic`は「`回帰線`」という意味を持つことから選択しています。
簡単に言うとテストを行いたい内容をシナリオと呼ばれる独自仕様のYAMLに記載し、ツールに与えることでテストを走行させるというものです。  
`ETTT`はキーワード駆動テストを行うための手段として用いれるようにすることを目標としています。  
最終的には「ISO/IEC/IEEE 29119-5:2016」に準拠したソフトウェアを作成したいと考えています。  


## Features

- 独自仕様のYAMLで定義されたシナリオに従って処理を自動実行することができます。
- シナリオの実行結果をHTML形式のレポートとして出力します。
- 複数環境で実行する場合に、環境毎に動的に解決したい値を定義するProfile機能を用意しています。
- 機能(Command)は拡張性をもっており、それぞれが疎結合であるため独立しています。
- ユーザが必要な機能を自分で実装し、追加することが可能です。

## Function Overview

このツールが保有する機能の大まかな一覧は以下です。

- ファイルシステムの操作を行う
- リモートで稼働しているSSHサーバと対話形式の操作を行う
- SCPによるファイル操作を行う
- FTPによるファイル操作を行う
- SeleniumのWebDriverを用いてローカルのブラウザの自動操作を行う
- AWSのS3に対してAPI発行を行う（一部APIのみ）
- AWSのSQSに対してAPI発行を行う（一部APIのみ）
- Redisに対して操作を行う（AWSのElastiCache for Redisを想定）
- RDBMSに対してSQLを発行する
- 任意のローカルコマンドの実行を行う

# Concept
このツールの使い方やカスタム方法の説明を行う前に、概念を説明します。  
用語を含め目線を合わせる必要があります。  

## Glossary

以下は`ETTT`上での用語集になります。

|名称|読み|説明|
|---|---|---|
|Scenario|シナリオ|テストツールに対してユーザが与える指示の総称にして実行単位です。|
|Flow|フロー|シナリオ中でどんなコマンドをどんな順序で実行するかを指示するものです。他にもフローは制御処理を行うことが可能です。|
|Command|コマンド|実際に実行する処理です。|
|TestData|データ||
|Expected|エクスペクテッド||
|Actual|アクチュアル||
|Assertion|アサーション||


# Specification

`ETTT`の独自仕様のYAMLを記載する必要があります。  
本章では、その独自仕様について説明を行います。

## Basic Structure
`ETTT`の基本構文は以下の通りになります。   
全てを１つのYAMLに表現することも可能ですが、後述するディレクトリ構成の説明にある通り、用途毎に分割して管理することを推奨しています。  

```yaml
t3 : 1.0
type : scenario || parts || config

info :
   version : Scenario-Version
   id : Scenario-Unique-ID
   summary: Optional Execute Scenario Summary.
   description:  Optional Execute Scenario Full Description.

flows :
  - id: Flow-Unique-ID
    type : FlowType 

commands :
  - id : Command-Unique-ID
    command : ExecuteCommandName
    summary : Optional Command Execute Summary. 
    description : Optional Command Execute Full Description.

variables :
  global :
    {Unique-Key} : Initial Value
  scenario :
    {Unique-Key} : Initial Value

profiles :
  {Unique-Profile-Name} :
    {Unique-Key} : Fix Value

customs :
  command :
    {Custom Unique Name} : Custom Package
```

YAMLのキーが `{Key}` のように記載されているものはユーザが定義する任意の値となります。


### Basic Information
シナリオの基本情報を記載するものです。

```yaml
info :
   version : 1.0.0  # (1)
   id : Scenario-Unique-ID  # (2)
   summary: Optional Execute Scenario Summary.  # (3)
   description:  Optional Execute Scenario Full Description.  # (4)
```

1. シナリオのバージョン管理をするためのバージョンです。(現状用途を見いだせていない)
1. シナリオのID。IDであるため、一意性を利用するユーザが担保してください。
1. シナリオの概要です。必須ではありません。
1. シナリオの説明です。必須ではありません。



## Basic Directory Structure
`ETTT`は様々な１つのYAMLで全てを表すこともできますが、  
シナリオのメンテナンス性や複数人で作成する場合の運用を考慮する場合、  
細かく用途によってファイルを分割・配置することを推奨します。  
また、これらのシナリオはGitやSubversionによるバージョン管理を行うことが重要です。

```
root
 |
 |-- parts
 |     |
 |     `-- {Parts-Unique-ID}
 |           |
 |           |-- t3-{Parts-Unique-ID}.yaml
 |           |
 |           |-- data
 |           |     |-- excel_data.xlsx
 |           |     `-- etc...
 |           |
 |           `-- assert
 |                 |-- excel_data.xlsx
 |                 `-- etc...
 |
 |-- scenarios
 |     |
 |     `-- {Scenario-Unique-ID}
 |           |
 |           |-- t3-{Scenario-Unique-ID}.yaml
 |           |
 |           |-- data
 |           |     |-- excel_data.xlsx
 |           |     `-- etc...
 |           |
 |           `-- assert
 |                 |-- excel_data.xlsx
 |                 `-- etc...
 |-- profiles
 |     |
 |     `-- t3-{ProfileName}.yaml
 |
 |
 `-- customs
       |
       `-- t3-custom.yaml

```


# How To Use
このツールを利用するにあたり、以下の環境が整っていることを確認してください。

|ソフトウェア|内容|
|:---|:---|
|[Java](https://java.com/ja/download/)|Javaの1.8以上がインストールされており、パスが通っていることを確認してください。|
|[Graphviz](http://www.graphviz.org/)|Graphvizの2.26.3以上がインストールされており、パスが通っていることを確認してください。レポート機能を利用しない場合は、インストールおよびパスの設定は不要になります。|


## ツールの起動

エンジンの構成は以下の通りです。


### 実行結果

```
root
 |
 |-- YYYYMMDD_HHMMSS_{scenario_id}
 |     |
 |     |-- result.html 結果となるHTMLレポート
 |     `-- evidences
 |           |
 |           `-- {reference scenario_process_id}
 |                 |-- *.log
 |                 `-- etc...
```

# How To Customize
`ETTT`は具備している機能では足りない場合や、振る舞いを変更したい場合に自分自身でカスタマイズすることができるようになっています。
`ETTT`をカスタマイズするためには、Javaでの実装を行う必要があります。

## 前提
`ETTT`をカスタマイズするには以下の環境が整っている必要があります。

|ソフトウェア|内容|
|---|---|
|Java|`ETTT`のエンジンはJavaで作成されています。Javaの1.8以上がインストールされており、パスが通っていることを確認してください。|
|Gradle|`ETTT`のビルドに利用します。バージョン4以降を推奨しています。|


## Project
`ETTT` をカスタマイズするためのプロジェクトの構築方法について説明します。
本章では`Gradle`をベースに説明をします。ビルドシステムは`Gradle`が必須というわけではありませんので好みに応じて`Maven`等をご利用ください。　


### build.gradleの例

```groovy

// omitted

dependencies {

    // ETTTのCoreライブラリを参照
    compile project('com.zomu.t:epion-t3-core:0.0.1')
    
}

// omitted

```

`ETTT`をカスタマイズするために必要な定義は、`ETTT`のCoreライブラリを参照するのみです。


## Message
`ETTT`では、例外時およびログ出力時に`ResourceBundle`が利用できます。
本章では、どのように`ResourceBundle`を定義し利用するかの説明を行います。

### Messageを定義する
制約として、メッセージを定義するファイル名の接尾辞は`_messages.properties`である必要があります。
クラスパス配下に存在する接尾辞が合致したリソースを`ResourceBundle`で扱えるようにします。

___実装例___

`ETTT`では以下のようなメッセージの定義をしています。
```properties
com.zomu.t.epion.t3.core.err.0001=予期せぬエラーが発生しました.システムの故障の可能性があります.
com.zomu.t.epion.t3.core.err.0002=指定されたバージョンは存在しません.バージョン:{0}
com.zomu.t.epion.t3.core.wrn.0001=変数バインドに失敗しました.対象文字列:{0}
```


### Messages用のEnumを定義する
この対応は必須ではありませんが、`ETTT`ではこのような実装を行なっていますという紹介になります。
メッセージを利用する際に、メッセージコードを文字列としてそのまま指定すると変更や削除が入った際にGrep等を行なって確実に修正することが面倒であるため、
各メッセージをEnumで定義してそのEnumを利用するようにしています。
この対応は、自動化がされていない限りはEnumを作成するのも面倒であるためどれほどのメリットがあるか定かではありません。
`ETTT`ではEnumを実装するために`Messages`インタフェースを用意しており、後述するメッセージを取得するクラスと連動するようにしています。
FQCNは以下になります。

~~~
com.zomu.t.epion.tropic.test.tool.core.message.Messages
~~~

___実装例___

`ETTT`では以下のようなメッセージのEnum定義をしています。

```java
@Getter
@AllArgsConstructor
public enum CoreMessages implements Messages {

    CORE_ERR_0001("com.zomu.t.epion.t3.core.err.0001"),

    CORE_ERR_0002("com.zomu.t.epion.t3.core.err.0002"),

    CORE_WRN_0001("com.zomu.t.epion.t3.core.wrn.0001");

    private String messageCode;
}
```

※`ETTT`ではボイラープレートコードの排除のため[Lombok](https://projectlombok.org/)を利用しています。



### Messageを取得する
メッセージの取得は、`MessageManager`クラスを利用することで行えます。
FQCNは以下になります。

~~~
com.zomu.t.epion.tropic.test.tool.core.message.MessageManager
~~~

`MessageManager`はカスタマイズ実装者が利用するクラスであり、状況に応じて使い分けが可能ないくつかのオーバーロードされたメソッドを保有しています。
このクラスはシングルトンであるため、インスタンスを取得してから利用してください。

___実装例___

```java

MessageManager.getInstance().getMessage(CoreMessages.CORE_ERR_0001);

// 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessage("com.zomu.t.epion.t3.core.err.0001");

// 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessage(CoreMessages.CORE_ERR_0002, "1.0.0");

// 指定されたバージョンは存在しません.バージョン:1.0.0

MessageManager.getInstance().getMessage("com.zomu.t.epion.t3.core.err.0002", "1.0.0");

// 指定されたバージョンは存在しません.バージョン:1.0.0

MessageManager.getInstance().getMessageWithCode(CoreMessages.CORE_ERR_0001);

// [com.zomu.t.epion.t3.core.err.0001] 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessageWithCode("com.zomu.t.epion.t3.core.err.0001");

// [com.zomu.t.epion.t3.core.err.0001] 予期せぬエラーが発生しました.システムの故障の可能性があります.

```

## Error Process
カスタマイズ実装を行う際のエラー処理について説明します。
`ETTT`では細かく用途分類された例外クラスをいくつか保有していますが、カスタマイズ実装にて利用するのは`SystemException`のみです。
FQCNは以下になります。

~~~
com.zomu.t.epion.tropic.test.tool.core.exception.SystemException
~~~

`SystemException`は`java.lang.RuntimeException`を継承しています。
コンストラクタは、メッセージと例外(`Throwable`)を受け取るものを用意しています。
バインド変数の有無などの状況に応じて使い分けが可能ないくつかのオーバーロードされたコンストラクタを保有します。(コンストラクタもオーバーロードっていうのかな・・・)

___実装例___

`t`は`Throwable`です。

```java

throw new SystemException(CoreMessages.BASIC_ERR_9001);

throw new SystemException(t, CoreMessages.BASIC_ERR_9001);

throw new SystemException("com.zomu.t.epion.t3.core.err.0001");

throw new SystemException(t, "com.zomu.t.epion.t3.core.err.0001");

throw new SystemException(CoreMessages.BASIC_ERR_9002, "1.0.0");

throw new SystemException(t, CoreMessages.BASIC_ERR_9002, "1.0.0");

throw new SystemException("com.zomu.t.epion.t3.core.err.0002", "1.0.0");

throw new SystemException(t, "com.zomu.t.epion.t3.core.err.0002", "1.0.0");

```

`SystemException`の内部では、`MessageManager`を利用してメッセージの解決を行ないます。
そのため、`Messages`インタフェースを実装したEnumについても引数で受け付けるようにしています。


## Flow
Flowはシナリオの動作を制御するためのものです。  
このFlowをカスタマイズすることによってコマンドの実行順序を動的に制御したり、特定の入力から得た情報で繰り返し処理を行ったりすることができます。

### FlowにおけるModelとRunner


## Command
Commandはシナリオにおける実際の動作を行うものです。  
このCommandをカスタマイズすることによって`ETTT`で任意の動作を行えるようにできます。  

### CommandにおけるModelとRunner
Commandのカスタマイズを行うためには、ModelクラスとRunnerクラスを1対1の関係性で作成する必要があります。
ModelクラスはYAMLの定義を読み込むためのJavaBeansです。RunnerはYAMLから読み込んだ情報を元に実際に処理を行うためのクラスです。
それぞれのクラスに対して `ETTT` にて決められたインタフェースの実装やスーパークラスの継承が必要になります。

### Modelの作成
CommandのModelクラスを実装するためには、`Command`クラスを継承する必要があります。
また、Commandであることを示すための`CommandDefinition`アノテーションを付与する必要があります。
それぞれ、FQCNは以下となります。
~~~
com.zomu.t.epion.tropic.test.tool.core.model.scenario.Command
com.zomu.t.epion.tropic.test.tool.core.annotation.CommandDefinition
~~~


先ずはModelクラスの作成の前にスーパークラスである`Command`クラスの仕様を理解する必要があります。

|Field|Type|Required|Description|
|:---|:---:|:---:|:---|
|id|String|Yes|Commandを定義する際に割り振るID。IDなので一意性を持ってユーザが設定するものです。|
|summary|String|No|Commandの概要。|
|description|String|No|Commandの説明。概要より詳細に記載されることを想定しています。|
|command|String|Yes|Commandの指定。ユーザが利用時に`@CommandDefinition`のid属性で定義してある値を指定します。|
|target|String|No|Commandの対象を指定する想定で作成したフィールドです。デフォルト状態で存在するフィールドで自由に利用できます。|
|value|String|No|Commandで利用する叩いを指定する想定で作成したフィールドです。デフォルト状態で存在するフィールドで自由に利用できます。|

次にカスタマイズする祭のModelクラスの実装例を示します。

```java
package com.zomu.t.epion.tropic.test.tool.basic.command.model;

import com.zomu.t.epion.tropic.test.tool.basic.command.runner.StringConcatRunner;
import com.zomu.t.epion.tropic.test.tool.core.annotation.CommandDefinition;
import com.zomu.t.epion.tropic.test.tool.core.model.scenario.Command;
import org.apache.bval.constraints.NotEmpty;
import java.util.List;

@CommandDefinition(
  id = "StringConcat", /* (1) */
  runner = StringConcatRunner.class) /* (2) */
public class StringConcat extends Command {

  @NotEmpty
  private List<String> referenceVariables; /* (3) */

// omitted Getter And Setter Methods

}
```

このModelクラスは文字列結合を行うための`StringConcat`コマンドの実際の実装コードになります。
それぞれの実装ポイントについて以下で説明します。
実際に`ETTT`ではボイラープレートコードの排除のため[Lombok](https://projectlombok.org/)を利用しています。

1. idにはCommandの名前を設定します。このidは重複すると`ETTT`が意図せぬ挙動を行う場合がありますので命名する際には一意性に気をつけてください。|
2. runnerにはCommandの実処理を行うRunnerクラスを設定します。`ETTT`では起動時に`@CommandDefinition`アノテーションからModelクラスとRunnerクラスを紐づける時に利用します。|
3. カスタマイズしたい処理に必要な情報を得るためのフィールドを定義します。BeanVaridationを行うことができます。`ETTT`では軽量な[Apache BVal](http://bval.apache.org/)を利用しています。|

このModelクラスに対するYAMLの定義例は以下のようになります。

```yaml
commands:
 - id: sample-command
   summary : "メールアドレス作成"
   description: "2つの文字列(固定もしくは変数)を結合してメールアドレスを作成します"
   command: "StringConcat"
   target : "mailaddress"
   referenceVariables : ["scenario.username", "@example.ettt.com"]
```
このようにスーパークラスである`Command`クラスのフィールドも利用することができますので、
どのフィールドをどのような用途で利用するかは自由です。


### Runnerの作成
CommandのRunnerクラスを実装するためには、`CommandRunner`インタフェースを実装する必要があります。
FQCNは以下をとなります。
~~~
com.zomu.t.epion.tropic.test.tool.core.command.runner.CommandRunner
~~~

`CommandRunner`インタフェースではexecuteメソッドを必ず実装する必要があります。
executeメソッドはCommandの処理を実行するメイン処理メソッドです。
このメソッド内でCommandが実現したい内容を実装する必要があります。


```java
void execute(  /* (1) */
  final COMMAND process,  /* (2) */
  final Map<String, Object> globalScopeVariables,  /* (3) */
  final Map<String, Object> scenarioScopeVariables,  /* (4) */
  final Map<String, Object> flowScopeVariables,  /* (5) */
  final Map<String, EvidenceInfo> evidences,  /* (6) */
  final Logger logger) throws Exception;  /* (7) */
```

1. 戻り値はありません。
1. Command定義クラスであり、`COMMAND`(総称型指定)
1. 全体スコープ変数Mapです。値を取り出したり設定したりする目的で利用します。
1. シナリオスコープ変数Mapです。値を取り出したり設定したりする目的で利用します。
1. Flowスコープ変数Mapです。値を取り出したり設定したりする目的で利用します。
1. エビデンスMapです。エビデンスを取り出したり設定したりする目的で利用します。
1. コマンド専用のロガーでありSLF4Jの`Logger`を利用している。ログを出力する時にはこの`Logger`を利用すること。

例外ハンドリング処理は、Commandの実行を司るクラスにて行うため特に必要ありません。
各スコープ変数には、`ETTT`であらかじめ予約的に利用している`Key`が存在します。(一覧にて後述します)
また、`CommandRunner`インタフェースには、いくつかの便利メソッドが`defaultメソッド`として提供されていますので必要に応じてご利用ください。

**aaaaa**



**実装例**

Modelクラスの説明でも例に出した`StringConcat`コマンドに対する`CommandRunner`の実装を例に出して説明します。

```java

```
![ETTT](media/logo-xsmall.svg)

# What Is ETTT?
Epion Tropic Test Tool (略称 : ETTT) はテストの効率化を目的として、できるだけ扱いやすくしたテストツールです。  
簡単に言うとテストを行いたい内容をシナリオと呼ばれる独自仕様のYAMLに記載し、ツールに与えることでテストを走行させるというものです。  
ETTTはキーワード駆動テストを行うための手段として用いれるようにすることを目標としています。  
最終的には「ISO/IEC/IEEE 29119-5:2016」に準拠したソフトウェアを作成したいと考えています。  


## Function Overview

このツールが保有する機能の大まかな一覧は以下です。

- YAMLで定義されたシナリオに従って処理を自動実行する
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

以下はETTT上での用語集になります。

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

ETTTの独自仕様のYAMLを記載する必要があります。  
本章では、その独自仕様について説明を行います。

## Basic Structure
ETTTの基本構文は以下の通りになります。   
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

```yaml
info :
   version : Scenario-Version
   id : Scenario-Unique-ID
   summary: Optional Execute Scenario Summary.
   description:  Optional Execute Scenario Full Description.
```


## Basic Directory Structure
ETTTは様々な１つのYAMLで全てを表すこともできますが、  
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

## 前提
このツールを利用するにあたり、以下の環境が整っていることを確認してください。


|ソフトウェア|内容|
|---|---|
|Java|Javaの1.8以上がインストールされており、パスが通っていることを確認してください。|



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
|Java|ETTTのエンジンはJavaで作成されています。Javaの1.8以上がインストールされており、パスが通っていることを確認してください。|
|Gradle|ETTTのビルドに利用します。バージョン4以降を推奨しています。|


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


## Flow
Flowはシナリオの動作を制御するためのものです。  
このFlowをカスタマイズすることによってコマンドの実行順序を動的に制御したり、特定の入力から得た情報で繰り返し処理を行ったりすることができます。

### Flowのインタフェース


## Command
Commandはシナリオにおける実際の動作を行うものです。  
このCommandをカスタマイズすることによって`ETTT`で任意の動作を行えるようにできます。  

### CommandにおけるModelとRunner
Commandのカスタマイズを行うためには、ModelクラスとRunnerクラスを1対1の関係性で作成する必要があります。
ModelはYAMLの定義を読み込むためのJavaBeansです。RunnerはYAMLから読み込んだ情報を元に実際に処理を行うためのクラスです。
それぞれのクラスに対して `ETTT` にて決められたインタフェースの実装やスーパークラスの継承が必要になります。

### Modelの作成
CommandのModelクラスを実装するためには、`com.zomu.t.epion.tropic.test.tool.core.model.scenario.Command`クラスを継承する必要があります。
また、Commandであることを示すための`com.zomu.t.epion.tropic.test.tool.core.annotation.Command`アノテーションを付与する必要があります。
以下に実装例を示します。


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
実際に`ETTT`ではボイラープレートコードの排除のため[`Lombok`](https://projectlombok.org/)を利用しています。

|#|Description|
|:---|:---|
|1|idにはCommandの名前を設定します。このidは重複すると`ETTT`が意図せぬ挙動を行う場合がありますので命名する際には一意性に気をつけてください。|
|2|runnerにはCommandの実処理を行うRunnerクラスを設定します。`ETTT`では起動時に`@CommandDefinition`アノテーションからModelクラスとRunnerクラスを紐づける時に利用します。|
|3|カスタマイズしたい処理に必要な情報を得るためのフィールドを定義します。BeanVaridationを行うことができます。`ETTT`では軽量な[`Apache BVal`](http://bval.apache.org/)を利用しています。|


### Runnerの作成



## 独自コマンドの作成方法

独自コマンドを作成するには、コマンド定義（Process）と実行クラス（Runner）を作成する必要があります。 +
コマンド定義は、YAMLに定義する内容を決めるものです。 +
実行クラスはコマンド定義でユーザが指定した内容に基づき処理を実行するものです。 +
ETTTでは、誰でも独自に作成ができるようにインターフェースやユーティリティを提供しています。


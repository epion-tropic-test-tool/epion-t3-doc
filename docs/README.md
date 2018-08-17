![ETTT](media/logo-xsmall.svg)

# What Is ETTT?
Epion Tropic Test Tool (略称 : ETTT) は、次世代回帰試験ツールです。  
テストの効率化を目的として、できるだけ扱いやすくしたテストツールです。  
簡単に言うとテストを行いたい内容をシナリオと呼ばれる独自仕様のYAMLに記載し、ツールに与えることでテストを走行させるというものです。  
ETTTはキーワード駆動テストを行うための手段として用いれるようにすることを目標としています。  
最終的には「ISO/IEC/IEEE 29119-5:2016」に準拠したソフトウェアを作成したいと考えています。  


## 機能

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

# Customize

## 概要
ETTTは具備している機能では足りない場合や、振る舞いを変更したい場合に
自分自身でカスタマイズすることができるようになっています。

## 前提
ETTTをカスタマイズするには以下の環境が整っている必要があります。

|ソフトウェア|内容|
|---|---|
|Java|ETTTのエンジンはJavaで作成されています。Javaの1.8以上がインストールされており、パスが通っていることを確認してください。|
|Gradle|ETTTのビルドに利用します。バージョン4以降を推奨しています。|

## 独自コマンドの作成方法

独自コマンドを作成するには、コマンド定義（Process）と実行クラス（Runner）を作成する必要があります。 +
コマンド定義は、YAMLに定義する内容を決めるものです。 +
実行クラスはコマンド定義でユーザが指定した内容に基づき処理を実行するものです。 +
ETTTでは、誰でも独自に作成ができるようにインターフェースやユーティリティを提供しています。


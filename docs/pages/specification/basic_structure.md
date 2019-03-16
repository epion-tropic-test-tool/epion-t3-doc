# Basic Structure
`ETTT`の基本構文は以下の通りになります。   
全てを１つのYAMLに表現することも可能ですが、後述するディレクトリ構成の説明にある通り、用途毎に分割して管理することを推奨しています。  

```yaml
t3 : 1.0

info :
   version : Scenario-Version
   id : Scenario-Unique-ID
   summary: Optional Execute Scenario Summary.
   description:  Optional Execute Scenario Full Description.
   
scenarios :
  - id: Target-Scenario-Unique-ID
    profiles: Execute Profile.
    mode: Execute Mode.
    debug: Execute Debug Option.
    noreport: Execute Non Report Output Option.

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
  packages :
    {Custom Unique Name} : Custom Package
```

YAMLのキーが `{Key}` のように記載されているものはユーザが定義する任意の値となります。  
YAML上の全ての値は、`${scope.name}`といった指定により変数をバインドすることができます。  
変数については、[Variables](/pages/specification/variables) にて詳細に説明を行います。


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

### Scenarios
実行するシナリオを指定するものです。   
複数のシナリオを一度に実行する際に利用することを想定しています。  
実行時のオプション指定を一部上書きすることも可能としているため、複数環境にわたる実行にも対応しています。

```yaml
scenarios :
  - id: Target-Scenario-Unique-ID # (1)
    profiles: Execute Profile. # (2)
    mode: Execute Mode. # (3)
    debug: Execute Debug Option. # (4)
    noreport: Execute Non Report Output Option. # (5)
```

1. 対象とする

### Flows
シナリオの動作制御を行うものです。
コマンドを呼び出したり、条件に従って分岐、ファイルを読み込んで繰り返し処理を行うなどです。
Flowはいくつもの機能が用意されている中からユーザが選んで組み合わせるものです。

```yaml
flows :
  - id: Flow-Unique-ID  # (1)
    type : FlowType   # (2)
    # 以降の要素はtypeに指定したFlow毎に異なる。
```

1. FlowのID。IDであるたえm、一意性を利用するユーザが担保してください。
1. 実行するFlowの種別です。


### Commands
コマンド＝つまり自動化を行いたい動作を定義するものです。  
コマンドはツールの中において最小の動作単位となります。  

コマンドの記載内容は拡張コマンド毎に異なるが、基本要素として全てのコマンドに用意されているものがあります。  
それが以下となります。

```yaml
commands : 
 - id : Command-Unique-ID # (1）
   summary : Optional Command Summary. # (2） 
   description : Optional Command Description. # (3）
   command : Execute-Command-ID # (4）
   target : Target # (5）
   value : Value # (6）
   # 以降の要素はcommandに指定したコマンド毎に異なる。
```

1. コマンドを一意に特定するID
1. コマンドの概要を定義します。記載されていればレポートにも表示されます。
1. コマンドの詳細を定義します。記載されていればレポートにも表示されます。
1. コマンドの種類を定義します。この要素にて、どの機能を実行するかを指定します。
1. 対象。汎用的な要素。機能によって利用するかしないかが異なります。
1. 値。汎用的な要素。機能によって利用するかしないかが異なります。

コマンドの一覧や、記載方法については [Command Index](pages/specification/command/index.md) を参照してもらいたい。

### Configurations

### Variables

### Profiles
プロファイルは`ETTT`を複数環境で実行する際に環境毎に変化させたい値を管理するための機能です。
このような値を環境依存値と呼んでいます。
例を挙げますと、ドメイン、ホスト名やポート番号、ユーザ名、パスワード等になります。

### Customs
カスタム機能の追加定義を行うものです。
`ETTT`は`Core`機能とそれ以外のカスタム機能で成り立ちます。
`Core`機能はツールの振る舞いやYAMLの形の定義などを持っているものです。
カスタム機能は、実際にユーザーが利用するすべてのコマンド等が含まれます。

```yaml
customs:
  packages: # (1)
    core: "com.zomu.t.epion.tropic.test.tool.core" # (2)
    basic: "com.zomu.t.epion.tropic.test.tool.basic" # (3)
    rest: "com.zomu.t.epion.tropic.test.tool.rest" # (3)
```

1. カスタム機能を利用するには、カスタム機能を読み込むためのパッケージの指定が必要になります。パッケージはカスタム機能が実装している`Java`クラスのパッケージになります。カスタム機能一覧にてパッケージの一覧を用意しているため適宜参照の上設定してください。

### Structure Image

シナリオ内の、Flow、Command、Configuration、Profile、Variableの関係性を図に表します。  

![structure-image](pages/specification/images/scenario-image.png)

この図の通り、シナリオとは、Flow、Command、Configuration、Profileの定義の集合です。
Variableはシナリオ上に静的に定義が可能ではありますが、基本的な用途として動的な値を管理するものとなるので、
シナリオ定義とは分離して記載しています。
また、Flowはあくまで、Command、Configuration、Profile、Variableを組み合わせたユーザ動作の流れを定義しているだけのものということを理解してください。
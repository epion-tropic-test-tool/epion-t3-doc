# Basic Structure
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





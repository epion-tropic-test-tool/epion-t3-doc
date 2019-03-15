# RDB Command

RDB（Relational DataBase）関連のコマンドを提供します。

|機能名|概要|アサート|
|:---|:---|:---|
|ImportRdbDataRunner|RDBに対してデータをインポートする|-|
|ExportRdbDataRunner|RDBからデータをエクスポートする|-|
|ExecuteRdbQueryRunner|RDBへ任意のクエリーを発行する|-|
|ExecuteRdbScriptRunner|RDBへ任意のスクリプト（SQL）の内容を発行する|-|

------

### 「ImportRdbDataRunner」

#### Function

1. RDBに対してデータのインポートを行います
1. XMLで定義したデータのインポートが行えます（DBUnit）
1. Excelで定義したデータのインポートが行えます（DBUnit）
1. DBUnitで定義されているDataBaseOperationが利用可能です

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "ImportRdbDataRunner"
    rdbConnectConfigRef: "RDBに対する接続先定義の参照ID" # (1)
    dataSetType: "(xml|excel)" # (2)
    operation: "(insert|clean_insert|update|refresh)" # (3)
    bind: "変数バインドを行うかどうかのフラグ" # (4)
    value: "インポートするDataSetのパス（相対）" # (5)
```

1. RDBへの接続先の設定を行っている`Configuration`の参照IDを指定します。
2. DataSetの種類を指定します。DataSetの詳細は後述します。
3. DataSetをインポートする際のロジックを指定します。Operationの詳細は後述します。
4. DataSetの中に[Variable](/pages/specification/variables.md)を利用するような変数参照が存在する場合に、バインドを行うかを指定できます。
5. インポートするDataSetのファイルのパスを相対パスで指定します。相対パスの基準ディレクトリは対象のシナリオファイルが配置している場所となります。
対象のシナリオファイルはパーツ定義に配置している場合はパーツのYAMLが配置している場所を指します。

#### DataSet
DataSetとは、RDBのデータ構造を表したもので、DataSetには、CSV、XML、Excelの形式が選べます。  
本コマンドが利用するDataSetとはすべて[DBUnit](http://dbunit.sourceforge.net/)のDataSetを指します。  
現状では、CSVには対応ができておりません。    
  
------

### 「ExportRdbDataRunner」

#### Function

#### Structure

------

### 「ExecuteRdbQueryRunner」

#### Function

#### Structure

------

### 「ExecuteRdbScriptRunner」

#### Function

#### Structure

------

# RDB Configuration

RDB（Relational DataBase）関連の設定を提供します。

### 「RDB接続先定義」

#### Function
1. RDBへの接続先の定義を行います。

#### Structure

```yaml
configurations:
  - configuration: "RdbConnectionConfiguration"
    id: "設定のID"
    summary : "設定の概要（任意）"
    description : "設定の詳細（任意）"
    driverClassName: "ドライバのクラス名"
    url: "JDBC接続URL"
    username: "接続するユーザー"
    password: "接続するパスワード"
    schema: "接続するスキーマ"
```

基本的に本設定は環境依存値を多分に含むため、Profileによるバインドを推奨します。
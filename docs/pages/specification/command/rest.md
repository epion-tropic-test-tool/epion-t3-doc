# REST Command

REST-API関連のコマンドを提供します。

|機能名|概要|Assert|Evidence|
|:---|:---|:---|:---|
|[ExecuteRestApi](#ExecuteRestApi)|REST-APIを実行します。|-|Yes|
|[StoreJsonElement](#StoreJsonElement)|REST-APIの実行結果のBodyから要素を取得して変数に格納します|-|-|
|[AssertHttpStatus](#AssertHttpStatus)|REST-APIの実行結果のHTTPステータスコードをアサートします。|Yes|-|
|[AssertHttpHeader](#AssertHttpHeader)|REST-APIの実行結果のHTTPヘッダをアサートします。|Yes|-|

------

### ExecuteRestApi

#### Functions

1. REST-APIを呼び出せる
2. レスポンスをエビデンスとして保持する

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "ExecuteRestApi"
    connectTimeout: "接続タイムアウトをミリ秒で指定（任意）" # (1)
    readTimeout: "読込タイムアウトをミリ秒で指定（任意）" # (2)
    bodyEncoding: "リクエストボディのエンコーディング指定（任意）" # (3)
    request:
      method: "(get|post|put|delete)"
      headers:
        "Content-Type" : "application/json" # (例)
        "Accept" : "application/json" # (例)
      queryParameters:
        - name: "name"
          value: "value"
          needUrlEncoded: true
      targetUrl: "${schema}://${host}:${port}/todos"
      body: | # (例)
        {
          "title" : "タイトル",
          "description" : "詳細"
        }
```

1. REST通信の接続タイムアウトを発生させる時間をミリ秒で指定します。  
デフォルト値は3000ms（3s）となっています。
2. REST通信の読込タイムアウトを発生させる時間をミリ秒で指定します。  
デフォルト値は3000ms（3s）となっています。
3. リクエストボディのエンコーディング指定を行えます。  
デフォルト値は存在しないため、Javaの起動時のエンコーディングが採用されます。（通常UTF-8）  
また、このオプションは`POST`、`PUT`、`PATCH`のみに適用されます。


#### Evidence

エビデンスは、オブジェクトエビデンスとして保持されます。  
現状の機能ではファイルには出力しませんので、ご注意ください。  
エビデンスは以下のクラスの形式で保持します。

```java
com.zomu.t.epion.tropic.test.tool.rest.bean.Response
```

↑さすがに、JSON形式で後で書く・・・

#### Report

本コマンドの実行に関しては、独自レポートが出力されます。  
レポートのサンプルイメージは以下です。  
（随時開発によってドキュメント記載当時とレポート内容が変更になっている場合があります）

![ExecuteRestApi_report](pages/specification/command/images/ExecuteRestApi_report.png)


#### Example

サンプルシナリオは以下を参照してください。

```
scenarios-todos-refer-001
```

------

### StoreJsonElement

レスポンスのボディを解析して値を抽出し、Variableへ登録するための機能です。  
REST-APIを連続して実行するようなシナリオにおいて、レスポンスの値を引き継ぐ必要がある場合に利用します。

#### Functions

1. REST-APIの実行結果のレスポンスボディ（JSONに限る）から値を[JSONPath](https://github.com/json-path/JsonPath)で抽出して変数に登録する。

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "StoreJsonElement"
    target: "ExecuteRestApiを実行したFlowID" # (1)
    value: "登録する変数名" # (2)
    jsonPath: "抽出するJSONPath" # (3)
```
1. `ExecuteRestApi`を実行した`FlowID`を指定します。
1. 登録する変数名は、`スコープ`.`名称`で指定します。（例：scenario.hoge）
1. [JSONPath](https://github.com/json-path/JsonPath)にて指定を行います。

------

### AssertHttpStatus

#### Functions

1. REST-APIの実行結果のHTTPステータスコードを確認すします。

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "AssertHttpStatus"
    target: "ExecuteRestApiを実行したFlowID" # (1)
    value: 200 # (2)
```
1. `ExecuteRestApi`を実行した`FlowID`を指定します。
2. 期待値としてのHTTPステータスコードを数値で設定します。

------

### AssertHttpHeader

#### Functions

1. REST-APIの実行結果のHTTPヘッダを確認します。

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "AssertHttpHeader"
    target: "ExecuteRestApiを実行したFlowID" # (1)
    header: "HTTPヘッダ名" # (2)
    value: "期待値" # (3)
```
1. `ExecuteRestApi`を実行した`FlowID`を指定します。
1. 確認対象のHTTPヘッダ名を指定します。
2. 期待値としてのHTTPヘッダの値を設定します。  
設定値が複数項目の場合、セミコロン区切りで指定してください。
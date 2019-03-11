# REST Command

### 「ExecuteRestApi」

#### Functions

1. REST-APIを呼び出せる
1. レスポンスをエビデンスとして保持する

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "ExecuteRestApi"
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

#### Evidence

エビデンスは、オブジェクトエビデンスとして保持されます。  
現状の機能ではファイルには出力しませんので、ご注意ください。  
エビデンスは以下のクラスの形式で保持します。

```java
com.zomu.t.epion.tropic.test.tool.rest.bean.Response
```

↑さすがに、JSON形式で後で書く・・・

#### Example

サンプルシナリオは以下を参照してください。

```
scenarios-todos-refer-001
```
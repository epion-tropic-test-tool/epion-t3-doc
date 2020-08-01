# Assert

`ETTT` は `Assert` という概念があります。
`Assert` はシステムテストにおける一般的な確認・アサートと同義です。
ただし、`ETTT` 特有の確認・アサートのお作法が存在するため説明を行います。

## Assert and Evidence

`Assert` はテストにおける確認を行う動作になります。  
`ETTT` ではテスト時にある確認を行うには確認する対象の `Evidence(証跡)` がなければ成り立たないものだと考えています。  
これは手作業であろうが自動であろうが試験を行う場合の必然であるからです。  
従って、`Assert`を行うためには確認対象の `Evidence(証跡)` を用意することを必須としています。  

`ETTT`のコマンドには、`Assert` と `Evidence` については特殊なコマンドとして扱っており、[Command Index](/pages/specification/command/index) においてもどのコマンドが`Assert`と`Evidence`であるかを明記しています。
`Assert`系のコマンドでは全て`Evidence`を参照することになりますが、その参照方法にも基本方針があります。その方法は`Evidence`を取得した`FlowID`を指定することです。

#### 例：テキストファイルの場合
ログの`Assert`を行う場合には、対象のログの取得を`Evidence`取得系のコマンドを用いて取得します。その後、`Assert`系のコマンドではEvidenceを取得した`FlowID`を指定して対象のログを参照します。

![assert-and-evidence](/media/assert-evidence-image.png)

この流れを支持するためのシナリオファイルの例としては、以下のようになります。
最小サンプルとなるため説明等の要素は全て排除しています。

```yaml
t3: '1.0'
info:
  id: "assert-evidence-sample"
  version: 1.0.0
flows:
  - id: "flow-001"
    type: "CommandExecute"
    ref: "assert-evidence-sample@assert-evidence-command-001"
  - id: "flow-002"
    summary: "ログのアサート"
    type: "CommandExecute"
    ref: "assert-evidence-sample@assert-evidence-command-002"
commands:
  - id: "assert-evidence-command-001"
    command: "FileGet"
    target: "/var/log/sample.log"
  - id: "assert-evidence-command-002"
    command: "AssertExistsStringInText"
    target: "flow-001" # (1)
    value: "TodosController#create stared!"
```

1. sample.logをエビデンスとして取得したFlowIDである`flow-001`を指定しています。

このように`Assert`を行う際には、必ず `Evidence` が存在している状況となるのです。  
必ず `Evidence` がある状況にしておくことによって、仮にテストの信憑性が不十分である場合なども手作業で確認を行うことができます。  
自動テストにおいて、テストが正しく実施されているかを追求できることはとても重要です。
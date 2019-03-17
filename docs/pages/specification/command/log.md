# Log Command

ログ関連のコマンドを提供します。

|機能名|概要|アサート|エビデンス|
|:---|:---|:---|:---|
|[LogExtractDuringTime](#LogExtractDuringTime)|SCPによるファイル取得|-|Yes|

------

### LogExtractDuringTime

#### Function

1. ログの切り出しを行います。
1. 既に取得したログに対して行います。
1. 指定したFlowIDの処理時間の間のログを切り出して上書き保存します。
1. この機能はテスト環境と`ETTT`が稼働しているサーバの時刻がNTPサーバ等で同期されていないと正しく動かない場合があります。
1. 日時の解決は、ログのヘッダ部に出力される日時フォーマットを正規表現で解決します。
1. 前後バッファ時間を設ける機能があります。ある程度余裕を持ってログの抽出が行えますが正確性が落ちます。 

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "LogExtractDuringTime"
    target: "抽出対象のログ取得を行ったFlowIDを指定" # (1)
    targetFlow: "抽出対象の処理を行ったFlowIDを指定" # (2)
    extractPattern: "^([0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2}:[0-9]{2}\\.[0-9]{3}).+" # (3)
    group: 1
    datePattern: "yyyy-MM-dd HH:mm:ss.SSS" # (4)
    encoding: "UTF-8"
    roundBuffer: 0
    roundBufferTimeUnit: "secound"
```

1. 抽出対象のログ取得を行ったFlowIDを指定します。
2. リモートに配置しているパスを指定します。
3. 

------
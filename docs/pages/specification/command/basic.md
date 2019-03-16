# Basic Command

基本的な動作を行うコマンドを提供します。

|Name|Summary|Assert|Evidence|
|:---|:---|:---|:---|
|[SetVariable](#SetVariable)|変数を設定します|-|-|
|[RemoveVariable](#RemoveVariable)|変数を設定します|-|-|
|[FileGet](#FileGet)|ファイルシステムにてファイルを取得します|-|Yes|
|[AssertExistsStringInText](#AssertExistsStringInText)|テキストファイル中に指定した文字列を含んでいることを確認|Yes|-|
|[AssertNotExistsStringInText](#AssertNotExistsStringInText)|テキストファイル中に指定した文字列を含んでいないことを確認|Yes|-|

------

### SetVariable

#### Functions

- 任意のスコープ変数に対して値を設定します。
- 設定する値自体も変数のバインドが利用できますが、基本的には固定値で利用されることを想定してます。

#### Structure

```yaml
commands : 
  - id : コマンドのID
    command : SetVariable
    summary : コマンドの概要（任意）
    description : コマンドの詳細（任意）
    target : 変数名を指定 # (1)
    value : 値を指定 # (2)
```

1. 変数名は「スコープ.変数名」の形式で指定します。「global.hoge」であればグローバルスコープにhogeという変数名で値を定義することになります。
2. 値は基本的に固定値を指定します。「あああ」と定義した場合は文字列で「あああ」と登録されます。

------

### RemoveVariable

#### Functions

- 任意のスコープ変数に設定してある変数を削除します

#### Structure

```yaml
commands :
  - id : コマンドのID
    command : SetVariable
    summary : コマンドの概要（任意）
    description : コマンドの詳細（任意）
    target : 変数名を指定 # (1)
```

1. 変数名は「スコープ.変数名」の形式で指定します。「global.hoge」であればグローバルスコープにhogeという変数名で値を定義することになります。

------

### FileGet

ファイルシステムを利用したファイル取得機能です。
ローカルテスト時等でしか役に立たないかもしれないですが、初期ツール導入時にご活用ください。

#### Functions

- ファイルシステムを利用してファイルを取得します
- 取得したファイルはエビデンスとして保存します

#### Structure

```yaml
commands :
  - id : コマンドのID
    command : SetVariable
    summary : コマンドの概要（任意）
    command: "FileGet"
    target: "ファイルパス(絶対パス)" # (1)
```

1. 取得するファイルパスを絶対パスにて指定します。変数のバインドにも対応していますので、絶対パスのベースパス等の解決にも利用可能です。

------

### AssertExistsStringInText

#### Functions

- 指定したテキストファイル中に任意の文字列が含まれていることを確認します

#### Structure

```yaml
commands :
  - id : コマンドのID
    command : AssertExistsStringInText
    summary : コマンドの概要（任意）
    description : コマンドの詳細（任意）
    target : 対象のFlowIDを指定 # (1)
```

1. 変数名は「スコープ.変数名」の形式で指定します。「global.hoge」であればグローバルスコープにhogeという変数名で値を定義することになります。

------

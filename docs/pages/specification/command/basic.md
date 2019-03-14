# Basic Command

基本的な動作を行うコマンドを提供します。

### 「 SetVariable 」

#### Functions

- 任意のスコープ変数に対して値を設定します。
- 設定する値自体も変数のバインドが利用できますが、基本的には固定値で利用されることを想定してます。

#### Structure

```yaml
commands : 
  - id : コマンドのID # (1)
    command : SetVariable
    summary : コマンドの概要（任意）
    description : コマンドの詳細（任意）
    target : 変数名を指定 # (2)
    value : 値を指定 # (3)
```

1.
1. 変数名は「スコープ.変数名」の形式で指定します。「global.hoge」であればグローバルスコープにhogeという変数名で値を定義することになります。
1. 値は基本的に固定値を指定します。「あああ」と定義した場合は文字列で「あああ」と登録されます。

------

### 「 ConsoleInput 」
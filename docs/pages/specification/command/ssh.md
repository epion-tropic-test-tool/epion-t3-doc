# SSH Command

SSH関連のコマンドを提供します。  
SSHには接続先定義等の`Configuration`も存在しますが、ページの下部に記載をしています。

|機能名|概要|アサート|エビデンス|
|:---|:---|:---|:---|
|[ScpGet](#ScpGet)|SCPによるファイル取得|-|Yes|
|[ScpPut](#ScpPut)|SCPによるファイル転送|-|-|

------

### ScpGet

#### Function

1. SCPによるファイル取得を行います。リモート→ローカルです。
1. ユーザー/パスワードでの認証に対応します。
1. PEMファイルでの認証に対応します。

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "ScpGet"
    sshConnectConfigRef: "SSHによる接続先定義の参照ID" # (1)
    remoteFile: "" # (2)
```

1. SSHによる接続先の設定を行っている`Configuration`の参照IDを指定します。
1. リモートに配置しているパスを指定します。

------

### ScpPut


#### Function

1. SCPによるファイル転送を行います。ローカル→リモートです。
1. ユーザー/パスワードでの認証に対応します。
1. PEMファイルでの認証に対応します。

#### Structure

```yaml
commands:
  - id: "コマンドのID"
    summary : "コマンドの概要（任意）"
    description : "コマンドの詳細（任意）"
    command: "ScpPut"
    sshConnectConfigRef: "SSHによる接続先定義の参照ID" # (1)
    remoteDir: "転送するリモートディレクトリのパス" # (2)
    localFile: "転送するローカルファイルのパス（相対）" # (3)
```

1. SSHによる接続先の設定を行っている`Configuration`の参照IDを指定します。
1. 転送するリモートのディレクトリのパスを指定します。
1. 転送するローカルファイルのパスを相対パスで指定します。相対パスの基準ディレクトリは対象のシナリオファイルが配置している場所となります。
対象のシナリオファイルはパーツ定義に配置している場合はパーツのYAMLが配置している場所を指します。

------

# SSH Configurations

### 「SSH接続先定義」

#### Function
1. SSHによる接続先の定義を行います。

#### Structure

```yaml
configurations:
  - configuration: "SshConnectionConfiguration"
    id: "設定のID"
    summary : "設定の概要（任意）"
    description : "設定の詳細（任意）"
    host: "ホスト名"
    port: ポート
    user: "ユーザー"
    password: "パスワード"
    pemFilePath: "鍵ファイルパス（相対）"
```
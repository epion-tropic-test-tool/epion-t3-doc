# Customization Spec

カスタム機能（Flow/Command）を作成するためには、
カスタム機能を定義する専用のYAMLファイル（以降 設計YAML）にて設計を行う必要があります。
この設計YAMLは `ETTT` を動かす際にも利用されるためとても重要なファイルになります。
また、カスタマイズする際に作成する必要のある様々な成果物の自動生成を行うための定義ファイルにもなります。

## What's use Spec YAML for

この設計YAMLは記載するのにとても面倒です。  
ですが、この設計YAMLが何に利用されるかを正しく理解してもらえれば、多少記載へのモチベーションが保てると思っています。

- 自動生成ツールへのInput
  - メッセージ定義のプロパティファイルの自動生成（_messages.properties）（i18n対応）
  - メッセージ定義のEnum定義Javaクラスの自動生成
  - Modelクラスのスケルトン実装の自動生成
  - Runnerクラスのスケルトン実装の自動生成
- 試験項目書の出力時の文言に利用
  - `ETTT` は試験項目の自動生成に今後対応しますが、その際の文言定義に利用します。

## Spec YAML Structure

設計YAMLの基本構成は以下となっています。

```yaml
t3: 1.0
info:
  name: {カスタム機能名}  # (1)
  customPackage: {カスタムパッケージ}  # (2)
  summary:  # (3)
    - lang: ja_JP  # (4)
      contents: "カスタム機能の概要を記載"
  description:  # (5)
    - lang: ja_JP
      contents: "カスタム機能の詳細を記載"
commands:
  - id: {コマンドID}  # (6)
    summary:
      - lang: ja_JP
        contents: "コマンドの概要を記載"
    testItem:  # (7)
      - order: 1
        summary:
          - lang: ja_JP
            contents: "試験項目書の文言"
    function:  # (8)
      - order: 1
        summary:
          - lang: ja_JP
            contents: "コマンドの機能説明の文言"
    structure:  # (9)
      - order: 1
        name: id
        required: true
        type: string
        summary:
          - lang: ja_JP
            contents: コマンドのID
```

1. カスタム機能名を定義します。  
この値はカスタム機能の代表的な名前でありパッケージ名等にも利用されます。  
例を出すと、`core`、`basic`、`log`と行ったものになります。機能名を簡易的に表してください。　
1. カスタムパッケージは、カスタム機能を実装するJavaパッケージを指定してください。
1. カスタム機能の概要を定義します。
1. 国際化（i18n）対応を行なっていますので、ロケール名を記載してください。  
Javaの [Locale](https://docs.oracle.com/javase/jp/8/docs/api/java/util/Locale.html) で扱えるものを指定してください。
1. カステム機能の詳細を定義します。
1. コマンドのIDを定義します。コマンドIDは機能を呼び出す時に利用するコマンドの代表名です。
1. 試験項目書出力時（未実装）に利用する文言です。コマンドがどのような動作を行うものであるかを記載します。
1. コマンドの機能の説明を記載します。コマンドに対して複数説明を記載できます。
1. コマンドの動作をユーザが指定するYAMLと１対１で紐づくModelを定義します。  
詳細は後述します。

#### 文言の表現について
文言の表現は、国際化（i18n）対応を行なっているため全て [Locale](https://docs.oracle.com/javase/jp/8/docs/api/java/util/Locale.html) とセットで定義します。
また、順序性を担保したいものについては、 `order` によって順序を整数値によって決めることができます。

```yaml
summary | description など
 - lang: ja_JP
   contents: これはメッセージです。
 - lang: en_US
   contents: This is Messages.
```

### Structure Details


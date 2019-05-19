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

設計YAMLは以下の構成となっています。

```yaml
t3: 1.0
info:
  name: selenium
  customPackage: com.epion_t3.selenium
  summary:
    - lang: ja_JP
      contents: "Selenium関連のコマンドを提供します。"
  description:
    - lang: ja_JP
      contents: "Seleniumを利用したWebブラウザテストを実行するための機能を提供します。本機能は、`Firefox 47` 以降にのみ対応します。geckodriverが必要となりますのでご注意ください。"
commands:
  - id: StartLocalWD
    summary:
      - lang: ja_JP
        contents: "SeleniumのWebDriverを起動します。"
    testItem:
      - order: 1
        summary:
          - lang: ja_JP
            contents: "Webブラウザ「${browser}」を開始します。"

```
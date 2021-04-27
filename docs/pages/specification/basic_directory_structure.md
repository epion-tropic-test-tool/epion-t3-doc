# Basic Directory Structure
`ETTT`は全てを１つのYAMLで表すこともできますが、  
シナリオのメンテナンス性や複数人で作成する場合の運用を考慮する場合、  
細かく用途によってファイルを分割・配置することを推奨します。  
また、これらのシナリオはGitやSubversionによるバージョン管理を行うことが重要です。

## Best Practice of Directory Structure

`ETTT` では以下のディレクトリ構成とファイル配置にてテストシナリオを作成していただくとスムーズにメンテナンスが可能になると考えています。

```
root
 |
 |-- configurations                              // 各種機能の設定についての定義を格納するディレクトリ
 |     |
 |     `-- et3_{Configurations-Unique-ID}.yaml   // 各設定を記載するファイル
 |
 |-- customs                                     // 利用するカスタム機能の定義
 |     |
 |     `-- et3_custom.yaml
 |
 |-- parts                                       // 複数のテストシナリオで使うコマンド定義をパーツ化し格納するディレクトリ
 |     |
 |     `-- {Parts-Unique-ID}
 |           |
 |           |-- et3_{Parts-Unique-ID}.yaml      // 共通的に利用するようなコマンドの定義
 |           |
 |           |-- data
 |           |     |-- csv_data.csv
 |           |     |-- excel_data.xlsx
 |           |     `-- etc...
 |           |
 |           `-- assert
 |                 |-- csv_data.csv
 |                 |-- excel_data.xlsx
 |                 `-- etc...
 |
 |-- profiles                                    // 環境毎に異なる値の定義を格納するディレクトリ
 |     |
 |     `-- et3_{ProfileName}.yaml                // 環境毎の具体的な値の定義
 |
 |-- scenarios
 |     |
 |     `-- {Scenario-Unique-ID}                  // テストシナリオ
 |           |
 |           |-- t3_{Scenario-Unique-ID}.yaml
 |           |
 |           |-- data
 |           |     |-- csv_data.csv
 |           |     |-- excel_data.xlsx
 |           |     `-- etc...
 |           |
 |           `-- assert
 |                 |-- csv_data.csv
 |                 |-- excel_data.xlsx
 |                 `-- etc...
 |
 `-- suite
       |
       `-- et3_{Suites-Unique-ID}.yaml          // 複数のテストシナリオを実行するための定義

```

### Configurations
`configurations`は、カスタム機能の設定定義を行うためだけのYAMLを格納するものです。
例を挙げますと、SSHやRDBへの接続先等になります。

### Customs
`customs`は、カスタム機能の利用定義を行うためだけのYAMLを格納するものです。
明示的に切り出しておくことによって、このシナリオ群はどのようなカスタム機能を利用するのかが明快になりメンテナンス性が高まります。

### Parts
複数のテストシナリオで同じようなコマンドの定義を行っている場合は、`parts` に切り出して参照することで定義ファイルの記述量を減らすことができます。    
汎用的なコマンド定義を行い文字通りパーツとして様々なテストシナリオから呼び出すようにします。

### Profiles
`profiles`は、プロファイルの定義を行うためだけのYAMLを格納するものです。
プロファイル機能は複数環境にまたがってシナリオを利用する場合には必須となるもので、試験設計としても重要な要素です。
プロファイル毎にファイルを分けたり、１つのファイルで全てのプロファイルを管理する事ができます。
差分比較の観点から言いますと、プロファイル毎にファイルを分割しておくことをオススメします。

### Scenarios
`scenarios`とは、実際に実行するシナリオを定義したYAMLを配置する場所です。
また、シナリオに特化したデータやアサーションに必要なデータを格納する場所です。

### Suites
`suites`とは、シナリオをさらに複数まとめて実行するための定義を行ったYAMLを配置する場所です。

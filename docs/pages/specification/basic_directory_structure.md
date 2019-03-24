# Basic Directory Structure
`ETTT`は様々な１つのYAMLで全てを表すこともできますが、  
シナリオのメンテナンス性や複数人で作成する場合の運用を考慮する場合、  
細かく用途によってファイルを分割・配置することを推奨します。  
また、これらのシナリオはGitやSubversionによるバージョン管理を行うことが重要です。

## Best Practice of Directory Structure
```
root
 |
 |-- configurations
 |     |
 |     `-- t3_{Configurations-Unique-ID}.yaml
 |
 |-- customs
 |     |
 |     `-- t3_custom.yaml
 |
 |-- parts
 |     |
 |     `-- {Parts-Unique-ID}
 |           |
 |           |-- t3_{Parts-Unique-ID}.yaml
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
 |-- profiles
 |     |
 |     `-- t3_{ProfileName}.yaml
 |
 |-- scenarios
 |     |
 |     `-- {Scenario-Unique-ID}
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
       `-- t3_{Suites-Unique-ID}.yaml

```

### Configurations
`configurations`は、カスタム機能の設定定義を行うためだけのYAMLを格納するものです。
例を挙げますと、SSHやRDBへの接続先等になります。

### Customs
`customs`は、カスタム機能の利用定義を行うためだけのYAMLを格納するものです。
明示的に切り出しておくことによって、このシナリオ群はどのようなカスタム機能を利用するのかが明快になりメンテナンス性が高まります。

### Parts
`parts`とは、汎用的なコマンド定義を行い文字通りパーツとして様々なシナリオから呼び出すことを目的として切り出したものです。

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

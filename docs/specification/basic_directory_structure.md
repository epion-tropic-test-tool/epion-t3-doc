# Basic Directory Structure
`ETTT`は様々な１つのYAMLで全てを表すこともできますが、  
シナリオのメンテナンス性や複数人で作成する場合の運用を考慮する場合、  
細かく用途によってファイルを分割・配置することを推奨します。  
また、これらのシナリオはGitやSubversionによるバージョン管理を行うことが重要です。

```
root
 |
 |-- parts　# (1)
 |     |
 |     `-- {Parts-Unique-ID}
 |           |
 |           |-- t3-{Parts-Unique-ID}.yaml
 |           |
 |           |-- data
 |           |     |-- excel_data.xlsx
 |           |     `-- etc...
 |           |
 |           `-- assert
 |                 |-- excel_data.xlsx
 |                 `-- etc...
 |
 |-- scenarios　# (1)
 |     |
 |     `-- {Scenario-Unique-ID}
 |           |
 |           |-- t3-{Scenario-Unique-ID}.yaml
 |           |
 |           |-- data
 |           |     |-- excel_data.xlsx
 |           |     `-- etc...
 |           |
 |           `-- assert
 |                 |-- excel_data.xlsx
 |                 `-- etc...
 |
 |-- profiles　# (1)
 |     |
 |     `-- t3-{ProfileName}.yaml
 |
 `-- customs　# (1)
       |
       `-- t3-custom.yaml

```

---
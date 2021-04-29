# Best Practices

## Summary

`ETTT` を利用してテストを行うためのベストプラクティスと考えている導入の流れ・シナリオの構築方法を説明します。  
このプラクティスは、WebシステムのWebブラウザを用いたテストの自動化ではなくバッチや、サーバレスを対象として記載しているものになります。

## 作業の流れ

1. 自動化したい動作・オペレーションの検討
1. コマンドの充足を洗い出し
1. 拡張機能の実装
1. 自動化する動作・オペレーションとその順序の整理
1. 標準化シナリオの作成
1. 個別シナリオの作成
1. Suitesとしての管理

## 自動化したい動作・オペレーションの検討

まずは、自動化したいテストのオペレーションを洗い出します。  
この作業はテストを4つのPhaseで分割して考えることをオススメしています。 
これは [Four-Phase Test](http://xunitpatterns.com/Four%20Phase%20Test.html) と呼ばれる手法を参考に行います。

|#|Phase|Description|
|:---|:---|:---|
|1|Setup|事前準備、前処理|
|2|Exercise|実行|
|3|Verify|検証、アサーション|
|4|Teardown|事後処理、後処理|

テストのオペレーションを上記のPhaseに分類して整理します。  
例としては以下のようなものが挙げられます。

|Operation|Description|Phase|
|:---|:---|:---|
|RDBレコード削除|RDBのレコードを削除する|Setup / Teardown|
|RDBレコード登録|RDBのレコードを登録する|Setup / Teardown|
|ディレクトリクリア|ファイルシステム上のディレクトリ内をクリア|Setup / Teardown|
|ファイル配置|ファイルシステム上のディレクトリへファイルを配置|Setup / Teardown|
|アプリケーション実行|試験対象のアプリケーションの実行|Exercise|
|ログ取得|試験対象のアプリケーションの出力ログを取得|Exercise|
|RDBレコード取得|RDBのレコードを取得する|Exercise|
|結果コード確認|実行したアプリケーションの終了コードを確認|Verify|
|RDBレコード確認|RDBのレコードを確認|Verify|
|ログ確認|ログを確認|Verify|

上記にてSetupとTeardownを同じオペレーションとして整理していますが、  
これはテストの組み方によって両方のPhaseで実行があり得るためです。
全てのテストケースの先頭にクリアや削除処理を実施する場合と、  
全てのテストケースの最後にクリアや削除処理を実施する場合などがあります。

また、[Assert](/pages/specification/assert.md) でも説明した通りエビデンスの取得は `Verify` の前に実施する必要があるため、 `Exercise` として扱っています。


## コマンドの充足を洗い出し / 拡張機能の実装

`ETTT` が提供している 拡張機能は [Available Functions - Index](/pages/specification/functions/index.md) に一覧のリンクを掲載しています。  
必要な機能が存在しない場合には、 [How to Customize - Overview](/pages/customize/overview.md) を参照して独自の拡張機能を作成するなど行います。


## 自動化する動作・オペレーションとその順序の整理






# Best Practices

## Summary

`ETTT` を利用してテストを行うためのベストプラクティスと考えている導入の流れ・シナリオの構築方法を説明します。  
このプラクティスは、WebシステムのWebブラウザを用いたテストの自動化ではなくバッチや、サーバレスを対象として記載しているものになります。

## 作業の流れ

1. 自動化したい動作・オペレーションの検討
1. コマンドの充足を洗い出し
1. 拡張機能の実装
1. 自動化する動作・オペレーションとその順序の整理
1. シナリオの全体構成の整理
1. シナリオパーツの作成
1. 標準シナリオの作成
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

必要な機能が出揃った状態で、自動化する動作・オペレーションとその実行順序を整理します。  
テスト対象によって動作・オペレーションの順序がことなることもあると思いますので、以下のような表で管理することをオススメしています。

|OperationId|Command|Summary|Target - A|Target - B|
|:---|:---|:---|:---|:---|
|OPE-STP-010|[ExecuteRdbScript](https://github.com/epion-tropic-test-tool/epion-t3-rdb/blob/master/rdb_spec_ja_JP.md#ExecuteRdbScript)|RDBのレコードを削除する|1|1|
|OPE-STP-020|[ExecuteRdbScript](https://github.com/epion-tropic-test-tool/epion-t3-rdb/blob/master/rdb_spec_ja_JP.md#ExecuteRdbScript)|RDBのレコードを登録する|2|2|
|OPE-STP-030||ディレクトリクリア|3|3|
|OPE-STP-040|[FileCopy](https://github.com/epion-tropic-test-tool/epion-t3-basic/blob/master/basic_spec_ja_JP.md#FileCopy)|ファイル配置|4|4|
|OPE-EXP-010|[ExecuteLocalCommand](https://github.com/epion-tropic-test-tool/epion-t3-basic/blob/master/basic_spec_ja_JP.md#executelocalcommand)|バッチ実行|5|5|
|OPE-STP-050|[ExecuteLocalCommand](https://github.com/epion-tropic-test-tool/epion-t3-basic/blob/master/basic_spec_ja_JP.md#executelocalcommand)|ログを取得|-|6|
|OPE-STP-060|[ExportRdbData](https://github.com/epion-tropic-test-tool/epion-t3-rdb/blob/master/rdb_spec_ja_JP.md#ExportRdbData)|RDBのレコード抽出|6|7|
|OPE-VRP-010|[AssertRdbData](https://github.com/epion-tropic-test-tool/epion-t3-rdb/blob/master/rdb_spec_ja_JP.md#AssertRdbData)|RDBのレコードを確認|7|8|
|OPE-VRP-020|[AssertRdbData](https://github.com/epion-tropic-test-tool/epion-t3-rdb/blob/master/rdb_spec_ja_JP.md#AssertRdbData)|ログを確認|-|9|

## シナリオの全体構成の整理

シナリオは複数のYAMLで構成する方法をオススメすることを、[Basic Directory Structure](/pages/specification/basic_directory_structure.md) で説明しています。  
この方式に沿ってシナリオの全体構成を整理します。

### 共通的に使えるコマンドのパーツ化

複数のテスト対象が存在したとしても、全てのテストが行う処理であったり、何割かのテストが共通的に使用するようなコマンドが出てくることが予測されます。  
そうしたコマンドを共通的に用意しておいて、各シナリオには手を加えなくとも利用できるようにしておくことがテストシナリオの作成やメンテナンスにかかるコストを低減させるために必須です。



## シナリオパーツ作成

コマンド、オペレーションの準備ができたら、シナリオの標準化を目的としたシナリオパーツの作成を行います。
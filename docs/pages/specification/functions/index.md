# Extention Functions Index

 `ETTT` にて利用可能な 拡張機能 ( Flow / Command / Configuration ) の紹介を行います。  
拡張機能はサポートしたい機能毎に細かく分割されており、シナリオの中で必要に応じて取捨選別して利用できる構成になっています。  
詳細は  [Basic Structure - Customs](pages/specification/basic_structure?id=customs) を参照して下さい。  
本ドキュメントに記載のあるものは `ETTT` が公式で用意しているものになります。  
この一覧の中に、必要とする機能が存在しない場合にはユーザー自身で拡張機能を用意する事が可能です。  
拡張機能の作成は [How to Customize - Overview](/pages/customize/overview.md) を参照して下さい。  


一覧のリンクはそれぞれの拡張機能の詳細説明ページに遷移します。  


|GroupId|ライブラリ名(ArtifactId)|パッケージ|概要|
|:---|:---|:---|:---|
|com.epion_t3|[epion-t3-basic](https://github.com/epion-tropic-test-tool/epion-t3-basic/blob/master/basic_spec.md ':include :type=markdown')|com.epion_t3.basic|基本機能|
|com.epion_t3|[epion-t3-log](https://github.com/epion-tropic-test-tool/epion-t3-log/blob/master/log_spec.md)|com.epion_t3.log|ログ関連の機能|
|com.epion_t3|[epion-t3-rest](https://github.com/epion-tropic-test-tool/epion-t3-rest/blob/master/rest_spec.md)|com.epion_t3.rest|REST関連の機能|
|com.epion_t3|[epion-t3-rdb](https://github.com/epion-tropic-test-tool/epion-t3-rdb/blob/master/rdb_spec_ja_JP.md)|com.epion_t3.rdb|RDBへのアクセスやアサーションを行う機能|
|com.epion_t3|[epion-t3-ssh](https://github.com/epion-tropic-test-tool/epion-t3-ssh/blob/master/ssh_spec.md)|com.epion_t3.ssh|SSH関連の機能|
|com.epion_t3|[epion-t3-selenium](https://github.com/epion-tropic-test-tool/epion-t3-selenium/blob/master/selenium_spec.md)|com.epion_t3.selenium|Selenium関連の機能|
|com.epion_t3|[epion-t3-aws-core](https://github.com/epion-tropic-test-tool/epion-t3-aws-core/blob/master/aws-core_spec_ja_JP.md)|com.epion_t3.aws.core|AWSアクセスの基礎機能|
|com.epion_t3|[epion-t3-aws-s3](https://github.com/epion-tropic-test-tool/epion-t3-aws-s3/blob/master/aws-s3_spec_ja_JP.md)|com.epion_t3.aws.s3|AWS S3アクセス関連の機能|
|com.epion_t3|[epion-t3-aws-ecs](https://github.com/epion-tropic-test-tool/epion-t3-aws-ecs/blob/master/aws-ecs_spec_ja_JP.md)|com.epion_t3.aws.ecs|AWS ECSアクセス関連の機能|
|com.epion_t3|[epion-t3-aws-cloudwatch-logs](https://github.com/epion-tropic-test-tool/epion-t3-aws-cloudwatch-logs/blob/master/aws-cwl_spec_ja_JP.md)|com.epion_t3.aws.cwl|AWS Cloud Watch Logsアクセス関連の機能|
|com.epion_t3|[epion-t3-aws-sqs](https://github.com/epion-tropic-test-tool/epion-t3-aws-sqs/blob/master/aws-sqs_spec_ja_JP.md)|com.epion_t3.aws.sqs|AWS SQSアクセス関連の機能|

### GroupId / ArtifactId について
[How To Use - Project](pages/use/build.md) にて説明していますが、  
それぞれの機能を利用するためにビルドシステムの設定ファイル（`build.gradle` or `pom.xml`）に対してライブラリの依存関係の定義を行います。  
その際に指定するのは、上記表の `GroupId` と `ArtifactId` およびそれぞれのライブラリの `Version` が必要になります。

### パッケージについて
[Basic Structure - Customs](pages/specification/basic_structure?id=customs) にて説明していますが、  
それぞれの機能を利用するためにシナリオに対して `パッケージ` を指定します。  
その際に指定するのは、上記表の `パッケージ` が必要になります。
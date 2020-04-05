# Command Index

このページでは、 `ETTT` にて利用可能なコマンドの紹介を行います。  
コマンドは機能分類毎に細かく分割されており、必要に応じて利用できるようになっています。  
また、本ドキュメントに記載のあるものは `ETTT` が公式で用意しているものになります。  
そのため、ユーザが独自カスタマイズしたものは掲載されていません。

|GroupId|ライブラリ名(ArtifactId)|パッケージ|概要|
|:---|:---|:---|:---|
|com.epion_t3|[epion-t3-basic](https://github.com/epion-tropic-test-tool/epion-t3-basic/blob/master/basic_spec.md ':include :type=markdown')|com.epion_t3.basic|基本機能|
|com.epion_t3|[epion-t3-log](https://github.com/epion-tropic-test-tool/epion-t3-log/blob/master/log_spec.md)|com.epion_t3.log|ログ関連の機能|
|com.epion_t3|[epion-t3-rest](https://github.com/epion-tropic-test-tool/epion-t3-rest/blob/master/rest_spec.md)|com.epion_t3.rest|REST関連の機能|
|com.epion_t3|[epion-t3-rdb](https://github.com/epion-tropic-test-tool/epion-t3-rdb/blob/master/rdb_spec.md)|com.epion_t3.rdb|RDBへのアクセスやアサーションを行う機能|
|com.epion_t3|[epion-t3-ssh](https://github.com/epion-tropic-test-tool/epion-t3-ssh/blob/master/ssh_spec.md)|com.epion_t3.ssh|SSH関連の機能|
|com.epion_t3|[epion-t3-selenium](https://github.com/epion-tropic-test-tool/epion-t3-selenium/blob/master/selenium_spec.md)|com.epion_t3.selenium|Selenium関連の機能|


### GroupId / ArtifactId について
[How To Use - Project](pages/use/build.md) にて説明していますが、  
それぞれの機能を利用するためにビルドシステムの設定ファイル（`build.gradle` or `pom.xml`）に対してライブラリの依存関係の定義を行います。  
その際に指定するのは、上記表の `GroupId` と `ArtifactId` およびそれぞれのライブラリの `Version` が必要になります。

### パッケージについて
[Basic Structure - Customs](pages/specification/basic_structure?id=customs) にて説明していますが、  
それぞれの機能を利用するためにシナリオに対して `パッケージ` を指定します。  
その際に指定するのは、上記表の `パッケージ` が必要になります。
# Command Index

コマンドは機能分類毎に細かく分割されており、取捨選択できるようになっています。  
ユーザが独自カスタマイズしたものは掲載されていませんが、`ETTT`であらかじめ用意しているものについては本ドキュメントに掲載しています。

|GroupId|ライブラリ名(ArtifactId)|パッケージ|概要|
|:---|:---|:---|:---|
|com.epion_t3|[epion-t3-basic](https://github.com/epion-tropic-test-tool/epion-t3-basic/blob/master/basic_spec.md ':include :type=markdown')|com.epion_t3.basic|基本機能|
|com.epion_t3|[epion-t3-log](pages/specification/command/log.md)|com.epion_t3.log|ログ関連の機能|
|com.epion_t3|[epion-t3-rest](pages/specification/command/basic.md)|com.epion_t3.rest|REST関連の機能|
|com.epion_t3|[epion-t3-rdb](pages/specification/command/rdb.md)|com.epion_t3.rdb|RDBへのアクセスやアサーションを行う機能|
|com.epion_t3|[epion-t3-ssh](pages/specification/command/ssh.md)|com.epion_t3.ssh|SSH関連の機能|
|com.epion_t3|[epion-t3-selenium](pages/specification/command/selenium.md)|com.epion_t3.selenium|Selenium関連の機能|


### GroupId/ArtifactId について
[How To Use - Project](pages/use/build.md) にて説明していますが、それぞれの機能を利用するために
ビルドシステムの設定ファイル（`build.gradle` or `pom.xml`）に対してライブラリの依存関係の定義を行います。
その際に指定するのは、上記表の `GroupId` と `ArtifactId` およびそれぞれのライブラリの `Version` が必要になります。

### パッケージについて
[Basic Structure - Customs](pages/specification/basic_structure?id=customs) にて説明していますが、
それぞれの機能を利用するためにシナリオに対して `パッケージ` を指定します。
その際に指定するのは、上記表の `パッケージ` が必要になります。
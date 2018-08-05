<a href="http://ettt.t-zomu.com"><img src="https://epion-tropic-test-tool.github.io/epion-t3/media/logo-xsmall.svg" width="168.5px"/></a>

# What's This

## 概要
このツールは、次世代回帰試験ツールです。 +
テストの効率化を目的とした扱いやすい(自分にとって)テストツールです。 +
簡単に言うとテストシナリオをYAMLに記載し、ツールに与えることでテストを走行させるというものです。 +
また、このツールはキーワード駆動テストを行うための手段として用いれるようにすることを目標としています。 +
最終的には「ISO/IEC/IEEE 29119-5:2016」に準拠したソフトウェアを作成したいと考えています。

## 機能

このツールが保有する機能の大まかな一覧は以下です。

- YAMLで定義されたシナリオに従って処理を自動実行する
- ファイルシステムの操作を行う
- リモートで稼働しているSSHサーバと対話形式の操作を行う
- SCPによるファイル操作を行う
- FTPによるファイル操作を行う
- SeleniumのWebDriverを用いてローカルのブラウザの自動操作を行う
- AWSのS3に対してAPI発行を行う（一部APIのみ）
- AWSのSQSに対してAPI発行を行う（一部APIのみ）
- Redisに対して操作を行う（AWSのElastiCache for Redisを想定）
- RDBMSに対してSQLを発行する
- 任意のローカルコマンドの実行を行う

<!-- test.md -->
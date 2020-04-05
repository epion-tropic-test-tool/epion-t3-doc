# Overview

## What's Customization ?
`ETTT`は具備している機能では足りない場合や、振る舞いを変更したい場合に自分自身でカスタマイズすることができるようになっています。
`ETTT`をカスタマイズするためには、現状Javaでの実装を行う必要があります。

カスタマイズできる範囲は以下となります。  

- Flow (整備中)
- Command
  - Assert系Command
  - Evidence系Command

## Customization Flow

`ETTT` のカスタマイズを行うには、以下のフローに沿って開発を行います。

![ettt-structure](/media/customization-flow.png)

1. カスタム機能を作成するためのプロジェクト作成を行います。  
このプロジェクトは [テンプレートプロジェクト](https://github.com/epion-tropic-test-tool/epion-t3-custom-template) を用意していますのでコピーして適宜リネームする程度の作業です。
1. カスタム機能（Flow/Command）を作成するためには、設計を行う必要があります。  
設計とは専用のYAML定義（以降 設計YAML）を記載することを言います。  
1. 設計YAMLを記載したら、カスタム機能として提供するためのFlowもしくはCommandの使い方をユーザへ知らせるためのドキュメントを出力します。  
ドキュメント出力は [ツール](https://github.com/epion-tropic-test-tool/epion-t3-devtools-generator) にて行うことができます。  
 [ツール](https://github.com/epion-tropic-test-tool/epion-t3-devtools-generator) は [テンプレートプロジェクト](https://github.com/epion-tropic-test-tool/epion-t3-custom-template) に組み込まれており、Gradleから利用可能な状態にしてあります。
1. 設計YAMLを記載したら、カスタム機能を作成するための各種リソース（スケルトンJava、properties）を出力します。
1. カスタム機能を実装（プログラミング）します。
1. カスタム機能を提供するためにビルドおよびデプロイを行います。  
デプロイは、Maven形式の公開リポジトリ（ Maven Central、JCenter、Bintray ）もしくはユーザが個別に構築する Inhouse Repository ( Nexus、Artifactory ) を想定しています。











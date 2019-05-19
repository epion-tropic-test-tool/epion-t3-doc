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

1. カスタム機能（Flow/Command）を作成するためには、設計を行う必要があります。  
設計とは専用のYAML定義（以降 設計YAML）を記載することを言います。  
1. 設計YAMLを記載したら、カスタム機能として提供するためのFlowもしくはCommandの使い方をユーザへ知らせるためのドキュメントを出力します。ドキュメント出力はツールにて行うことができます。
1. 設計YAMLを記載したら、カスタム機能を作成するための各種リソース（java、properties）を出力します。
1. カスタム機能を実装します。
1. カスタム機能を提供するためにビルドおよびデプロイを行います。  
デプロイは、Maven形式の公開もしくはInhouse Repositoryを想定しています。











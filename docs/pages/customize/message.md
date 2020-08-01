# Message
カスタム機能を提供した際に、様々な状況（情報、警告、エラー）にてユーザに対してメッセージを通知したい場合があります。  
メッセージはその際に利用するものになります。

`ETTT`では、例外時およびログ出力時に`ResourceBundle`が利用できます。  
本章では、どのように`ResourceBundle`を定義し利用するかの説明を行います。

## 成果物
Messageの実装を行うための成果物は以下となります。

|リソース名|内容|
|:---|:---|
|messages.properties|メッセージ内容を定義したプロパティファイル。ロケール毎に作成します。|
|Messages.java|メッセージ定義をJava実装から参照しやすくするためのEnumクラスです。|


## Messageを定義する

### ファイル命名規約
メッセージを定義するファイル名は以下のパターンに合致している必要があります。

```
カスタム機能名 + _messages.properties
```

カスタム機能を読み込んだ際に、クラスパスリソースから合致したプロパティファイルを`ResourceBundle`で扱えるようにします。  
例として、`basic_messages.properties` などが挙げられます。

### メッセージID命名規約
メッセージIDに対しては特に制約を設けていませんが、大体以下のパターンに合致していると`ETTT` が提供する他機能との足並みが揃います。

```
カスタムパッケージ + inf + 連番４桁（左0埋め）
カスタムパッケージ + err + 連番４桁（左0埋め）
カスタムパッケージ + wrn + 連番４桁（左0埋め）
```

#### 実装例

`ETTT`では以下のようなメッセージの定義をしています。
```properties
com.epion-t3.core.err.0001=予期せぬエラーが発生しました.システムの故障の可能性があります.
com.epion-t3.core.err.0002=指定されたバージョンは存在しません.バージョン:{0}
com.epion-t3.core.wrn.0001=変数バインドに失敗しました.対象文字列:{0}
```


## Messages用のEnumを定義する
この対応は必須ではありませんが、`ETTT`ではこのような実装を行なっていますという紹介になります。  
メッセージを利用する際に、メッセージコードを文字列としてそのまま指定すると変更や削除が入った際にGrep等を行なって確実に修正することが面倒であるため、
各メッセージをEnumで定義してそのEnumを利用するようにしています。  
この対応は、自動化がされていない限りはEnumを作成するのも面倒であるためどれほどのメリットがあるか定かではありません。  
`ETTT`ではEnumを実装するために`Messages`インタフェースを用意しており、後述するメッセージを取得するクラスと連動するようにしています。
FQCNは以下になります。

~~~
com.epion-t3.core.message.Messages
~~~

#### 実装例

`ETTT`では以下のようなメッセージのEnum定義をしています。

```java
@Getter
@AllArgsConstructor
public enum CoreMessages implements Messages {

    CORE_ERR_0001("com.epion-t3.core.err.0001"),

    CORE_ERR_0002("com.epion-t3.core.err.0002"),

    CORE_WRN_0001("com.epion-t3.core.wrn.0001");

    private String messageCode;
}
```

※`ETTT`ではボイラープレートコードの排除のため[Lombok](https://projectlombok.org/)を利用しています。



## Messageを取得する
メッセージの取得は、`MessageManager`クラスを利用することで行えます。
FQCNは以下になります。

~~~
com.epion_t3.core.message.MessageManager
~~~

`MessageManager`はカスタマイズ実装者が利用するクラスであり、状況に応じて使い分けが可能ないくつかのオーバーロードされたメソッドを保有しています。
このクラスはシングルトンであるため、インスタンスを取得してから利用してください。

#### 実装例

```java

MessageManager.getInstance().getMessage(CoreMessages.CORE_ERR_0001);

// 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessage("com.epion-t3.core.err.0001");

// 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessage(CoreMessages.CORE_ERR_0002, "1.0.0");

// 指定されたバージョンは存在しません.バージョン:1.0.0

MessageManager.getInstance().getMessage("com.epion-t3.core.err.0002", "1.0.0");

// 指定されたバージョンは存在しません.バージョン:1.0.0

MessageManager.getInstance().getMessageWithCode(CoreMessages.CORE_ERR_0001);

// [com.epion-t3.core.err.0001] 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessageWithCode("com.epion-t3.core.err.0001");

// [com.epion-t3.core.err.0001] 予期せぬエラーが発生しました.システムの故障の可能性があります.

```

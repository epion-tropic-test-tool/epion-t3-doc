# Message
`ETTT`では、例外時およびログ出力時に`ResourceBundle`が利用できます。
本章では、どのように`ResourceBundle`を定義し利用するかの説明を行います。

## Messageを定義する
制約として、メッセージを定義するファイル名の接尾辞は`_messages.properties`である必要があります。
クラスパス配下に存在する接尾辞が合致したリソースを`ResourceBundle`で扱えるようにします。

#### 実装例

`ETTT`では以下のようなメッセージの定義をしています。
```properties
com.zomu.t.epion.t3.core.err.0001=予期せぬエラーが発生しました.システムの故障の可能性があります.
com.zomu.t.epion.t3.core.err.0002=指定されたバージョンは存在しません.バージョン:{0}
com.zomu.t.epion.t3.core.wrn.0001=変数バインドに失敗しました.対象文字列:{0}
```


## Messages用のEnumを定義する
この対応は必須ではありませんが、`ETTT`ではこのような実装を行なっていますという紹介になります。
メッセージを利用する際に、メッセージコードを文字列としてそのまま指定すると変更や削除が入った際にGrep等を行なって確実に修正することが面倒であるため、
各メッセージをEnumで定義してそのEnumを利用するようにしています。
この対応は、自動化がされていない限りはEnumを作成するのも面倒であるためどれほどのメリットがあるか定かではありません。
`ETTT`ではEnumを実装するために`Messages`インタフェースを用意しており、後述するメッセージを取得するクラスと連動するようにしています。
FQCNは以下になります。

~~~
com.zomu.t.epion.tropic.test.tool.core.message.Messages
~~~

#### 実装例

`ETTT`では以下のようなメッセージのEnum定義をしています。

```java
@Getter
@AllArgsConstructor
public enum CoreMessages implements Messages {

    CORE_ERR_0001("com.zomu.t.epion.t3.core.err.0001"),

    CORE_ERR_0002("com.zomu.t.epion.t3.core.err.0002"),

    CORE_WRN_0001("com.zomu.t.epion.t3.core.wrn.0001");

    private String messageCode;
}
```

※`ETTT`ではボイラープレートコードの排除のため[Lombok](https://projectlombok.org/)を利用しています。



## Messageを取得する
メッセージの取得は、`MessageManager`クラスを利用することで行えます。
FQCNは以下になります。

~~~
com.zomu.t.epion.tropic.test.tool.core.message.MessageManager
~~~

`MessageManager`はカスタマイズ実装者が利用するクラスであり、状況に応じて使い分けが可能ないくつかのオーバーロードされたメソッドを保有しています。
このクラスはシングルトンであるため、インスタンスを取得してから利用してください。

#### 実装例

```java

MessageManager.getInstance().getMessage(CoreMessages.CORE_ERR_0001);

// 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessage("com.zomu.t.epion.t3.core.err.0001");

// 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessage(CoreMessages.CORE_ERR_0002, "1.0.0");

// 指定されたバージョンは存在しません.バージョン:1.0.0

MessageManager.getInstance().getMessage("com.zomu.t.epion.t3.core.err.0002", "1.0.0");

// 指定されたバージョンは存在しません.バージョン:1.0.0

MessageManager.getInstance().getMessageWithCode(CoreMessages.CORE_ERR_0001);

// [com.zomu.t.epion.t3.core.err.0001] 予期せぬエラーが発生しました.システムの故障の可能性があります.

MessageManager.getInstance().getMessageWithCode("com.zomu.t.epion.t3.core.err.0001");

// [com.zomu.t.epion.t3.core.err.0001] 予期せぬエラーが発生しました.システムの故障の可能性があります.

```

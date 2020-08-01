# Error Process
カスタマイズ実装を行う際のエラー処理について説明します。  
`ETTT`では細かく用途分類された例外クラスをいくつか保有していますが、  
カスタマイズ実装にて利用するべき例外は`SystemException`のみです。
FQCNは以下になります。

~~~
com.epion_t3.core.exception.SystemException
~~~

`SystemException`は`java.lang.RuntimeException`を継承しています。
コンストラクタは、メッセージと例外(`Throwable`)を受け取るものを用意しています。
バインド変数の有無などの状況に応じて使い分けが可能ないくつかのオーバーロードされたコンストラクタを保有します。

**実装例**

`t`は`Throwable`です。

```java

throw new SystemException(CoreMessages.BASIC_ERR_9001);

throw new SystemException(t, CoreMessages.BASIC_ERR_9001);

throw new SystemException("com.epion_t3.core.err.0001");

throw new SystemException(t, "com.epion_t3.core.err.0001");

throw new SystemException(CoreMessages.BASIC_ERR_9002, "1.0.0");

throw new SystemException(t, CoreMessages.BASIC_ERR_9002, "1.0.0");

throw new SystemException("com.epion_t3.core.err.0002", "1.0.0");

throw new SystemException(t, "com.epion_t3.core.err.0002", "1.0.0");

```

`SystemException`の内部では、`MessageManager`を利用してメッセージの解決を行ないます。  
そのため、`Messages`インタフェースを実装したEnumについても引数で受け付けるようにしています。

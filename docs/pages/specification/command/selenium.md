# Selenium Command

[Selenium](https://www.seleniumhq.org/)に関するコマンドを提供します。  
本機能は、`Firefox 47`以降にのみ対応します。[geckodriver](https://github.com/mozilla/geckodriver/releases)が必要となりますのでご注意ください。

|Command|Summary|Assert|Evidence|
|:---|:---|:---|:---|
|[StartLocalWD](#StartLocalWD)|WebDriverの利用を開始する(通常のWebDriver)|-|-|
|[EndLocalWD](#EndLocalWD)|WebDriverの利用を終了する(通常のWebDriver)|-|-|
|[WDClearSendKeysElement](#WDClearSendKeysElement)|タグに対して現状の入力をクリアしてから入力を送信します|-|-|
|[WDClearSendKeysElements](#WDClearSendKeysElements)|タグに対して現状の入力をクリアしてから入力を送信します（複数タグ）|-|-|
|[WDClickElement](#WDClickElement)|タグをクリックします|-|-|
|[WDClickElements](#WDClickElements)|タグをクリックします（複数タグ）|-|-|
|[WDGet](#WDGet)|指定したURLへ遷移します。|-|-|
|[WDScreenShot](#WDScreenShot)|画面のスクリーンショットを取得します。|-|Yes|
|[WDSelectByIndexElement](#WDSelectByIndexElement)|Selectタグの選択肢をOptionタグのインデックスを指定して選択します。|-|-|
|[WDSelectByIndexElements](#WDSelectByIndexElements)|Selectタグの選択肢をOptionタグのインデックスを指定して選択します。（複数タグ）|-|-|
|[WDSelectVisibleTextElement](#WDSelectVisibleTextElement)|Selectタグの選択肢をOptionタグの表示文字列を指定して選択します。|-|-|
|[WDSelectVisibleTextElements](#WDSelectVisibleTextElements)|Selectタグの選択肢をOptionタグの表示文字列を指定して選択します。（複数タグ）|-|-|
|[WDSendKeysElement](#WDSendKeysElement)|タグに対して入力を送信します|-|-|
|[WDSendKeysElements](#WDSendKeysElements)|タグに対して入力を送信します（複数タグ）|-|-|
|[WDSendKeysSpaceElement](#WDSendKeysSpaceElement)||-|-|



## Selenium Command Processing

Selenium関連の機能は他機能と異なり、SeleniumのWebDriverを介してブラウザを操作するものになります。そのため、SeleniumのWebDriverを開始、操作、終了といった一連の流れをユーザがシナリオとして組む必要性があります。WebDriverは`ETTT`が変数へ格納して随時必要になった際に取り出すという方式を採用しています。

![selenium-command-image](/media/selenium-image.png)

例としてLocalWebDriverを利用するシナリオでは、必ず`StartLocalWD`を行い指定するブラウザを開始します。ブラウザを開始すると指定したスコープの[Variable](/pages/specification/variables)にブラウザの参照を登録します。その後、Selenium関連のコマンドでは登録したブラウザを[Variable](/pages/specification/variables)から参照して利用します。最終的にシナリオ終了時には`EndLocalWD`を行い利用したブラウザを終了するコマンドを実行します。こうすることによってブラウザは正常に終了されて綺麗な形でシナリオを終了することができます。

## Selector Kind

Selenium関連の機能には`Selector`を多く利用する場面があります。
SelectorはSeleniumのWebDriverを利用したことがある方であれば一度は使ったことがあると思いますが、いわゆる以下のソースコードの`findElement`の引数部になります。

```java
WebElement element = driver.findElement(By.id("hoge-id"));
```

この内容は、ブラウザが表示しているHTMLのDOM（Document）の中から、`getElementById`による要素の検索を行なっていることになります。`Selector`はつまり２つの要素から成り立ちます。

1. どのロジックで検索するか（id、name、css、etc...）
2. 対象は何か（ロジックに沿った検索するためのヒント情報）

このうち、`1`について`ETTT`では`セレクター要素（SelectorKind）`と呼びます。
コマンド上で現状利用できるセレクター種別は以下の通りです。

|SelectorKind|Contents|MultiHit|
|:---|:---|:---|
|id|要素に指定しているid属性による検索|-|
|name|要素に指定しているname属性による検索|Yes|
|className|要素に指定しているクラス名による検索|Yes|
|linkText|リンク(aタグ)の表示テキストによる検索|Yes|
|cssSelector|CSSのセレクターによる検索|Yes|
|xpath|XPathによる検索|Yes|

上記の表の列として`複数取得の可能性`とあります。これは`セレクター種別`とHTMLの特性上検索結果として取得できる要素（タグ）が複数存在する可能性があるということを表します。
例えばname属性は複数のタグに指定ができます。また、別の例ですと同様のcssは当然ながら複数のタグに指定できます。

## Basics of Selenium Command

Selenium関連のコマンドの基礎として、特殊な用途を持たない通常のブラウザ操作を行うようなコマンドでは以下の要素を必ず含むことになります。
これはコマンドを拡張した際にも基本的には守っていただきたい内容ではあります。

|Property|Contents|Common|
|:---|:---|:---|
|refWebDriver|対象とするブラウザの参照名です。`StartLocalWD`のtargetと合わせる必要があります。|Yes|
|selector|セレクター種別（SelectorKind）|Yes|
|elementIndex|複数要素を取得した場合に、何番目の要素か指定するためのインデックスです。|-|

以降のコマンド固有の説明において、上記のプロパティについては冗長であるため説明を行わない。

------

### StartLocalWD

#### Function

1. WebDriverを開始します
2. ブラウザを指定できます
3. 開始したWebDriverを変数へ格納し、後続コマンドで参照可能にします

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: StartLocalWD
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    target: scenario.web_driver_1
    browser: chrome
    height: 画面の高さを指定（768等）
    width: 画面の幅を指定（1024等）
    driverPath: driver/chromedriver
```

------

### EndLocalWD

#### Function

1. WebDriverを終了します
1. 終了したWebDriverを変数から削除します。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: EndLocalWD
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
```
------

### WDClearSendKeysElement

#### Function

1. 指定したタグの内容をクリアします。
2. 指定したタグに対して入力を行います。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDClickElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター  # (1)
    value: 入力値
```

1. 対象を特定するためのセレクター値を指定します。 
 
------

### WDClearSendKeysElements

#### Function

1. 指定したタグの内容をクリアします。
1. 指定したタグに対して入力を行います
1. タグが複数存在する場合に、何個目のタグかを指定することができます

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDClickElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
    value: 入力値
    elementIndex: 何番目かを指定するインデックス（0始まり）
```


------

### WDClickElement

#### Function

1. 指定したタグをクリックします

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDClickElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値  # (1)
```

1. 対象を特定するためのセレクター値を指定します。 
 

### WDClickElements

#### Function

1. 指定したタグをクリックします
2. タグが複数存在する場合に、何個目のタグかを指定することができます

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDClickElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
    refWebDriver: WebDriverを格納している変数
    elementIndex: 何番目かを指定するインデックス（0始まり）
```

------

### WDGet

#### Function

1. 指定したURLへ遷移します。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDGet
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    target: 遷移したいURL（バインド利用可能） #(1)
```

1. 遷移したいURLを指定します。 

------

### WDScreenShot

#### Function

1. 指定したWebDriverが操作しているブラウザのスクリーンショットを取得します。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDScreenShot
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
```

------

### WDSelectByIndexElement

#### Function

1. Selectタグの値をOptionタグのインデックス指定で選択します。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDSelectByIndexElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
    index: 選択するインデックス（0始まり）
```

------

### WDSelectByIndexElements

#### Function

1. Selectタグの値をOptionタグのインデックス指定で選択します。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDSelectByIndexElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
    index: 選択するインデックス（0始まり）
```

------

### WDSelectVisibleTextElement

#### Function

1. Selectタグの値をOptionタグの表示文字列指定で選択します。
2. タグが複数存在する場合に、何個目のタグかを指定することができます。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDSelectByIndexElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
    value: 選択する表示文字列
    elementIndex: 何番目かを指定するインデックス（0始まり）
```

------

### WDSelectVisibleTextElements

#### Function

1. Selectタグの値をOptionタグの表示文字列指定で選択します。
2. タグが複数存在する場合に、何個目のタグかを指定することができます。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDSelectByIndexElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
    value: 選択する表示文字列
    elementIndex: 何番目かを指定するインデックス（0始まり）
```
------

### WDSendKeysElement

#### Function

1. 指定したタグに対して入力を行います

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDClickElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター  # (1)
    value: 入力値
```

1. 対象を特定するためのセレクター値を指定します。 
 
------

### WDSendKeysElements

#### Function

1. 指定したタグに対して入力を行います。
1. タグが複数存在する場合に、何個目のタグかを指定することができます。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDClickElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
    value: 入力値
    elementIndex: 何番目かを指定するインデックス（0始まり）
```

------

### WDSendKeysSpaceElement

スペースキーのみを入力という特殊なコマンドになりますが、
Radioボタンの選択時にクリックでは反応しない場合に利用することを想定して作成しています。実際に動作確認を行なった[日本Seleniumユーザーコミュニティ
様のテスト用サイト](http://example.selenium.jp/reserveApp_Renewal/)のRadioボタンにてクリックでは対応ができなかったため、コマンド実装対応を行なっています。

#### Function

1. 指定したタグに対してスペースキーの入力を行います。

#### Structure

```yaml
commands: 
  - id: コマンドのID
    command: WDSendKeysSpaceElement
    summary: コマンドの概要（任意）
    description: コマンドの詳細（任意）
    refWebDriver: WebDriverを格納している変数
    selector: 対象を特定するためのセレクター種別
    target: 対象を特定するためのセレクター値
```
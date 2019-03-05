# Command
Commandはシナリオにおける実際の動作を行うものです。  
このCommandをカスタマイズすることによって`ETTT`で任意の動作を行えるようにできます。  

## CommandにおけるModelとRunner
Commandのカスタマイズを行うためには、ModelクラスとRunnerクラスを1対1の関係性で作成する必要があります。
ModelクラスはYAMLの定義を読み込むためのJavaBeansです。RunnerはYAMLから読み込んだ情報を元に実際に処理を行うためのクラスです。
それぞれのクラスに対して `ETTT` にて決められたインタフェースの実装やスーパークラスの継承が必要になります。

## Modelの作成
CommandのModelクラスを実装するためには、`Command`クラスを継承する必要があります。
また、Commandであることを示すための`CommandDefinition`アノテーションを付与する必要があります。
それぞれ、FQCNは以下となります。
~~~
com.zomu.t.epion.tropic.test.tool.core.model.scenario.Command
com.zomu.t.epion.tropic.test.tool.core.annotation.CommandDefinition
~~~


先ずはModelクラスの作成の前にスーパークラスである`Command`クラスの仕様を理解する必要があります。

|Field|Type|Required|Description|
|:---|:---:|:---:|:---|
|id|String|Yes|Commandを定義する際に割り振るID。IDなので一意性を持ってユーザが設定するものです。|
|summary|String|No|Commandの概要。|
|description|String|No|Commandの説明。概要より詳細に記載されることを想定しています。|
|command|String|Yes|Commandの指定。ユーザが利用時に`@CommandDefinition`のid属性で定義してある値を指定します。|
|target|String|No|Commandの対象を指定する想定で作成したフィールドです。デフォルト状態で存在するフィールドで自由に利用できます。|
|value|String|No|Commandで利用する叩いを指定する想定で作成したフィールドです。デフォルト状態で存在するフィールドで自由に利用できます。|

次にカスタマイズする祭のModelクラスの実装例を示します。

```java
package com.zomu.t.epion.tropic.test.tool.basic.command.model;

import com.zomu.t.epion.tropic.test.tool.basic.command.runner.StringConcatRunner;
import com.zomu.t.epion.tropic.test.tool.core.annotation.CommandDefinition;
import com.zomu.t.epion.tropic.test.tool.core.model.scenario.Command;
import org.apache.bval.constraints.NotEmpty;
import java.util.List;

@CommandDefinition(
  id = "StringConcat", /* (1) */
  runner = StringConcatRunner.class) /* (2) */
public class StringConcat extends Command {

  @NotEmpty
  private List<String> referenceVariables; /* (3) */

// omitted Getter And Setter Methods

}
```

このModelクラスは文字列結合を行うための`StringConcat`コマンドの実際の実装コードになります。
それぞれの実装ポイントについて以下で説明します。
実際に`ETTT`ではボイラープレートコードの排除のため[Lombok](https://projectlombok.org/)を利用しています。

1. idにはCommandの名前を設定します。このidは重複すると`ETTT`が意図せぬ挙動を行う場合がありますので命名する際には一意性に気をつけてください。|
2. runnerにはCommandの実処理を行うRunnerクラスを設定します。`ETTT`では起動時に`@CommandDefinition`アノテーションからModelクラスとRunnerクラスを紐づける時に利用します。|
3. カスタマイズしたい処理に必要な情報を得るためのフィールドを定義します。BeanVaridationを行うことができます。`ETTT`では軽量な[Apache BVal](http://bval.apache.org/)を利用しています。|

このModelクラスに対するYAMLの定義例は以下のようになります。

```yaml
commands:
 - id: sample-command
   summary : "メールアドレス作成"
   description: "2つの文字列(固定もしくは変数)を結合してメールアドレスを作成します"
   command: "StringConcat"
   target : "mailaddress"
   referenceVariables : ["scenario.username", "@example.ettt.com"]
```
このようにスーパークラスである`Command`クラスのフィールドも利用することができますので、
どのフィールドをどのような用途で利用するかは自由です。


## Runnerの作成
CommandのRunnerクラスを実装するためには、`CommandRunner`インタフェースを実装する必要があります。
CommandのRunnerクラスを作成する場合は、Commandの処理に必要な処理を提供している`AbstractCommandRunner`を継承します。
FQCNはぞれぞれ以下となります。
~~~
com.zomu.t.epion.tropic.test.tool.core.command.runner.CommandRunner
com.zomu.t.epion.tropic.test.tool.core.command.runner.impl.AbstractCommandRunner
~~~

`AbstractCommandRunner`を継承したクラスではexecuteメソッドを必ず実装する必要があります。
executeメソッドはCommandの処理を実行するメイン処理メソッドです。
このメソッド内でCommandが実現したい内容を実装します。


```java
void execute(  /* (1) */
  final COMMAND process,  /* (2) */
  final Logger logger) throws Exception;  /* (3) */
```

1. 戻り値はありません。
1. Command定義クラスであり、`COMMAND`(総称型指定)
1. コマンド専用のロガーでありSLF4Jの`Logger`を利用している。ログを出力する時にはこの`Logger`を利用すること。

例外ハンドリング処理は、Commandの実行を司るクラスにて行うため特に必要ありません。
また、`AbstractCommandRunner`クラスには、いくつかの便利メソッドが提供されていますので必要に応じてご利用ください。
以下では用意しているメソッドのシグネチャ説明を行います。

**resolveVariables**

変数の解決を行います。  
引数の変数の参照名からスコープの判断を行い値を返却します。

```java
Object resolveVariables(final String referenceVariable)
```

**getScenarioDirectory**

現在実行中のシナリオが格納されているディレクトリのパスを文字列で取得します。

```java
String getScenarioDirectory()
```

**getScenarioDirectoryPath**

現在実行中のシナリオが格納されているディレクトリのPathを取得します。
利用用途としては、シナリオが格納されているディレクトリの配下にdata/assertなどのディレクトリからデータを読み込むためなどを想定。
詳しくは、[基本ディレクトリ構成](/pages/specification/basic_directory_structure.md)を参照。

```java
Path getScenarioDirectoryPath()
```

**getGlobalScopeVariables**

グローバル変数マップを取得します。
このメソッドを利用して自分で変数を解決するよりも、`resolveVariables`を利用することをオススメします。

```java
Map<String, Object> getGlobalScopeVariables()
```

**getScenarioScopeVariables**

シナリオ変数マップを取得します。
このメソッドを利用して自分で変数を解決するよりも、`resolveVariables`を利用することをオススメします。

```java
Map<String, Object> getScenarioScopeVariables()
```

**getFlowScopeVariables**

Flow変数マップを取得します。
このメソッドを利用して自分で変数を解決するよりも、`resolveVariables`を利用することをオススメします。

```java
Map<String, Object> getFlowScopeVariables()
```


**実装例**

Modelクラスの説明でも例に出した`StringConcat`コマンドに対する`CommandRunner`の実装を例に出して説明します。
この説明では、クラスのソースコードを省略することなく全てのコードを掲載します。(JavaDoc等は割愛)
このCommandクラスは、指定された文字列(変数 or 固定値)を結合して１つの文字列としてシナリオスコープの変数に登録するという機能を提供します。

```java
package com.zomu.t.epion.tropic.test.tool.basic.command.runner;

import com.zomu.t.epion.tropic.test.tool.basic.command.model.StringConcat;
import com.zomu.t.epion.tropic.test.tool.core.command.runner.impl.AbstractCommandRunner;
import com.zomu.t.epion.tropic.test.tool.core.context.EvidenceInfo;
import org.apache.commons.lang3.StringUtils;
import org.slf4j.Logger;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

public class StringConcatRunner extends AbstractCommandRunner<StringConcat> {  /* (1) */

  @Override
  public void execute(
    final StringConcat command,
    final Logger logger) throws Exception {

    List<String> rawValues = new ArrayList<>();

    for (String referenceVariable : command.getReferenceVariables()) {  /* (2) */

      Object variable = resolveVariables(  /* (3) */
        globalScopeVariables,
        scenarioScopeVariables,
        flowScopeVariables,
        referenceVariable);

      if (variable != null) {
        rawValues.add(variable.toString());
      }
      
    }

    String joinedValue = StringUtils.join(rawValues.toArray(new String[0]));
    logger.info("Joined Value : {}", joinedValue);  /* (4) */
    scenarioScopeVariables.put(command.getTarget(), joinedValue);  /* (5) */

  }

}
```

1. `AbstractCommandRunner`を継承します。総称型には対応するModelクラスを指定するように実装してください。
1. `StringConcat`のModelクラスで定義しているFieldである`referenceVariables`をループ処理します。
1. `resolveVariables`メソッドを実行して変数の解決を行います。取得した変数がnullでなければ結合対象としてリストに追加しています。
1. 与えれた`Logger`に対してログを出力することでレポートにもそのログ内容を表示することができます。
1. シナリオスコープの変数に結合した文字列を設定します。変数名はModelクラスの`target`で指定された値を利用します。
# annotation


### 主なアノテーション

|アノテーション|説明|
|---|---|
|@Override|スーパークラスのメソッドをオーバーライドすることを示す|
|@FunctionalInterface|関数型インターフェースであることを示す|
|@Deprecated|非推奨の要素であることを示す|
|@SuppressWarnings|コンパイラの警告を無効にする|
|@SafeVarargs|安全でない可変長引数に対する警告を無効にする。final,static,privateのいずれかのメソッド|


### @FunctionalInterface
- 単一の抽象メソッドを持つインターフェースとする
- staticメソッドやデフォルトメソッドは定義可能
- java.lang.Objectクラスのpublicメソッドは抽象メソッドとしての宣言は可能
- equals(Object obj),hashCode(),toString()の3つはデフォルトメソッドとして定義できない
- 関数型インターフェースとして明示する場合は、@FunctionalInterfaceを付与する

### @Deprecated
非推奨。

### @SuppressWarnings
- 引数必須

|値|説明|
|---|---|
|unchecked|型の使用に関連する警告を抑制する|
|deprecation|@Deprecatedアノテーションがつけられたタイプまたはメソッドに関連する警告を抑制する|

例
```java
@SuppressWarnings(value={"unchecked"})
@SuppressWarnings(value={"unchecked","deprecation"}
@SuppressWarnings("unchecked")
@SuppressWarnings({"unchecked"})
@SuppressWarnings({"unchecked","desprecation"})
```


### @SafeVarargs
`@SafeVarargs`を付けるメソッドは、`final`、`static`、`private`のいずれかである必要がある。

### カスタムアノテーション
- 戻り値は不変タイプ
- 基本データ型
- String型
- Class型
- 列挙型
- アノテーション型
- 上記の型の一次元配列

### @Target

ElementTypeの列挙型の定数
|定数|適用場所|
|---|---|
|TYPE||
|FIELD||
|METHOD||
|PARAMETER||
|CONSTRUCTOR||
|LOCAL_VARIABLE||
|ANNOTATION_TYPE||
|PACKAGE||
|TYPE_PARAMETER||
|TYPE_USE||
|MODULE||

### @Retention
アノテーションをソースコードまで存在させるか確認。

|定数|適用場所|
|---|---|
|SOURCE|アノテーションはコンパイラによって破棄される|
|CLASS|アノテーションはコンパイラによってクラスファイルに記録されるが、実行時にVMによって無視される。デフォルトの動作である。|
|RUNTIME|アノテーションはコンパイラによってクラスファイルに記録され、実行時にVMによって読み取られる。|

### @Inherited
スーパークラスからアノテーションを継承させる。

### @Repeatable
複数回使用可能となるアノテーション自体の定義。

### アノテーションのアノテート

|アノテーション|説明|
|---|---|
|@Documented|Javadoc APIドキュメントの出力にも反映するようになる|
|@Target|アノテーションを付与する要素を限定する（TYPE,FIELD,METHOD,PARAMETER）|
|@Retention|アノテーションをソースコードもしくはクラスファイルまで保持するかなどを制御する|
|@Inherited|サブクラスにアノテーションを引き継ぐことを示す|
|@Repeatable|同じ場所に複数回適用する|


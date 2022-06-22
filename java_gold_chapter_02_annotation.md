# annotation


### 主なアノテーション

|アノテーション|説明|
|---|---|
|@Override|スーパークラスのメソッドをオーバーライドすることを示す|
|@FunctionalInterface|関数型インターフェースであることを示す|
|@Deprecated|非推奨の要素であることを示す|
|@SuppressWarnings|コンパイラの警告を無効にする|
|@SafeVarargs|安全でない可変長引数に対する警告を無効にする|


### @FunctionalInterface
- 単一の抽象メソッドを持つインターフェースとする
- staticメソッドやデフォルトメソッドは定義可能
- java.lang.Objectクラスのpublicメソッドは抽象メソッドとしての宣言は可能
- 関数型インターフェースとして明示する場合は、@FunctionalInterfaceを付与する

### @Deprecated

### @SuppressWarnings

|||
|---|---|
|unchecked|型の使用に関連する警告を抑制する|
|deprecation|@Deprecatedアノテーションがつけられたタイプまたはメソッドに関連する警告を抑制する|
|||

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


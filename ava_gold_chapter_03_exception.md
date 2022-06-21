## chapter03 例外処理

- checked例外 >> 必須
- unchecked例外 >> 任意

独自例外クラスの作成

[修飾子] class クラス名 extends Exception{}

|カテゴリ|クラス名|説明|
|---|---|---|
|Errorのサブクラス  unchecked例外（例外処理は任意）|AssertionError|assert文を使用している際にboolean式でfalseが返ると発生|
||StackOverflowError|アプリケーションでの再起の回数が多すぎる場合に発生|
||NoClassDefFoundError|読み込もうとしたクラスファイルが見つからない場合に発生|
|RuntimeExceptionのサブクラス  unchecked例外|ArrayIndexOutOfBoundsException||
||ArrayStoreException||
||ClassCastException||
||IllegalStateException|メソッドの呼び出しが正しくない状態で行われた場合に発生|
||DateTimeException||
||MissingResourceException||
||ArithmeticException|整数をゼロで除算した場合に発生|
||NullPointerException||
||NumberFormatException||
|RuntimeException以外のExceptionのサブクラス  checked例外|IOException||
||FileNotFoundException||
||ParseException|解析中に予想外のエラーがあった場合に発生|
||SQLException||

|メソッド名|説明|
|void printStackTrace()|エラートレースを出力する|
|String getMessage()|エラーメッセージを取得する|

#### マルチキャッチ
- 継承関係のある例外クラスは列記できない
- キャッチした参照変数は暗黙的に*final*となる

#### throwsキーワードが使用されているメソッドをオーバーライドする
- サブクラスのメソッドがスローする例外は、スーパークラスのメソッドがスローする例外クラスと同じか、その例外クラスのサブクラスとする。
- RuntimeExceptionおよびRuntimeExceptionのサブクラスの例外は、スーパークラスのメソッドに関係なくスロー出来る。
- スーパークラスのメソッドにthrowsがあっても、サブクラス側でthrowsを記述しないことは可能


#### 例外処理の順番
読み込み >> close >> finally

#### アサーション
```java
-ea：「」
-da：「明示的に無効を指定」

assert(val<0):"A";
assert val<0:"A";
```

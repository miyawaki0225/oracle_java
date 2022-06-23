# 入出力（基本とNIO.2）

- 入出力用のパッケージ
- ストリーム
- シリアライズ
- コンソール
- ストリームの書式化及び解析
- ファイル操作
- ディレクトリ操作

## 入出力用のパッケージ

### Java I/O,NIO,NOI.2
|パッケージ|説明|
|---|---|
|java.io                  |データ・ストリーム、直列化、ファイルシステム入出力用の基本的なインターフェースとクラスを提供|
|java.nio.file            |ファイル、ファイル属性、およびファイルシステムにアクセスするためのインターフェースとクラスを提供|
|java.nio.file.atttribute |ファイルおよびファイルシステム属性へのアクセスを提供するインターフェースとクラスを提供|
|java.nio.channels        |入出力操作を実行できるエンティティー（ファイル、ソケットなど）への接続を表すチャネルクラスを提供|

### Fileクラス

|コンストラクタ名|説明|
|---|---|
|File(String pathname)|指定されたパス名文字列を抽象パス名に変換して、Fileオブジェクトを生成|
|File(String parent,String child)|親パス名文字列および子パス名文字列からFileオブジェクトを生成|
|File(File parent, String child)|親抽象パス名及び子パス名文字列からFileオブジェクトを生成|

## ストリーム

||バイトストリーム（byte単位）|キャラクタストリーム（char単位）|
|---|---|---|
|出力ストリーム|OutputStream |Writer|
|入力ストリーム|InputStream  |Reader|

主なバイトストリーム
|クラス名|説明|
|---|---|
|FileInputStream  |ファイルからbyte単位の読み込みを行うストリーム|
|FileOutputStream |ファイルからbyte単位の書き出しを行うストリーム|
|DataInputStream  |基本データ型のデータを読み込めるストリーム|
|DataOutputStream |基本データ型のデータを書き出せるストリーム|

主なキャラクタストリーム
|クラス名|説明|
|---|---|
|FileReader     |ファイルからchar単位の読出しを行うストリーム|
|FileWriter     |ファイルからchar単位の書き出しを行うストリーム|
|BufferedReader |char単位で、文字、配列、行をバッファリングしながら読み込むストリーム|
|BufferedWriter |char単位で、文字、配列、行をバッファリングしながら書き出すストリーム|

### FileInputStreamクラスとFileOutputStreamクラス
### DataInputStreamクラスとDataOutputStreamクラス
### FileReaderクラスとFileWriterクラス

|コンストラクタ名|説明|
|---|---|
|FileReader(File file) throws FileNotFoundException||
|FileReader(File fileName) throws FileNotFoundException||
|FileWriter(File file) throws IOException||
|FileWriter(String fileName)throws IOException||

|メソッド名|説明|
|---|---|
|int read() throws IOException|ストリームから単一文字を読み込む。ファイルの終わりに達すると-1を返す。|
|void write(String str) throws IOException|引数で指定された文字列を書き出す。|
|void flush() throws IOException|目的の送信先に、ただちに文字を書き出す。|


### BufferedReaderクラスとBufferedWriterクラス
```java
//構文
FileWriter(File file, boolean append) throws IOException
FileWriter(String fileName, boolean append) throws IOException
```
各コンストラクタの第2引数にtrueを指定すると追記となります。falseを指定すると先頭から書き込みが行われます。

### BufferedReaderクラスとBufferedWriterクラスの主なコンストラクタとメソッド

|コンストラクタ名|説明|
|---|---|
|BufferedReader(Reader in)||
|BufferedReader(Reader in,int sz)||
|BufferedWriter(Writer out)||
|BufferedWriter(Writer out)||
|BufferedWriter(Writer out, int sz)||

※szはバッファサイズ

|戻り値|メソッド名|説明|
|---|---|---|
|int    |read()                   |ストリームから単一文字を読み込む。終わりに達すると「-1」を返す。|
|String |readLine()               |1行のテキストを読み込む。1行の終わりは「\\n」。終わりに達すると「null」を返す。|
|void   |mark(int readAheadLimit) |ストリームの現在位置にマークを設定する。|
|void   |reset()                  |ストリームを、mark()によりマークされた位置にリセットする。|
|long   |skip(long n)             |引数で指定された文字数をスキップする|
|void   |write(String str)        |引数で指定された文字列を書き出す。|
|void   |newLine()                |改行文字を書き出す。|
|void   |flush()                  |目的の書き出し先に、ただちに文字列を書き出す。|

### Systemクラスの定数
- write()
- append()
- print()
- println()（暗黙的に`flush()`）
- printf()（暗黙的に`flush()`）
- format()（暗黙的に`flush()`）

## シリアライズ
- シリアライズ：オブジェクトを出力ストリームに書き出す
- デシリアライズ：シリアライズされたオブジェクトを読み込んで、メモリ上に復元する事
- static変数はシリアライズ対象外
- 明示的にシリアライズ対象外にしたいインスタンス変数がある場合には、変数にtransient修飾子を指定

### ObjectInputStreamクラスとObjectOutputStreamクラス

|コンストラクタ名|説明|
|---|---|
|ObjectInputStream(InputStream in)|引数で指定されたInputStreamから読み込むObjectInputStreamを作成|
|ObjectOutputStream(OutputStream out)|引数で指定されたOutputStreamに書き出すObjectOutpustStreamを作成|

|メソッド名|説明|
|---|---|
|Object readObject()|ObjectInputStreamからオブジェクトを読み込む。戻り値はObject型|
|void witeObject(Object obj)||

### シリアライズの継承
クラスが直接`Serializable`インターフェースを実装していなくても、スーパークラスが実装していればシリアライズ可能。

- 配列をシリアライズする場合は、その要素のそれぞれがシリアライズ可能でなければならない。
- シリアライズ化されたオブジェクトがオブジェクト参照変数によって参照するすべてのオブジェクト（参照先のオブジェクト）はシリアライズ可能でなければならない。
- static変数およびtransient指定された変数はシリアライズ対象外となる。
- あるクラスがシリアライズ可能であれば、たとえ明示的に`Serializable`インターフェースを実装していなくても、暗黙的にシリアライズ可能である。
- サブクラスが`Serializable`インターフェースを実装している場合、スーパークラスはデシリアライズの際にインスタンス化される。

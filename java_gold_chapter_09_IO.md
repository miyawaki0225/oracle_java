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

## コンソール
- ConsoleオブジェクトはSystemクラスのconsole()メソッドで取得。コンストラクタがprivate指定されているためnewによるインスタンス化は出来ない。
- コンソールが利用できない場合`console()`メソッドは`null`を返す。

|戻り値|メソッド名|説明|
|---|---|---|
|PrintWriter|writer()||
|Console|format(String fmt, Object... args)||
|Console|printf(String format, Object... args)||
|String|readLine()||
|String|readLine(String fmt, Object... args)||
|char[]|readPassword()||
|char[]|readPassword(String fmt,Object... args)|

## ストリームの書式化および解析

```java
%[インデックス$][フラグ][幅][.精度]変換の種類
```
|||
|---|---|
|インデックス|%の後に、数字$を記述すると置換引数を明示的に指定可能|
|フラグ|+ 符号を出力|
||0 空きを0で埋める|
||, 数値を桁ごとに「,」で区切る。ロケールに依存。|
|幅|出力時の最小文字数|
|.精度|精度。出力に書き込まれる最大文字数。|
|変換の種類|b(boolean),c(文字),d(整数),f(浮動小数点数),s(文字列),n(行区切り文字)|

## ファイル操作
### java.nio.fileパッケージの利用
### Pathインターフェース
|インターフェース|説明|
|---|---|
|Pathインターフェース |システムに依存するファイルパスを表す。|
|Pathsクラス         |パス文字列またはURIを変換してPathオブジェクトを返すstaticメソッドを提供する。|
|FileSystemsクラス   |ファイルシステム用のファクトリクラス。|
|FileSystemクラス    |ファイルシステムへのインターフェースを提供。|

### Pathクラスのget()メソッド
- windowsベースのファイルセパレータを明示的に使用する場合は、エスケープシーケンスを使用。

|メソッド名|説明|
|---|---|
|Path get(String first,...)|1つのパス文字列、もしくは第2引数以降で指定されたパス文字列をもとにPathオブジェクトに変換する。|
|Path get(URI uri)|指定されたURIをPathオブジェクトに変換する。|

```java:sample
Path path1 = Paths.get("data.txt");
Path path2 = Paths.get("C:\\sample\\chap9\\13\\data.txt");
Path path3 =
```

### Pathインターフェースの主なメソッド
|戻り値|メソッド名|説明|
|---|---|---|
|String |toString()                           ||
|Path   |getFileName()                        |このパスが示すファイルまたはディレクトリの名前をPathオブジェクトとして返す。|
|Path   |getName(int index)                   ||
|int    |getNameCount()                       ||
|Path   |subpath(int beginIndex,int endIndex) |開始インデックスから、終了インデックス-1の要素までで構成されたパス（ルート要素は含まない）を返す。|
|Path   |getParent()                          |親ディレクトリのパスを返す。|
|Path   |getRoot()                            |パスのルートを返す。|
|Path   |normalize()                          |このパスから冗長な名前要素を削除したパスを返す。|
|URI    |toUri()                              ||
|boolean|isAbsolute()                         ||
|Path   |toRealPath()                         ||
|Path   |resolve(String other)                ||
|Path   |relativize(Path other)               |このパスと指定されたパスとの間の相対パスを返す。|
|Iterator\<Path>|iterator()                   |ディレクトリ階層の要素を返すイテレータを取得。イテレータでは、ルートコンポーネント（存在する場合）は返さない。|
|boolean|endsWith(String other)               |引数で指定したパス文字列で終わっているとtrueが返る。|

### Filesクラス

Filesクラスの主なメソッド
|戻り値|メソッド名|説明|
|---|---|---|
|boolean|exists(Path path, LinkOption... options)||
|boolean|notExists(Path path, LinkOption... options)||
|boolean|isSameFile()||
|boolean|isDirectory()||
|boolean|isRegularFile()||
|boolean|isReadable()||
|boolean|isWritable()||
|boolean|isExecutable()||
|Path|createDirectory()||
|Path|copy()||
|Path|move()||
|long|size()||
|void|delete()||
|boolean|deleteIfExists()||
|List\<String>readAllLines()|||
|UserPrincipal|getOwner()||
|Object|getAttribute()||
|Path|setAttribute()||
|Map<String,Object>|readAttributes()||
|\<A extends BasicFileAttributes>A| readAttributes()||
|DirectoryStream\<Path>|newDirectoryStream()||


### copy()およびmove()メソッドのオプション

copy()メソッドの第3引数に指定できるStandardCopyOption列挙型とLinkOption列挙型
|定数名|説明|
|---|---|
|REPLACE_EXISTING|コピー先ファイルが既に存在する場合でもコピーを実行する|
|COPY_ATTRIBUTES|ファイルに関連付けられたファイル属性をコピー先ファイルにコピーする|
|NOFOLLOW_LINKS|リンクがコピーされてリンク先はコピーされない。|

copy()メソッドの第3引数に指定できるStandardCopyOption列挙型とLinkOption列挙型
|定数名|説明|
|---|---|
|REPLACE_EXISTING|移動先ファイルが既に存在する場合でも移動を実行する|
|ATOMIC_MOVE|移動をアトミックなファイル操作として実行する。|


### java.nio.file.attributeパッケージの主なインターフェースとクラス

|インターフェース|説明|
|---|---|
|FileTimeクラス||
|FileAttribute\<T>||
|BasicFileAttributes||
|DosFileAttributes||
|PosixFileAttributes||

### 主な属性名

|属性名|メソッド名|戻り値|
|---|---|---|
|lastModifiedTime |lastModifiedTime()|java.nio.file.attribute.FileTime|
|lastAccessTime   |lastAccessTime()|java.nio.file.attribute.FileTime|
|creationTime     |creationTime()|java.nio.file.attribute.FileTime|
|size             |size()|long|
|isRegularFile    |isReqularFile()|boolean|
|isDirectory      |isDirectory()|boolean|
|isSymbolicLink   |isSymbolicLink()|boolean|

### DosFileAttribulesインターフェースのメソッド
|メソッド名|説明|
|---|---|
|boolean isArchive()  |アーカイブ属性の値を返す|
|boolean isHidden()   |隠し属性の値を返す|
|boolean isReadOnly() |読み取り専用属性の値を返す|
|boolean isSystem()   |システムファイル属性の値を返す|

### ファイルツリーの探索

|戻り値|メソッド名|説明|
|---|---|---|
|Stream\<Path>    |walk()||
|Stream\<Path>    |walk()||
|Stream\<Path>    |find()||
|Stream\<Path>    |list|ディレクトリ内のエントリを要素に持つStreamを返す|
|Stream\<String>  |lines|ファイル内のすべての行をStreamとして読み取る|

第2引数に文字コードを指定するlines()メソッドを使用

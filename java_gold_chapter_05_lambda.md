# lambda

## 独自の関数型インターフェースの定義
- 単一の抽象メソッドを持つインターフェースとする
- ただし、staticメソッドやデフォルトメソッドは定義可能
- java.lang.Objectクラスのpublicメソッドは抽象メソッドとしての宣言は可能
- 関数型インターフェースとして明示する場合は、@FunctionalInterfaceを付与する

## varが利用できる箇所
- ローカル変数の初期化時
- 通常for文内のインデックス
- 拡張for文内で宣言されたローカル変数
- try-with-resources文内のローカル変数

## 注意
- メソッドの引数
- コンストラクタの引数
- メソッド戻り型
- フィールド（メンバ変数）
- catchブロック
- 予約語ではない

## ラムダ式
(実装するメソッドの引数)->{処理};

主な関数型インターフェース

|インターフェース名|抽象メソッド|引数|戻り値|
|---|---|:---:|:---:|
|Function<T,R>    |R apply(T,t)           |T    |R    |
|BiFunction<T,U,R>|R apply(T t, U u)      |T,U  |R    |
|Consumer\<T>      |void accept(T t)       |T    |なし |
|BiConsumer<T,U>  |void accept(T t, U u)  |T,U  |なし |
|Predicate\<T>     |boolean test(T t)      |T    |boolean|
|BiPredicate<T,U> |boolean test(T t, U u) |T,U  |boolean|
|Supplier\<T>      |T get()                |なし |T    |
|UnaryOperator\<T> |T apply(T t)           |T    |T    |
|BinaryOperator\<T>|T apply(T t1, T t2)    |T,T  |T    |

## 省略記法
- ()が省略できない場合；引数がない場合、引数が複数ある場合、データ型を明示した場合
- returnを記述した場合末尾に「；」が必要
- returnを省略した場合「｛｝」も省略する必要がある

- メソッド参照
- staticメソッド参照
- インスタンスメソッド参照
- コンストラクタ参照（クラス名::）

## 引数や戻り値の型にint型を使用する関数型インターフェース
|インターフェース名|抽象メソッド|
|---|---|
|IntFunction\<R>|R apply(int value)|
|IntConsumer|void accept(int value)|
|IntPredicate|boolean test(int value)|
|IntSupplier|int getAsInt()|
|IntUnaryOperator|int applyAsInt(int operand)|
|IntBinaryOperator|int applyAsInt(int left,int right)|

## 異なるデータ型をもとに、基本データ型の結果を返す関数型インターフェース
|インターフェース名|抽象メソッド|
|---|---|
|ToIntFunction\<T>|int applyAsInt(T value)|
|ToIntBiFunction<T,U>|int applyAsInt(T t, U u)|
|IntToDoubleFunction|double applyAsDouble(int value)|
|IntToLongFunction|long applyAsLong(int value)|
|ObjIntConsumer\<T>|void accept(T t,int value)|


## 関数型インターフェースの命名パターン
|条件|インターフェース名|（引数）->戻り値|
|---|---|---|
|引数が２つ「Bi」|BiFunction<T,U,R>|(T,U)->R|
||BiConsumer<T,U>|(T,U)->void|
|「引数の型名」|IntFunction\<R>|(int)->R|
||DoubleFunction\<R>|(double)->R|
|「To戻り値の型名」|ToIntFunction\<T>|(T)->int|
||ToDoubleFunction\<T>|(T)->double|
|「引数の型名To戻り値の型名」|IntToDoubleFunction|(int)->double|
||DoubleToIntFunction|(double)->int|

## 関数型インターフェースの合成

|メソッド名|説明|メソッドを提供している主なインターフェース|
|---|---|---|
|andThen|Consumer andThen (Consumer after)  ||
|compose|Function compose(Function before)  ||
|and    |Predicate and(Predicate other)     ||
|or     |Predicate or (Predicate other)     ||
|negate |Predicate negate()                 ||
※negate()⇒反転

Streamの基本の流れ

1. 配列やコレクションからストリームオブジェクトを取得
2. ストリームオブジェクトに対して中間操作（Intermediate Operation）
3. ストリームオブジェクトに対して終端操作（Terminal Operation）
4. 中間操作は終端操作が行われた時点で行われる（遅延評価）
5. ストリームインターフェースはjava.lang.AutoCoseableを継承しているので再利用不可


8.0で関数型インターフェースというものが実装された

どういうものか？
＞抽象メソッドを１つだけもつインターフェース

```java
@FunctionalInterface
interface SampleBaseRambda {
    public abstract void rambdaBaseMethod();
}
```

### Stream Object

- Stream\<T>
- IntStream
- LongStream
- DoubleStream
上記4種のsuper interfaceは`BaseStream`



java.util.collectionインターフェースの実装例

```java
List\<String>list=Arrays.asList("A","B","C","D");
Stream<String>stream = list.stream();
```

java.util.Arraysクラスの実装例

```java
//sample01
int[] arr = {1,2,3};
IntStream stream = Arrays.stream(arr);

//sample2
IntStream stream = IntStream.of(1,2,3);
```

関数型インターフェースは43種類。
しかし、メインは7つで残りは派生ver

- Supplier\<T>
  - `「T get()」`
  - 主なメソッド：generate,
  - 派生：Boolean～,Int～,Long～,Double～
- Consumer\<T>
  - `「void accept(T t)」`
  - 主なメソッド：peek,forEach
  - 派生：Int～、Long～、Double～
  - 特殊：BiConsumer\<T,U>
  - 参照：ObjInt～、ObjLong～、ObjDouble～
- Predicate\<T>
  - 「boolean test(T t)」
  - 主なメソッド：allMatch,anyMatch,noneMatch
  - 特殊：Int～、Long～、Double～、BiPredicate\<T,U>
- Function\<T,R>
  - 「R apply(T t)」
  - 主なメソッド：map,flatMap
  - プリミティブ型→参照型：Int～、Long～,Double～
  - 参照型→プリミティブ型：ToInt～、ToLong～、ToDouble～
  - プリミティブ→別プリミティブ：
      - IntToLong～、IntToDouble～
      - LongToInt～、LongToDouble～
      - DoubleToInt～、DoubleToLong～
- BiFunction\<T,U,R>
  - 「R apply(T t,U u)」
  - 特殊：ToIntBi～、ToLongBi～,ToDoubleBi～
- UnaryOperator\<T>
  - 「T apply(T t)」
  - 主なメソッド：iterator
  - 特殊：Int、Long、Double
- BinaryOperator\<T> 
  - 「T apply(T t1,T t2)」
  - 主なメソッド：reduce
  - 特殊：Int～、Long～、Double～

### Supplier\<T>

---
供給者：何もないところからオブジェクトを生み出す「T get()」

```java
Stream<Double> stream01 = Stream.generate(()-> Math.random()).limit(10);

Stream<Double> stream02 = Stream.generate(Math::random).limit(10);
```

### Consumer\<T>

消費者：戻り値なし「void accept(T t)」

### Predicate\<T>

評価：「boolean test(T t)」

```java
List\<String>list = Arrays.asList("ABC","DE","FGHI");
Predicate\<String> p = s -> s.length >= 3;

boolean all = list.stream().allMatch(p);
boolean any = list.stream().anyMatch(p);
boolean none = list.stream().noneMatch(p);

System.out.println(all);
System.out.println(any);
System.out.println(none);
```

### Function<T,R>

---

T型として受け取った引数を処理し、R型の値を返す。（データの加工・変換）
「R apply(T t)」


### BiFunction<T,U,R>
TとU型の引数を受け取り、R型として返す
「R apply(T t,U u)」


### UnaryOperator<T>

「T apply(T t)」

```java
Stream.iterate(1, x -> x * 10).limit(5).forEach(System.out::println);
```

iterateメソッドは2つの引数を取り、第一引数は初期値、第二引数は値に対する演算。
よって「無限順次ストリーム」となるので「limit」メソッドが必要

### BinaryOperator<T>

---

2つの引数と戻り値の型が同じ
「T apply(T t1,T t2)」


# 終端操作

- count
- reduce →　後述
- sum,average(optional型)
- max,min
  - 引数`Comparator<? super T>`型、戻り値として`Oprional<T>`型を返す　
  - Comparatorが表現する順序付けに従って最大値（最小値）を返す
  - OptionalクラスはNullではなく、空っぽを返す事が出来る

```java
List<String> list1 = Arrays.asList("ABC","DE","FGHI","J");
String max1 = list1.stream().max((x, y) -> x.length() - y.length()).orElse("最大値無し");
String min1 = list1.stream().min((x, y) -> x.length() - y.length()).orElse("最小値無し");

System.out.println(max1);
System.out.println(min1);

List<String>list2 = Arrays.asList();
String max2 = list2.stream().max((x, y)-> x.length() - y.length()).orElse("最大値無し");
String min2 = list2.stream().min((x, y)-> x.length() - y.length()).orElse("最小値無し");

System.out.println(max2);
System.out.println(min2);

```

↑は文字数の少ないものが小さいという順序付け

### 終端操作reduce

広義の「リダクション操作」データの集合を一つに要約するような操作。
狭義の「リダクション操作」はcount,max,min,sum,average

引数1つ

```java
Optional<T> reduce(BinaryOperator<T> accumulator)
T reduce(T identity, BinaryOperator<T> accumulator)

List<String> list=Arrays.asList("A","B","C","D","E");
Optional<String> optional = list.stream().reduce((x, y) -> x + y);
System.out.println(optional.orElse(""))

```
この出力結果は「ABCDE」となります。
「list.stream()」の戻り値であるStream<String>型のストリームオブジェクトは、「A」「B」「C」「D」「E」の５つの文字列を保持しています。
reduceメソッドの引数のラムダ式「(x,y)->x+y」は、文字列の連結を表現しています。
この指示により、「A」と「B」を連結して「AB」、「AB」に「C」を連結して「 BC」という具合に、
すべての要素に対して文字列を連結します。つまり、「A」+「B」+「C」+「D」+「E」となり、
リダクションの結果は「ABCDE」ですね。reduceメソッドの戻り値がOptional<String>型なのは、
ストリームオブジェクトの保持するデータが０個の場合は連結結果がないからです。


引数２つ(String str1,String str2)

```java
List<String> list=Arrays.asList("A","B","C","D","E");
String str = list.stream().reduce((x, y) -> x + y);
System.out.println(str)
```

## 終端操作collect

---
「可変リダクション操作」
List,Set,Map,StringBuilderに要素を収集する操作

定義
```java
<R> R collect(
        Supplier<R> supplier,BiConsumer<R,? super T> accumulator,
        BiConsumer<R, R> combiner)
```

1. T型はストリームが持つ個々のデータ型
2. R型は可変コンテナの型
3. 第一引数で可変コンテナオブジェクトに新規に生成するラムダ式を記述
4. 第二引数でその可変コンテナにストリームが持つ個々のデータをどう格納するかを記述
5. 第三引数で複数の可変コンテナをどう結合するかを記述。

sample

```java
Stream<String> stream = Stream.of("A","B","C","D","E");
ArrayList<String> list = stream.collect(
        () -> new ArrayList<String>(),
        (l, str) -> l.add(str),
        (l1, l2) -> l1.addAll(l2)
        );
list.stream().forEach(System.out::println);

```

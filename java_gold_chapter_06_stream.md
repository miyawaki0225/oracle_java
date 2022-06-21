# StreamAPI

## ストリームのパイプライン処理
- 問い合わせの対象となるデータソース（コレクション、配列、I/Oリソースなど）
- 中間操作：ストリームに対して処理を行った結果をストリームで返す
- 終端操作：処理結果をデータとして返す

## ストリーム生成用の主なメソッド

```java
// Collectionインターフェース
List<String>list = Arrays.asList("a","b","c");
Stream<String>stream = list.stream();

// Arraysクラスで提供
String[] array = {"a","b","c"};
Stream<String> stream = Arrays.stream(array);

// Streamインターフェースで提供
Stream<String> stream = Stream.of("abc");
Stream<String> stream = Stream.empty();
Stream<String> stream = Stream.generate(()->"\"java\"");

// IntStreamインターフェースで提供
IntStream = IntStream.of(1,2,3);
IntStream = IntStream.iterate(1,n->n+1);
IntStream = IntStream.range(1,10); //1-9
IntStream = IntStream.rangeClosed(1,10); //1-10

```

## 終端操作
- 一つのStreamオブジェクトに対して、終端操作は1度きり

|戻り値|メソッド名|説明|R|
|---|---|---|---|
|boolean|allMatch||×|
|boolean|anyMatch||×|
|boolean|noneMatch||×|
|\<R,A>|collect||〇|
|\<R> R|collect||〇|
|long|count()||〇|
|Optional\<T>|findAny()||×|
|Optional\<T>|findFirst()||×|
|void|forEach||×|
|Optional\<T>|min||〇|
|Optional\<T>|max||〇|
|T|reduce||〇|
|Object[]|toArray()||×|
|\<A>A[]]|toArray(IntFunction\<A[]>generator||×|
  
>※リダクション操作とは
>リダクション操作はストリーム内のすべての要素を累積関数を使って一つにまとめた結果を返す操作です。
>reductionは日本語では畳み込みと言ったりするようですが、いわゆる畳み込み積分(convolution)ではありません。
>どっちかというと総和や総乗のようなイメージです(というか、総和や総乗がリダクションの一種です)。
>リダクション操作には、1つの値を返すリダクションと値を複数含むコンテナ(Collectionなど)を返す可変リダクションがあります。


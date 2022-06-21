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

## 引数や戻り値の型にint型を使用する関数型インターフェース
|インターフェース名|抽象メソッド|
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
|andThen|Consumer andThen (Consumer after)||
|compose|Function compose(Function before)||
|and|Predicate and(Predicate other)||
|or|Predicate or (Predicate other)||
|negate|Predicate negate()||
※negate()⇒反転

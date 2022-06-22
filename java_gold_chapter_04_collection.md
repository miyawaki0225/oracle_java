# chapter04 Collection

### ラッパークラス
### Boxing/Unboxing

基本データ型同士は、暗黙の型変換が行われる。
基本データ型　⇒　ラッパークラス

|インターフェース名|説明|
|---|---|
|List||
|Set ← SortedSet||
|Queue||
|Map ← SortedMap||

### Collectionインターフェースの主なメソッド

|戻り値|メソッド名|説明|
|---|---|---|
|boolean    |add(E e)||
|void       |clear()||
|boolean    |contains(Object obj)||
|boolean    |containsAll(Collection\<?>c||
|boolean    |isEmpty()||
|boolean    |remove(Object o)||
|boolean    |removeAll(Collection\<?>c||
|Iterator\<E>|iterator()||
|Object[]   |toArray()||
|\<T> T[]    |toArray(T[] array)||
|int size()|||

### Mapインターフェースの主なメソッド

|戻り値|メソッド名|説明|
|void|clear()||
|boolean|containsKey(Object key)||
|boolean|containsValue(Object vale)||
|V|get(Object key)||
|void|putAll()||
|V|remove(Object key)||
|int|size()||
|Collection\<V>|values()||

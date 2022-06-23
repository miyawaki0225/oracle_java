# 並列処理
※`ConcurrentModificationException`


- スレッド
- スレッドの制御
- 排他制御と同期制御
- Concurrencyユーティリティーズ
- 並列コレクション
- Executorフレームワーク
- アトミック変数
- パラレルストリーム

## スレッド
1. Threadクラスのサブクラスを定義
2. Runnableインターフェースを実装

Runnableインターフェースは、関数型インターフェース
```java
@FunctionalInterface
public interface Runnable{
  public abstract void run();
}
```

スレッドの優先度
|メソッド名|説明|
|---|---|
|currentThread()||
|getName()||
|getPriority()||
|setPriority(int new Priority)||

スレッドの制御
|メソッド名|説明|
|---|---|
|sleep(long millis)||
|join() throws InterruptedException||
|yield()||
|interrupt()||

synchronizedによる排他制御
- 同時に一つのスレッドからしか実行されないことが保証されます。

wait(),notify(),notifyAll()による同期制御

|メソッド名|説明|
|---|---|
|wait()||
|wait(long timeout)||
|void notify()||
|void votifyAll()||

- デッドロック：すべてのスレッドがロックの開放を同時に待ってしまい、ロックが永久に解けなくなる状況。
- ライブロック：獲得が必要な時にほかのスレッドにロックされ、進まない処理を繰り返し続ける状態
- スレッドスタベーション：スレッドのスケジューリングが上手くいっていない状態。

### Concurrencyユーティリティーズ

- 例外：ConcurrentModificationException

```java
Map<Integer, String> map = new HashMap<>();
map.put(1, "tanaka");
map.put(2, "urai");
for (Integer key : map.keySet()) {
	map.remove(key);
}
```

並列処理のための主なパッケージ
- java.util.concurrent
- java.util.concurrent.atomic

|メソッド名|説明|
|synchronizedCollection(Collection\<T>c)||
|synchronizedSet(Set\<T>s)||
|synchronizedSortedSet(SortedSet\<T>s||
|synchronizedNavigableSet()||
|synchronizedList()||
|synchronizedList(List\<T>list)||
|synchronizedMap(Map\<K,V>m)||
|synchronizedNavigableMap(NavigableMap<K,V>m)||

## Queueインターフェースの拡張

|インターフェース／クラス名|説明|
|---|---|
|BlockingQueue      ||
|SynchronousQueue   ||
|LinkedBlockingQueue||
|ArrayBlockingQueue ||

BlockingQueueインターフェースの主なメソッド
||例外のスロー|特殊な値|ブロック|タイムアウト|
|---|---|---|---|---|
|挿入|add(e)|offer(e)|put(e)|offer(e,time,unit)|
|削除|remove()|poll()|take()|poll(time,unit)|
|検査|element()|peek()|適用外|適用外|

BlockingDequeインターフェースの主なメソッド（最初の要素）
||例外のスロー|特殊な値|ブロック|タイムアウト|
|---|---|---|---|---|
|挿入|addFirst(e)|offerFirst(e)|putFirst(e)|offerFirst(e,time,unit)|
|削除|removeFirst()|pollFirst()|takeFirst()|pollFirst(time,unit)|
|検査|getFirst()|peekFirst()|適用外|適用外|

BlockingDequeインターフェースの主なメソッド（最後の要素）
||例外のスロー|特殊な値|ブロック|タイムアウト|
|---|---|---|---|---|
|挿入|addLast(e)|offerLast(e)|putLast(e)|offerLast(e,time,unit)|
|削除|removeLast()|pollLast()|takeLast()|pollLast(time,unit)|
|検査|getLast()|peekLast()|適用外|適用外|

`queue` ← `Deque`,`BlockingQueue` ← `BlockingDequeu` ← "LinkedBlockingDeque"  

## Mapインターフェースの拡張

concurrentMapインターフェース
|メソッド名|説明|
|---|---|
|putIfAbsent(K key,V value)||
|remove(Object key,Object value)||
|replace(K key, V value)||
|replace(K key,V oldValue, V new Value)||

putIfAbsent()メソッドは1回のロック内で処理を実行し、別スレッドからの割り込みが入らないことを保証。

## ArrayListクラスとSetインターフェースの拡張

各々新しいオブジェクトを作成して、スレッドセーフを実現する。
|クラス名|説明|
|---|---|
|CopyOnWriteArrayList||
|CopyOnWriteArraySet||

## Executorフレームワーク
### ExecutorServiceを使用したタスクの実行
スレッドの再利用やスケジューリングを行うスレッドコードを簡単に実装出来る。

### Executorフレームワークの主なインターフェースとクラス
|インターフェース|説明|
|---|---|
|Executor|指令されたRunnableタスク（一つの処理）を実行するオブジェクト|
|ExecutorService|終了を管理するメソッド。Futureを生成する事も出来る|
|Future|非同期計算の結果を表す。|
|Callable|タスクを行うクラス。結果を返すメソッドを提供。|
|Executors||

### Executorsクラスの主なメソッド
|戻り値|メソッド名|説明|
|---|---|---|
|ExecutorService|newSingleThreadExecutor()|1つのスレッドでタスクの処理するExecutorServiceオブジェクトを返す。|
|ExecutorService|newFixedThreadPool(int nThreads)|固定数のスレッドを再利用するスレッドプールを提供する|
|ExecutorService|newCachedThreadPool()|以前構築されたスレッドを再利用する。|
|ScheduledExecutorService|newSingleThreadScheduledExecutor()|周期的にコマンドの実行をスケジュールできる。|
|ScheduledExecutorService|newScheduledThreadPool(int corePoolSize)|周期的にコマンドの実行をスケジュールできるスレッドプールを作成する。|
|Callable\<Object>|callable(Runnable task)|呼び出し時に、指定されたタスクを実行し、nullを返すCallableオブジェクトを返す。|
|Callable\<T>|callable(Runnable task, T result)||
  
  
### ExecutorServiceの主なメソッド
|戻り値|メソッド名|説明|
|---|---|---|
|boolean|awaitTermination(long timeout,TimeUnit unit)||
|boolean|isShutdown()||
|boolean|isTreminated()||
|void|shutdown()||
|List<Runnable|shutdownNow()||
|\<T>Future\<T>|submit(Callable\<T>task||
|Future\<?>|submit(Runnable task)||
|\<T>Future\<T>|submit(Runnable task, T result)||
|void execute(Runnable command)|指定されたタスクを実行する|
※executeは`Executor`インターフェースのメソッド

### Futureインターフェースの主なメソッド
|戻り値|メソッド名|説明|
|---|---|---|
|boolean|cancel(boolean mayInterruptIfRunnnig)|このタスクの実行の取り消しをここを見る|
|boolean|isCancelled()|このタスクが正常に完了する前に取り消された場合はtrueを返す。|
|boolean|isDone()|このタスクが完了した場合はtrueを返す。|
||V get(long timeout, TimeUnit unit)|必要に応じてタスクが完了するまで待機し、その後、タスク結果を取得する。|
||V get(long timeout, TimeUnit unit)|必要に応じて、最大で指定された時間及び計算が完了するまで待機し、その後タスク結果を取得する。|

### Callabeインターフェース

- Runnableインターフェースのrun()メソッドは、戻り値が`void`
- java.util.concurrent.Callableインターフェースは戻り値がオブジェクト
- call()メソッドにタスクを実装

|メソッド名|説明|
|---|---|
|V call() throws Exception|タスクを実行し結果を返す。タスクが実行できない場合は例外をスローする。|

### スレッドプール

CyclicBarrierクラスのコンストラクタと主なメソッド
|コンストラクタ／メソッド|説明|
|---|---|
|CyclicBarrire(int parties)|引数で指定された数分のスレッドが待機状態になると、バリアポイントを通過する。|
|CyclicBarrier(int parties,Runnable barrierAction)|バリアポイントを通過する際に、第2引数で指定されたバリアアクションを実行する。|
|int await()|バリアポイントに指定された数のスレッドが到着するまで待機する。|


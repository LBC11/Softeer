### Executor Framework
- 스레드를 만들고 관리하는 메커니즘 제공
```
ExecutorService executor = Executors.newFixedThreadPool(5); // 풀에는 5개의 스레드가 있습니다.
for (int i = 0; i < 10; i++) {
    Runnable worker = new MyRunnable(i);
    executor.execute(worker); // 스레드를 실행합니다.
}
executor.shutdown();
```

### Concurrent Collections
- Thread-safe함은 물론 여러 Thread가 동시에 컬렉션에 접근할 수 있다.
- 여러 부분으로 나눠서 락을 걸어 다른 부분에 접근 중이라면 여러 스레드가 동시 접근할 수 있다.
- synchronized Collection의 경우는 Thread-safe하지만, 동시 접근이 불가능해서 성능이 떨어진다.
```
Map<String, String> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("key1", "value1");
concurrentMap.put("key2", "value2");
concurrentMap.get("key1"); // value1을 반환합니다.
```

### Futures
- 비동기 계산의 결과를 표현하는 데 사용된다.
- 스레드 풀이나 비동기 작업에서 작업의 결과를 참조하는 데 사용되며, 작업이 아직 완료되지 않았더라도 결과에 대한 참조를 얻을 수 있다.
- Callable 인스턴스에서 비동기 작업을 정의하고, 그 작업의 결과를 반환하는 call() 메서드를 구현한다.
- isDone(): 작업이 완료되었는지 여부를 반환한다.
- get(): 작업의 결과를 반환한다. 만약 작업이 완료되지 않았다면, 작업이 완료될 떄까지 블로킹되어 기다린다.
```
ExecutorService executorService = Executors.newSingleThreadExecutor();

Callable<Integer> task = new Callable<Integer>() {
    @Override
    public Integer call() {
        // some long running task
        return result;
    }
};

Future<Integer> future = executorService.submit(task);

// do something else

if (future.isDone()) {
    Integer result = future.get();
}
```

### Locks
- 고급 동기화 도구를 제공한다.
- synchronized 키워드에 비해 더 많은 제어와 유연성을 제공하며, 복잡한 동기화 요구 사항을 다룰 수 있다.
- 그러나 복잡도가 즐가하므로 가능한 synchronized 블록이나 메서드를 사용하는 것이 좋다.
- ReentrantLock class: 가장 일반적인 잠금 메커니즘, 이미 잠금을 보유한 스레드가 같은 잠금을 다시 얻을 수 있다.
```
ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();

// Read Lock
rwLock.readLock().lock();
try {
    // read shared data
} finally {
    rwLock.readLock().unlock();
}

// Write Lock
rwLock.writeLock().lock();
try {
    // write or update shared data
} finally {
    rwLock.writeLock().unlock();
}
```
- ReentrantReadWriteLock: 동시에 읽기 작업을 수행할 수 있도록 허용하면서, 쓰기 작업은 한번에 하나의 스레드에서만 수행하도록 허용한다.
```
ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();

// Read Lock
rwLock.readLock().lock();
try {
    // read shared data
} finally {
    rwLock.readLock().unlock();
}

// Write Lock
rwLock.writeLock().lock();
try {
    // write or update shared data
} finally {
    rwLock.writeLock().unlock();
}
```
- Condition interface: 스레드가 특정 조건이 충족될 때까지 기다리게 하거나 해당 조건이 충족될 때 스레드를 꺠우는 메커니즘을 제공한다.
```
ReentrantLock lock = new ReentrantLock();
Condition condition = lock.newCondition();

lock.lock();
try {
    while (/* condition not met */) {
        condition.await();
    }
    // proceed when condition is met
} finally {
    lock.unlock();
}

// Another thread
lock.lock();
try {
    // Change the condition
    condition.signal(); // Or condition.signalAll()
} finally {
    lock.unlock();
}
```

### Atomic Variables
- 원자적 연산을 수행하는 클래스를 제공한다.
- 원자적이란, 연산이 중단되거나 중간에 방해받지 않는 것을 의미한다.
- Thread-safe하다.
```
AtomicInteger atomicInteger = new AtomicInteger(0);

// Thread 1
int newValue = atomicInteger.incrementAndGet();

// Thread 2
int anotherValue = atomicInteger.incrementAndGet();
```

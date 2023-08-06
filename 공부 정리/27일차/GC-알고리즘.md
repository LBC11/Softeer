### GC 알고리즘 분류
- Throughput Collector: 모든 리소스를 투입해서 Stop the World 시간을 단축시켜 GC를 최대한 빨리 끝내자.
  + Serial GC
  + Parallel GC
  + Praallel GC
- Low Pause(Latency) Collector: Stop the World를 분산하여 수행하여 오버헤드를 줄이고 GC를 수행함과 동시에 작업도 수행하자.
  + CMS
  + G1 GC

### Serial Collector
- 가장 간단한 알고리즘
- 단일 쓰레드를 사용하기에 성능이 좋지 않다
- Minor GC에서는 Mark-Sweep을, Major GC는 Mark-Sweep-Conmpact를 사용한다.
- Java 실행시 옵션 : -XX:+UseSerialGC

### Parallel GC
- java 8 default GC
- 실행 방식은 Serial Collector와 동일
- Minor GC를 멀티 쓰레드로 실행(Major GC는 단일 쓰레드)
- Java 실행시 옵션 : -XX:+UseParallelGC

### Parallel Old GC
- Parallel GC 개선 버전
- Mark-Summary-Compact 사용
- Major GC에서도 멀티 쓰레드 사용
- Java 실행시 옵션 : -XX:+UseParallelOldGC

### CMS(Concurrent Mark-Sweep)
- 다수의 쓰레드를 이용하여 GC 쓰레드가 애플리케이션 쓰레드와 동시에 실행되기 위해 설계된 GC
- STW 시간이 분산되어 체감 pause time이 줄어들었지만, 과정이 매우 복잡해짐
- Java 9 부터 deprecated되었고 java 14부터 사용 중지
- Phases of CMS
  + Initial Mark: Old Generation의 root 객체에서 직접 참조하는 객체들을 빠르게 마킹, STW 발생O
  + Concurrent Mark: marking 된 객체들에 의해 참조되는 객체를 추가적으로 마킹, STW 발생X
  + Precleaning: 다음 단계인 Rescan 단계의 부담을 줄이기 위해서, 전 단계동안 추가/변경된 Object들을 미리 스캔(Scan)해둔다. STW 발생X
  + Remark: 지금까지 Scan되지 않은 모든 Object들을 모두 scan, STW 발생O
  + Concurrent Sweep: 마킹되지 않은 객체들을 sweep하여 Garbage를 청소, STW 발생X
  + Concurrent Reset: 내부 데이터 구조를 재설정하고 다음 주기를 준비하기 위해 초기화, STW 발생X
- 단점
  + CMS GC는 복잡한 알고리즘이며, 동시에 수행되는 작업으로 인해 예상치 못한 상황이 발생할 수 있다.
  + Fragmentation issue: Compaction 작업이 없기에 메모리 단편화 문제로 인해 Full GC가 발생될 수 있다. Full GC에서 Compaction 작업을 진행한다.
  + Floating Garbage: Concurrent Mark 과정이 끝나기 전에 참조가 끊겨 Garbage가 되었다면, GC 대상으로 인식 X, 해당 객체는 다음 GC떄 삭제

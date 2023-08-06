### GC(Garbage Collection)이란
- JVM의 Heap 영역에서 더 이상 필요없는 메모리 객체를 주기적으로 제거하는 프로세스

### GC의 장점
- 기존 C/C++ 언어에서는 사용자가 메모리를 할당/해제해주어야 했는데, GC는 이러한 수고를 덜어준다.
- 한정된 메모리를 효율적으로 사용할 수 있다.
- 메모리 누수 문제에서 벗어나 개발에만 집중할 수 있게 해준다.

### GC의 단점
- GC 프로세스 자체가 CPU 자원을 사용한다.
- GC 작업 중 Stop the World를 통해 모든 애플리케이션 스레드를 일시 중지하여 오버헤드가 발생한다.
- 많은 메모리를 관리해야 하는 대형 애플리케이션의 경우 GC로 인한 오버헤드가 성능에 영향을 미칠 수 있다.

### GC의 대상
- 객체를 제거할지 말지 판단하는 기준으로 도달능력(Reachability)를 사용한다.
- 도달능력은 Root space로부터 시작하여 참조 그래프를 순회하여 각 객체에 도달할 수 있는지 여부이다.
- Root space: Heap 메모리 영역을 참조하는 static variable, method area, native method stack
- Reachable: Root space부터 객체에 도달할 수 있는 상태, 객체가 참조되고 있어 사용중이라고 판단한다.
- Unreachable: Root space부터 객체에 도달할 수 없는 상태, 객체가 참조되지 않아 제거되어야 한다고 판단한다.

### GC 작동 방식
1. Stop the World: GC 관련 스레드를 제외한 모든 애플리케이션 스레드 정지
2. Mark: Root Space 부터 순회를 시작하여 제거해야 하는 객체(Unreachable한) 찾아내서 표시
3. Sweep: 표시된 객체 Heap 메모리 영역에서 제거
4. Compact: 파편하된 메모리를 정리하기 위해 Heap의 시작 주소로 메모리가 할당된 객체를 모으는 과정
- 대부분의 경우 위의 과정을 따르며, Compact 과정의 경우 알고리즘의 종류에 따라 실행하지 않는다.

### GC의 전제(Weak Generational Hypothesis)
- 대부분의 객체는 금방 접근 불가능한 상태가 된다.
- 오래된 객체를 새롭게 참조하는 경우는 매우 적다.
- 대부분의 객체는 일회성이며, 오랫동안 참조되며 사용되는 경우는 적다는 것을 의미한다.

### Heap 메모리 구조
- GC가 일어날 때마다 Heap의 모든 영역을 대상으로 하는 것은 너무 큰 오버헤드가 발생하여 비효율적이다.
- 그렇기에 위의 전제를 이용하여 Heap영역을 다음과 같이 나누어 GC가 발생하도록 하였다.
- Young 영역(Young Generation)
- Old 영역(Old Generation) 

### Young 영역(Young Generation)
- 대부분의 객체가 생성되고 제거되는 영역
- Minor GC가 일어나는 영역
- Eden, Survivor1, Survivor2 영역으로 나눠진다.
- 상대적으로 영역의 크기가 작다

### Eden
- 새롭게 생성되는 객체가 할당되는 영역, 단 객체의 크기가 너무 크면 Old 영역에 할당된다.
- Minor GC에서 살아남은 객체들은 Survivor 영역으로 보낸다.

### Survivor
- 최소 1번의 GC에서 살아남은 객체들이 할당되는 영역
- Survivor1, Survivor2 둘 중 하나만 사용하고 다른 하나는 비어있다.
- Minor, Survivor1에서 Minor GC후 -> Survivor2로 재할당
- Minor, Survivor2에서 Minor GC후 -> Survivor1 재할당

### Old 영역(Old Generation)
- Young 영역에서 오랫동안 살아남은(age가 일정 이상인) 객체가 재할당되는 장소
- Major GC가 일어나는 영역
- 상대적으로 영역의 크기가 크다.

### Minor GC
- Young 영역에서 Eden 영역이 가득 차면 발생
- Young 영역의 크기가 작으므로 실행속도가 빠르다.
- 일정 이상 살아남은 객체는 Old 영역으로 재할당(promotion) 된다.

### Major GC
- Old 영역이 가득 차면 발생
- Old 영역의 크기가 크므로 실행속도가 상대적으로 느리다.

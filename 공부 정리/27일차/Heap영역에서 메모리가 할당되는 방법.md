### bump the pointer
- pointer가 가장 마지막 객체를 가리키고 있어, 남는 영역에 새로운 객체가 할당될 수 있는지 판단 가능

### TLAB(Thread-Local Allocation Buffers)
- 멀티 쓰레드 환경에서 Young 영역에 새로운 객체를 할당할 때 pointer는 하나이기에 동기화를 위해 Lock이 발생한다.
- Lock을 없애기 위해 Young 영역을 작은 Buffer들로 나누어 각 Thread에 할당
- 각 Thread는 주어진 buffer에만 객체를 할당하여 Lock이 발생하지 않는다.

### PLAB(Parallel-Local Allocation Buffers)
- TLAB와 동일한 개념, Old 영역의 Lock을 없애기 위해 고안
- 마찬가지로 영역을 나누어 각 Thread는 주어진 Buffer에만 객체 할당 가능
- 메모리 단편화 문제 발생 가능

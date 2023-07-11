## JVM(java virtual machine)
- java byte code를 실행할 수 있는 런타임 환경
- "Write Once, Run Anywhere"
- 주요 component
    + 클래스 로더(Class Loader): java class를 로드하고 링크를 통해 바이트 코드가 JVM에서 실행되도록 한다.
    + 런타임 데이터 영역(Runtime Data Area)
        - Method Area: 각 클래스와 인터페이스에 대한 런타임 상수 풀, 필드와 메서드 데이터, 메서드의 생성자의 바이트 코드, 그리고 생성자와 메서드의 프레임 데이터등을 저장한다. 모든 스레드에서 공유된다.
        - Heap Area: 모든 클래스의 인스턴스와 배열이 이 영역에 저장된다. 모든 스레드에서 공유된다.
        - Stack Area: 메서드 호출과 로컬 변수 할당에 사용된다. 각 스레드에 자신만의 스택 영역을 가지고 있따.
        - PC Registers: Context switching을 대비해 각 스레드는 자신만의 PC 레지스터가 있으며, 현재 실행중인 JVM 명령의 주소를 저장합니다.
        - Native Method Stack: c 혹은 c++ 코드를 실행하기 위한 스택
    + 실행 엔진(Execution Engine): Byte code를 기계어로 변환하여 실행한다
        - interpreter: Byte code를 기계어로 변환한다.
        - JIT(Just-In-Time) compiler: 자주 사용되는 코드를 미리 기계어로 변환시켜놓고 실행시키는 방식으로 속도를 빠르게 해준다.
- GC(Garbage Collection): 더 이상 참조가 없는 객체들을 heap memory에서 주기적으로 삭제하는 역할을 해준다.

### 번외
```
String s = “hello”
String s2 = new String(“hello”);
```
- s 와 s2가 생성되는 방식은 다르다.
- s는 먼저 constant fool에 "hello"를 생성하고 이것을 참조한다.
- 그렇기에 String s3 = "hello"; 라고 선언하고 둘을 == 으로 비교하면 같은 주소를 참조하므로 true가 나온다.
- s2는 새로운 참조의 객체를 만들고 그 객체의 value를 hello로 설정한다.
- 그렇기에 s3 != s2이다.

```
Public class Hello {

	Hello {
		System.out.println(“22222”);
	}

	static {
		System.out.println(“33333”);
	
	public static void main(Strings[[ args) {

		System.out.println(“11111”);
		Hello hello = new Hello();

	}
	
}
```
- 위의 코드를 실행하면, 33333, 11111, 22222 순으로 출력된다.
- 이유는 먼저 static method(33333)가 실행되고 entry point에 진입하면서 (11111)이 실행 된다
- 그 다음에 Hello class가 선언되면서 (22222)가 실행된다.
- (22222)가 제일 늦게 실행되는 이유는 Lazy loading 이라는 특성 떄문으로, 클래스는 사용되는 시점에 메모리에 로딩되어 사용되는 특성이다.

```
Public class Hello {

	public static int a = 5;

	Hello {
		System.out.println(a);
	}

	static {
		System.out.println(a);
	
	public static void main(Strings[[ args) {

		Hello hello = new Hello();

	}	
} 
```
- 위의 코드를 실행하면 5, 5가 출려된다.
- static 변수는 자동으로 초기화를 제일 먼저 진행한다. 그래서 나중에 실행되는 두 method에서 둘 다 5를 출력한다. 

### java thread의 변천사
1. JDK 1.2 버전까지 100% User-Level-Thread 사용
2. 성능을 이유로 Kernel-Level-Thread 사용으로 변경
3. Java 21부터 Project loom(실짜는 배틀) 도입

### Project loom 도입 이유
- OS에서 제공하는 스레드를 java가 주체적으로 컨트롤하기 어려우며, cost가 높기 때문에
- 기존의 OS 스레드로 비동기 처리하기 힘들어서
- 결론: 경량 쓰레드를 원하는 만큼 쓸 수 있게 하자.

### java에서 thread 사용하는 법
- Thread 클래스 상속
```
class MyThread extends Thread {
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getId() +" Value "+ i);
        }
    }
}

public class Main {
    public static void main(String args[]) {
        MyThread t1 = new MyThread();
        t1.start();
        
        MyThread t2 = new MyThread();
        t2.start();
    }
}

```
- Runnable 인터페이스 구현
```
class MyRunnable implements Runnable {
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getId() +" Value "+ i);
        }
    }
}

public class Main {
    public static void main(String args[]) {
        Thread t1 = new Thread(new MyRunnable());
        t1.start();

        Thread t2 = new Thread(new MyRunnable());
        t2.start();
    }
}
```

### start()
- 메소드를 호출하면 새로운 스레드가 생성되고, 이 스레드는 run() 메소드를 실행합니다.
- 메소드를 호출할 후에 JVM이 이 스레드를 스케쥴링하여 실행한다.
- 그렇기에, start()를 호출하면 해당 스레드는 별도의 실행 흐름을 가지게 되어 다른 스레드와 동시에 실행될 수 있다.

### run()
- 메소드를 호출하면 새로운 스레드가 생성되지 않는다.
- 그래서 run() 메소드를 호출하면, 현재 스레드(run을 호출한 스레드)에서 run() 메소드를 실행한다.
- 그래서 run() 메소드를 직접 호출하면, 다른 스레드와 동시에 실행되지 않고 호출한 스레드의 실행 흐름에 따라 순차적으로 실행된다.

### 스레드 동기화
- 스레드는 일반적으로 Local 변수들은 각자 자신만의 스택에 따로 가지고 있다.
- 그렇기에 Local 변수들은 동기화를 신경 쓸 필요가 없다.
- 하지만 Global 변수들은 동기화에 대한 오류가 발생할 수 있기에 처리 해줘야 한다. 

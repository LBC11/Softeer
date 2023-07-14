### 널 객체 패턴
- 널 참조를 피하고자 사용하는 디자인 패턴
- 메소드에서 null을 반환하는 대신 Null 객체를 반환하여 NullPointerException을 방지한다.
- 널 객체는 인터페이스를 따르지만, 아무런 동작을 수행하지 않는 객체를 말한다.
- 널 객체는 그 자체로 완벽한 객체이므로, 무의미한 널 체크 반복을 피할 수 있다.

```
public interface Animal {
    void makeSound();
}

public class Dog implements Animal {
    public void makeSound() {
        System.out.println("Woof!");
    }
}

public class Cat implements Animal {
    public void makeSound() {
        System.out.println("Meow!");
    }
}

public class NullAnimal implements Animal {
    public void makeSound() {
        // Do nothing
    }
}
``` 

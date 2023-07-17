### Optional 사용 이유
- null이 올 수 있는 값을 감싸는 Wrapper 클래스
- 가장 많이 발생하는 NPE(NullPointerException)를 방지하기 위해 제공
- 결과 없음을 명확하게 드러내기 위해 메소드 반환 타입으로 사용되도록 매우 제한적인 경우로 설계

### Optional 생성
- 값이 null인 경우: Optional<String> op = Optional.empty();
- 값이 null이 아닌 경우: Optional<String> op = Optinal.of("string");
- 값이 null일 수도 아닐 수도 있는 경우: Optional<String> op = Optional.ofNullable(getString());
- Optional 클래스 내부에서 static 변수로 EMPTY 객체를 미리 생성해서 가지고 있으면서 메모리를 절약하고 있다.

### Optional Method
- get(): 값이 존재하면 그 값을 반환하고, 그렇지 않으면 NoSuchElementException 발생
- isPresent(): 값이 존재하면 true, 그렇지 않으면 false;
- isEmpty(): 값이 존재하면 false, 그렇지 않으면 true;
- ifPresent(): 값이 존재하면 값을 이용해 action 실행
- ifPresentOrElse(): 값이 존재하면 값을 이용해 action을 실행하고, 그렇지 않으면 emptyAction을 실행한다.
- orElse(): 값이 존재하면 그 값을 반환하고, 그렇지 않으면 other를 반환한다.
- orElseGet(): 값이 존재하면 그 값을 반환하고, 그렇지 않으면 other가 제공하는 값을 반환한다.
- orElseThrow(): 값이 존재하면 그 값을 반환하고, 그렇지 않으면 NosuchElementException 발생
- orElseThrow(exception Supplier): 값이 존재하면 그 값을 반환하고, 그렇지 않으면 해당하는 Exception 발생

### orElse vs orElseGet
- orElse()
  + 파라미터로 값을 받는다.
  + 값이 있든 없든 항상 인자로 받은 기본값을 계산한다.
  + 값이 미리 존재하는 경우에 사용한다.
  + 오류의 가능성, orElseGet보다 비용이 크므로 최대한 사용을 피하자
    
- orElseGet()
  + 파라미터로 함수형 인터페이스를 받는다.
  + 값이 없을 떄만 함수를 실행하여 기본값을 계산한다.
  + 값이 미리 존재하지 않는 대부분의 경우에는 orElseGet()을 사용한다.
 
```
import java.util.Optional;

public class OptionalExample {
    public static void main(String[] args) {
        Optional<String> optionalWithValue = Optional.of("Hello, World!");
        
        System.out.println("Using orElse:");
        String value = optionalWithValue.orElse(expensiveOperation());
        
        System.out.println("Using orElseGet:");
        value = optionalWithValue.orElseGet(() -> expensiveOperation());
    }

    private static String expensiveOperation() {
        System.out.println("Performing an expensive operation...");
        return "Expensive Value";
    }
}
```
```
// 결과
Using orElse:
Performing an expensive operation...
Using orElseGet:
```

### Optional의 단점
- NPE 대신 NoSuchElementException 발생
- 직렬화를 지원하지 않는다.
- Lambda, Steam 등과 재대로 작동하지 않는 경우가 있다.
- Java 8 이전의 라이브러리, 프레임워크등과 호환성 문제가 발생할 수 있다.
- 코드의 가독성을 떨어뜨린다.
- 시간적, 공간적 비용(오버헤드)가 증가한다: Optional이 Wrapper 클래스라서

### Optional 재대로 사용하는 법
- Optional 변수에 Null을 할당하면 Optional 변수 자체가 null인지 검증해야 한다. 그러니 값이 없는 경우 empty()를 이용하자.
- 값이 없을 떄를 대비해 get() 보다는 orELseGet()을 이용하자
- 단순히 값을 얻으려는 목적으로만 Optional을 사용하지 말자. 기존의 방법으로 충분하고, 사용해봤자 비용만 증가할 뿐이다.
- 생성자, 수정자, 메소드 파라미터 등으로 Optional을 넘기지 말자. null 체크도 해야하고 직렬화를 지원하지 않으니 문제가 발생할 수 있다.
- Collection의 경우 비어있다면 Optional이 아닌 Collections.emptyList()를 사용하자.
- 기존 목적에 맞게 반환 타입으로만 사용하자!!!


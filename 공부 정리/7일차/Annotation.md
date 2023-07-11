### 사용 시기
- 메타데이터 추가: 클래스, 메소드, 변수 등에 메타데이터를 추가할 수 있다. 이 메타 데이터는 컴파일러에게 정보를 제공하거나, 런타임에 리플렉션을 통해 코드의 동작을 변경하는 데 사용될 수 있다.
- 코드 검사: 코드의 올바른 사용을 검사하는 데 사용된다. ex. @Override
- 프레임워크와 라이브러리 통합: 많은 자바 프레임워크와 라이브러리가 어노테이션을 사용하여 사용자 코드와 통합한다. ex. Service
- 설정 정보 제공: 코드 내에 설정 정보를 직접적으로 포함시킬 수 있다. @Test

### 장점
- 타입 안정성: 어노테이션은 컴파일 타임에 type 체크가 가능함로, 문자열 기반의 메타데이터를 사용하는 것보다 안전하다.
- 코드의 명확성: 어노테이션을 사용하면 클래스, 메소드, 변수의 역할과 동작을 명확하게 나타낼 수 있다.
- 보일러플레이트 코드 감소: 프레임워크나 라이브러리를 사용할 때, 어노테이션을 사용하면 반복적인 코드를 크게 줄일 수 있다. ex. spring에서 어노테이션을 통해 의존성 주입, 트랜잭션 관리 등을 간단히 처리한다.

### 단점 
- 과도한 사용: 어노테이션을 사용하여 복잡한 로직을 처리하려고 하면, 코드의 가독성과 유지 보수성이 떨어진다.
- 런타임 오버헤드: 어노테이션은 리플렉션을 사용하여 처리되므로 런타임에 오버헤드가 발생할 수 있다. 대부분의 경우에서는 무시할 수 있을 만큼 작다.
- 호환성: 어노테이션을 사용한 코드는 해당 어노테이션을 지원하는 환경에서만 실행될 수 있다.

### 예시
```
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}
```

```
public class ExampleService {
    @LogExecutionTime
    public void performTask() {
        // Task implementation
    }
}
```

```
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class LogExecutionTimeAspect {
    @Around("@annotation(LogExecutionTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        
        Object proceed = joinPoint.proceed();

        long executionTime = System.currentTimeMillis() - start;
        
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        
        return proceed;
    }
}

```

설명: 위의 코드에서 LogExecutionTimeAspect 클래스는 모든 @LogExecutionTime이 붙은 메소드의 실행 시간을 로깅합니다. 이 클래스는 AspectJ의 @Around 어노테이션을 사용하여 @LogExecutionTime 어노테이션이 붙은 메소드가 호출될 때마다 logExecutionTime 메소드가 실행되도록 합니다.

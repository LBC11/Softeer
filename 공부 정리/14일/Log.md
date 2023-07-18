### Print
- 장점
  + 코드에서 바로 출력 후 확인이 가능하다.
- 단점
  + print문은 console에만 출력이 가능하여, 이를 따로 저장하려면 추가적인 작업이 필요하다.
  + 출력의 on/off를 동적으로 제어할 수 없다.
  + 필요없는 출력을 제거하려면 코드를 직접 수정하고 프로그램을 재실행해야 한다.
  + 모든 메시지가 동일한 우선순위를 가진다. 그래서 중요한 에러 메시지와 일반적인 메시지를 구분하기 어렵다.

### Log
- 장점
  + 로그 메시지를 consloe, file, database 등 다양한 위치에 저장할 수 있다.
  + 실행 중에 로그 설정을 변경할 수 있다. ex. 로그의 레벨을 변경하여 중요한 로그 메시지만 남길 수 있다.
  + 로그 메시지는 시간, 클래스명, 메소드명 등의 유용한 컨텍스트 정보와 함께 출력될 수 있습니다.
- 단점
  + 로깅 시스템의 설정이 print보다 복잡하다. 하지만 이는 일회성의 작업이며, 많은 라이브 러리에서 이를 간단하게 처리하는 기능을 제공한다.
 
### 결론
- 위의 이유로 print보다는 Log 이용을 권장한다.

### Log의 선언 방법
- SLF4J는 인터페이스이고 Logback, Log4J, Log4J2등등이 구현체이다. 무엇을 사용할지는 선택하면 된다.
- 직접 선언하여 사용하는 방법
```
private final Logger log = LoggerFactory.getLogger(getClass());

private static final Logger log = LoggerFactory.getLogger(Xxx.class);
```
- Lombok 이용 방법
```
@Slf4j
public class LogExample {
}

@Log4j
public class LogExample {
}
```

### Log의 레벨
- ERROR
    + 시스템의 요청 또는 작업을 수행하는 데 실패했음을 나타내는 메시지에 대한 로그 레벨
    +  일반적으로 이 레벨의 로그는 모든 상황에서 기록되어야 한다.
- WARN
    + 잠재적인 문제를 나타내는 로그 레밸
    + 에러는 아니지만 무시하면 나중에 식각한 문제가 발생할 여지가 있다.
- INFO
    + 일반적인 진행 상황을 나타내는 로그 레벨
    + 애플레케이션이 정상적으로 작동하고 있음을 보여주는 데 유용하다.
- DEBUG
    + 시스템 동작의 자세한 내용을 추적하는 데 사용되는 로그 레벨
    + 개발 중이거나 문제를 진단할 떄 사용된다.
- TRACE
    + Debug 보다 더 자세한 정보를 제공하는 로그 레벨
    + 매우 상세한 정보를 제공하므로, 대량의 로그 데이터를 생성하므로 주의해서 사용해야 한다. 
- 상세 정도: TRACE > DEBUG > INFO > WARN > ERROR

### 주의할 점
- 메시지를 출력할 때 "+" 연산자를 사용하지 말자
- 출력은 정상적으로 진행되나, Log를 출력할 때 String의 더하기 연산을 진행하여 cost를 잡아먹는다.
- 하나로는 무시할만 하지만, 정말 많은 Log가 출려되는 상황에서는 무시하기 어렵다.
```
// 잘못된 예시
String username = "Alice";
log.debug("User login: " + username);

// 권장하는 예시
String username = "Alice";
log.debug("User login: {}", username);
```

### Mockito란
- java에 대한 테스팅 프레인워크 중 하나로, 단위 테스트를 위한 모의(mock) 객체를 생성하고 관리하는 데 특화
- 이 툴을 이용해, 코드의 측정 부분이 의도한 대로 작동하는지 검증 가능하다.

### 주요 기능
1. 모의 객체 생성: 인터페이스나 클래스의 인스턴스를 모의 객체로 생성
2. 행동 지정: 모의 객체의 메소드가 호출될 때 어떤 행동을 해야 하는지 정의
3. 호출 검증: 모의 객체에 대한 메소드 호출을 추적하고 검증하여 코드가 예상대로 동작하는지 확인
4. 인자 일치자 사용: 특정 인자로 메소드가 호출되었는지, 또는 특정 조건을 만족하는 인자로 메소드가 호출되었는지 확인

```
import static org.mockito.Mockito.*;

// 모의 객체 생성
List mockedList = mock(List.class);

// 모의 객체에 대한 행동 지정
when(mockedList.get(0)).thenReturn("first");

// 모의 객체 사용
System.out.println(mockedList.get(0));  // "first" 출력

// 호출 검증
verify(mockedList).get(0);
```

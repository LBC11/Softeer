### 스태틱 팩토리 메소드(Static Factory Method) 패턴
- 장점
  + 이름이 있어, 가독성이 좋다
  + 호출할 때마다 인스턴스를 새로 생성하지 않는다. 즉, 불필요한 객체 생성을 방지할 수 있다.
  + 하위 타입의 객체를 반환할 수 있다.
  + 생성할 클래스의 객체를 생성할 때 매개변수에 따라 다른 클래스의 객체를 반환할 수 있다.
- 단점
  + public, protected 생성자가 없으므로 하위 클래스를 만들 수 없다.
  + 다른 정적 메서드와 잘 구분되지 않는다.
 
### 빌더(Builder) 패턴
- 장점
  + 매개변수가 많은 경우 코드의 가독성을 높일 수 있다. 각 매개변수에 어떤 값이 설정되는지 명확하게 표현가능하다.
  + 메서드 호출을 통해 원하는 매개변수만 설정하고, 나머지는 기본갑이나 빈 상태로 두는 것이 가능하다.
  + 한 번에 객체를 생성하므로, 불변 객체를 만드는 데 유용하다.
- 단점
  + 코드가 길어진다.
  + 설계 시간이 더 필요하고, 코드의 복잡성이 증가한다.

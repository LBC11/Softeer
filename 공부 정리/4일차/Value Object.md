### 사용 이유
- 객체를 나타내기 위해 primitive type을 사용하는 것은 좋지 않다(primitive obsession)
- 그 객체를 나타내기 위한 자료형을 만드는 것이 좋다

### Value Object 특성
- value를 담는 게 주 목적이다.
- 객체를 구별하는 게 무의미하므로 unique한 구분자를 가지지 않는다.
- 생성되면 value의 상태가 변하지 않는다.(불변성)
- VO는 동등성 검사를 통해 두 객체의 값이 같다면 동일하다고 판단한다.
- VO는 유효하지 않은 값으로 값 객체를 만들 수 없어야 한다.

### Value Object 생성 기준
- VO의 초기 클래스에는 생성자와 private 인스턴스 변수만 있어야 한다.
- 생성자 또는 static factory pattern만을 사용한다.
- 무의미한 getter, settter, default를 지양한다.

### 장점
- Reference로 공유할 수 있다.
- 명확한 이름과 동작을 가진다.

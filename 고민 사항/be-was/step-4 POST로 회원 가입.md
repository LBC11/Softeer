## 구현 내용
- RequestBody를 위한 클래스
- Response의 빈 객체가 생성되고 코드의 흐름에 따라 설정이 추가되는 형식으로 변경

## 고민 사항

1. OCP 원칙을 적용해보려 했다.
- ControllerMapper에서 자동으로 controller directory에서 controller class를 읽어와서 map에 저장하는 로직을 추가했었다.
- 그런데, 적용하고보니 처리해야 하는 예외가 3개 추가되고 코드도 대략 120줄이 추가되었다.
- 현재 controller가 1개뿐이고 앞으로도 추가되는 controller가 그렇게 많지 않은데 너무 오버 엔지니어링이라 판단하여 그 전 버전으로 되돌렸다.

2. Body parsing이 지금으로도 충분한가?
- 현재는 text 형태의 body만 파싱이 가능하다.
- Request의 content-type을 확인하고 그에 따라 파싱 방법을 다르게 하려고 했었다.
- 하지만 위처럼 생각하면 너무 하염없이 추가해야할 요소가 많아서 요구사항을 충족하는 것에 만족했다.

## 기타

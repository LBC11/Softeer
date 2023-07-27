### Http란?
- HyperText Transfer Protocol의 약자로 웹에서 정보를 주고받을 수 있는 프로토콜
- HTML, TEXT, IMAGE, JSON, 음성, 연상, 파일 등 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP를 사용한다.

### Http의 역사
- Http/0.9: Get 메서드만 지원, 헤더X
- Http/1.1: 가장 많이 사용, keep alive등 대부분의 기능이 여기 존재
- Http/2.0: 성능개선
- Http/3.0 진행중: TCP 대신 UDP사용하여 성능 개선하려 함.

### Http 특징
- Client, Server 구조
- 무상태 프로토콜(Stateless)
- 비연결성
- 단순하여 확장에 용이함

### Client, Server 구조
- Client는 Server에 request를 보내고, 응답을 대기
- Server는 request에 대한 response를 만들어서 응답
- 두 개념를 분리하여 독립적으로 발전 개발 가능!!!

### 무상태 프로토콜(Stateless)
- 서버가 클라이언트의 상태를 보존하지 않는 방식
- 장점: 서버에 많은 user가 몰리는 경우 scail out(수평적 확장)등을 통해 서버를 확장하여 대응할 수 있다.
- 단점: Request에 필요한 모든 정보를 담아야 하기에 추가 데이터를 전송해야 한다.
- 하지만, 모든 것을 무상태로 설계할 수 없다.
- 로그인과 같이 유저의 상태를 유지해야 하는 경우는 브라우저 쿠키, 서버 세션등을 사용하자.
- 상태 유지는 cost가 높기 때문에 최소한만 사용하자.

### 비연결성
- Http는 기본적으로 연결을 유지하지 않는다.
- 무상태이기에 어떠한 서버라도 요청 처리가 가능하다.
- 장점
  + 효율성: 연결에 사용하던 자원을 다른 요청에 사용할 수 있다.
  + 확장성: 비연결성과 더불어 서버가 많은 수의 클라이언트와 통신할 수 있게 한다.
- 단점
  + 비용: 연결(3-way handshake), 해제(4-way handshake)의 비용이 크다.
  + 위의 단점을 극복하기 위해, Http/1.1에서 keep alive 기능을 추가했다.

### Http Request 메시지
- start-line: Http method, 요청 대상, http version 포함
- Http header: Http 전송에 필요한 모든 부가정보 ex. 메시지 바디 내용, 메시지 바디 크기 등
- Http 메시지 body: 실제 전송할 데이터, ex. 문서, image, Json data등 byte로 표현할 수 있는 모든 데이터

### Http Response 메시지
- start-line: Http version, Http status code
- Http header: Http 전송에 필요한 모든 부가정보 ex. 메시지 바디 내용, 메시지 바디 크기 등
- Http 메시지 body: 실제 전송할 데이터, ex. 문서, image, Json data등 byte로 표현할 수 있는 모든 데이터

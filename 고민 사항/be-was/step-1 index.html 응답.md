## 구현 내용
- HttpRequest class
     + Request를 처리하기 위한 클래스 구현
     + method는 enum으로 구현
     + Header는 map을 1급 컬레션으로 감싸서 구현
- HttpResponse class
     + Response를 처리하기 위한 클래스 구현
     + HttpStatus enum으로 구현
- View, ViewResolver
     + path에 맞는 file을 찾기 위한 viewResolver interface 구현
     + rendering을 담당할 View interface 구현
     + 정적 파일을 위한 StaticViewResolver, StaticView 구현
- Dispatcher class
     + request를 parameter로 받아 최종적으로 response 객체를 만들어 반환하기 위한 클래스
- StringUtils
     + 개행 문자 중복을 제거하기 위한 StringUtils 구현 
## 고민 사항
1. 도대체 어떻게 접근을 해야할까?
- 이런 http를 이론적으로만 공부했지 직접 구현하는 것에 대해 해본적이 없어서 어떻게 시작해야 하나 막막했다.
- 먼저 현재 주어진 코드를 다음과 같이 분석했다.
     + WebServer의 main 메소드에서 시작
     + 만약 실행 당시 argument가 주어졌다면 port를 해당 argument로 변경, 아니라면 default port 8080 사용
     + ServerSocket 객체를 생성하여, port 번호에서 클라이언트의 연결 요청을 기다린다.
     + while문을 이용해 계속해서 연결 요청을 받아드린다.
     + 연결 요청이 들어오면 listenSocket.accept()를 이용해 요청을 수락하고 connection 이라는 socket을 생성한다.
     + 요청을 처리하는 RequsetHandler를 생성하고 socket을 argument로 넘겨준다.
     + 새로운 스레드를 생성하고 그 스레드에서 httpRequestHandler를 실행한다.
     + RequestHandler에서 Hello world를 반환하는 response를 생성하고 응답

- 그런 다음에 필요한 역할과 그것들의 메시지를 분석하여 다음과 같은 클래스가 필요하다고 생각했다.
     + Request를 parsing할 클래스
     + Response를 만들어서 응답을 보내는 클래스
     + Request의 요청에 따라 과정을 처리할 방법을 찾아주는 클래스
     + 실제로 요청을 처리하는 클래스
     + 요청에 담긴 정보를 담을 클래스
     + 결과를 담을 파일을 찾아줄 클래스
     + 파일을 rendering할 클래스
- 최종적으로 내 분석에서 부족한 부분을 찾고 실제로 어떻게 구현해야 하는지 알기 위해 Spring의 코드를 공부한 결과 앞으로 다음의 클래스들이 하는 역할을 구현할 예정이다.
     + Request
     + Response
     + ControllerMapper
     + Controller
     + DispatcherServlet
     + ViewResolver
     + View
- 물론 지금은 초기 단계이기에 위의 클래스들을 모두 구현하지도 않았고 완벽하게 할 수 있을까 의문이 들지만 최선을 다해보자!!!

2. 어떻게 해야 동시성 문제가 발생하지 않을까?
- 먼저 Concurrent package, Thread에 대해서 공부해보았다.
- 그 결과 Thread가 각각 가지고 있는 statck에 저장되는 지역 변수를 최대한 사용하고 동시성 문제를 일으킬 수 있는 전역 변수는 지양할 생각이다.
- 만약 전역 변수를 사용해야 한다면 int와 같은 변수는 Atomic으로 map과 같은 자료구조는 ConcurrentHashMap을 사용하려고 한다.
 
3. Path와 같은 클래스는 field로 주면 안 되는 건가?
- 이 의문은 View관련 부분을 구현하다 가지게 되었다.
- 평소처럼 field를 String으로 구현했었는데, ViewResolver에서 View를 return할 때 Path를 String으로 바꾸고, 정작 View에서는 다시 Path로 바꾸어 사용하는 것이다.
- 생성자에 String이 아닌 Path를 사용한다면, 분명 변환과정에서 생기는 cost를 줄일 수 있어보였다.
- 추가로 공부해 본 결과 다음과 같은 단점들이 존재했다.
- 먼저, Path와 같은 클래스는 직렬화 과정을 지원하지 않는다.
- 또한 Path는 환경에 의존적이기에 에러를 일으킬 수 있다는 것이었다.
- 그렇기에 field의 type을 String으로 결정했다.
  
4. 테스트를 어떻게 할 것인가?
- 저번 Chess 미션을 진행하면서 테스트의 필요성을 refactoring하는 중에 느끼게 되었다.
- 그런데 이번 프로젝트를 구현하다보니 반환타입이 void가 아닌 것은 private이고, public 메소드들은 dos에 메소드의 기능을 write하는 형식이라서 기존의 방식으로는 테스트 하는 것에 한계를 느꼈다.
- 이 부분에서는 Postman을 이용하라는 조언을 받았다.
## 기타

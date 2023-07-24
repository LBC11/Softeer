## 구현 내용
- UserController class
     + User domain을 다루기 위한 controller 클래스
     + ModelAndView를 반환타입으로 사용
- UserDto class
     + User model을 사용하기 위한 Dto 클래스  
- UserRepository class
     + Database를 application 단에서 사용하기 위한 클래스
     + addUser()
     + findUserById()
     + findAll()
- ControllerMapper class
     + 미리 controller 클래스를 map에 저장
     + getController(): RootPath에 맞는 Controller 반환
     + getMethod(): controller에서 request에 맞는 method 반환
- Exception
     + InvalidPathException
     + InvalidQueryParameterException
     + InvalidVersionException
- ModelAndView class
     + 반환할 resource 이름과 사용할 Model 정보를 가진 Value Object
- Controller annotation
     + Controller임을 나타내기 위한 annotation
     + Runtime동안 유지
     + Class를 대상
     + Value: RootPath
- RequestMapping annotation
     + Method의 path와 HttpMethod를 표시하기 위한 anntation
     + Runtime동안 유지
     + Method를 대상
     + method: HttpMethod, value: fullPath
- 위의 요소들을 구현하여 전체적인 구조 완성
## 고민 사항
1. Webserver 와 실제 도메인을 다루는 application 을 나누어 보자
- Controller 등의 클래스를 구현하다보니 구조가 너무 난잡하여 정리할 필요성을 느끼게되었다.
- 그래서 이와 같이 실제 도메인을 다루는 부분을 정리하기 위해 application 폴더를 만들고 그 안에 정리하였다.
- 문제는 view를 처리하는 부분처럼 어디 두기 애매한 클래스들이 존재한다는 것이었다.
- 또한 model을 application 안에 두어야 할지 아니면 지금처럼 밖에 빼놓아야 하는지도 고민이었다.
- 먼저 application 폴더 안의 클래스들은 domain과 실제로 관계되어 다루는 클래스가 들어간다고 생각하였기에 view와 같이 애매한 친구들은 webserver 폴더안에 배치하였다. 
- model은 DB에 존재하는 스키마같은 존재라고 간주하고 application안에는 model의 Dto 클래스를 만들어 넣는 것으로 결론을 내렸다.

2. 스레드 간에 데이터를 어떻게 공유할 것인가?
- Database class, UserController class 처럼 각 스레드가 공유해야 하는 클래스들이 존재한다.
- 처음에는 생성자에서 초기화하는 방법을 사용했지만, 이는 각 스레드마다 초기화가 반복되므로 쓸데없는 메모리가 낭비된다고 판단했다.
- 그래서 private static final로 클래스가 load 될 때 생성되어 지속적으로 공유되는 방식을 사용
- Container, Bean 을 이용하여 Dependency injection을 하면 좋을 텐데... 어떻게 해야 하는지 더 공부해보자 

3. Path를 어떻게 처리할까?
- 처음에는 Path를 "/"를 기준으로 하나하나 찾아들어가는 방식을 사용하려 했다.
- 하지만 이는 n개의 경로가 들어오는 경우를 모두 처리해야 하므로, 너무 나중을 생각하기도 하고 구현이 어렵다고 판단하였다.
- 그래서 첫 부분만 RootPath로 Controller annotation의 value로 사용하고 나머지는 fullPath를 이용하는 방식으로 구현하였다. 

4. 정적 리소스를 먼저 찾아야 하나? 아니면 컨트롤러를 먼저 찾아야 하나?
- 처음에는 컨트롤러를 먼저 찾아보고 없으면 정적 리소스를 찾는 형식으로 구현했다.
- 그런데 /user/login에 존재하는 정적 리소스 파일은 RootPath가 /user이기에 Controller 쪽으로 갔다가 path에 맞는 Controller가 없다는 error가 터지면서 정적 리소스 파일에 접근할 수 없는 문제가 발생했다.
- 그래서 정적 리소스를 먼저 탐색하고 controller를 탐색하는 방식으로 변경하였다.

5. Exception 처리는 어떻게 할 것인가?
- 처리해야 하는 예외의 종류가 매우 다양하다.
- 이를 어디서 catch하고 처리해야 예외 transaction을 해결하면서 깔끔하게 처리할 수 있을까 고민중이다.
- 아직 확실하게 방법을 정하지는 못했고 더 고민중이다. 

6. Optional을 재대로 사용하고 있는가?
- Return type으로만 제한적으로 사용하라고 공부했기에 null을 반환할 수 있는 모든 경우에 Optional을 사용했었다.
- 하지만, 본능적으로 코드에서 bad smell이 풍기기 시작했다는 것을 느꼈다.
- 더 자세하게 분석해보니 error가 일어나야 함에도 굳이 Optional.empty를 반환하고, 반환 받은 곳에서 비어있는지 확인하고 error를 발생시키고 있었다.
- 더 공부해보니, Optional은 return type이면서, 값이 비어있을 경우 특정 행위를 한다거나, default value를 주고 싶은 등 추가적인 행위가 필요한 경우에 사용해야 했다.

## 기타

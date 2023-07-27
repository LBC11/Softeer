## 구현 내용
- Session Manager
     + Session을 관리하는 클래스
     + ConcurrentHashMap을 이용해서 session 저장
- Session 
     + SessionId를 저장하고 있는 Value Object
- Cookie
     + Header의 한 부분인 Cookie를 다룰 클래스
     + map을 이용해서 Cookie의 option을 저장하는 일급컬렉션
- CookieOption
     + Cookie의 Option을 위한 상수를 저장한 클래스

## 고민 사항
1. Cookie를 생성하고 관리할 주체는 어떤 클래스여야 하는가?
- 처음에는 Request, Response 객체의 field에 Cookie 클래스를 추가하여 관리가 되게했다.
- 하지만, Cookie는 Header의 한 부분으로 생명주기가 Header에 종속되는게 맞다고 생각하였다.
- 추가적으로 Request, Reponse 단에서 관리하면 초기화, Option추가의 기능을 두 곳 모두에서 구현해야 했지만 Header에서 관리하면 이곳에서만 구현하고 두 곳은 호출하는 형식으로 해도 된다는 장점이 있었다.

2. Session Manager에서 어떤 자료구조를 사용할까?
- 너무 당연하게 ConcurrentHashMap을 쓰고 넘어가는 나 스스로의 모습에 의문이 들어 고민해보았다. 
- 먼저 단점을 살펴보았다.
     + 당연하지만, map 구조는 key, value 두 개를 저장해야 하기에 메모리를 더 많이 사용한다.
     + 성능 문제: ConcurrentHashMap은 동시성을 해결해주지만 성능에 영향을 미치는 건 동일하다.
     + 충돌 가능성: Hash를 이용하기에 당연히 충돌가능성이 존재하지만 이는 확률적으로 극히 미미하므로, 무시할만하다. 
- 그 다음, 대체할 수 있는 자료구조를 알아보고 위의 단점을 극복해줄 수 있는지 알아보았다..
     + BlockingQueue, ConcurrentLinkedQueue, ConcurrentSkipListSet 등이 있다.
     + 당연하게도 value 한 가지만 저장하기에 메모리 사용량이 더 적다.
     + 동시성 문제를 해결하기 위해 성능에 영향을 미치는 것은 동일했다.
     + 충돌 가능성은 존재하지 않는다.  
- 결론적으로 장단점을 비교해본 결과, 충돌 가능성 제로, 적은 메모리 사용량등의 장점은 순회를 해야 한다는 큰 단점 때문에 원래대로 ConcurrentHashMap을 사용한기로 했다.  

## 기타

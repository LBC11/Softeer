## 구현 내용
- Piece class
  + Pawn class 에서 Piece로 이름 변경
  + value object 패턴 사용
  + 팩토리 메소드 패턴 사용
  + isBlack(), isWhite() 구현
- Board class
  + Piece의 수정에 따른 변경
  + Pawn을 제외한 기물 배치 구현
- StringUtils
  + Uppercase 추가
- Chess
  + board가 프린트 되지 않는 에러 수정

## 고민 사항
- Piece 안의 Color, Type enum을 밖으로 빼야 하는가?
  + Board class에서도 위의 enum을 사용하기에 Piece안의 정보를 가져와서 사용한다는 점에서 결합도가 증가하지 않을까 고민
  + Piece class가 애초에 value object로 data를 제공해주는 객체이기에 지금의 상태로도 괜찮다는 결론을 내렸다.
- Factory method가 지금으로도 충분한가?
  + 관련 블로그를 읽으면서 추상 클래스를 이용하는 방법, 인터페이스를 이용하는 방법이 있었다.
  + 이 중 어떠한 방식이 좋을까 더 고민해보자.

## 기타

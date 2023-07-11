## 구현 내용
- Piece
  + 기존의 enum(Color, Type) constants class로 추출
  + 빈 칸 피스 type 이름, 색 이름 변경
- Board
  + emptyInitialize() 구현
  + 기물과 색에 따른 기물들의 개수 반환 기능 구현
  + "a8" 과 같은 position을 받아 해당 위치의 기물 반환 기능 구현
  + 임의의 기물을 체스판 위에 추가하는 기능 추가
  + 기물의 점수가 높은 순으로 정렬 기능 추가
- 추가된 클래스/enum
  + Constant class 추가
  + SortOrder enum 추가

## 고민 사항
- 객체지향의 원칙을 어디까지 지켜야 할까???
  + 캡슐화를 엄격하게 지키자면 piece의 속성인 type, color에 다른 class가 접근하여 직접 사용하면 안 된다.
  + 미션지의 isBlack(), createWhitePawn() 등의 메소드는 위의 원칙을 고려한 매소드로 보인다.
  + 그래서 type 역시 board class에서 접근하는 것을 방지하기 위해 isPawn()등의 메소드를 추가하였었다.
  + 하지만 위의 방식을 고수하다보니 중복이 하염없이 증가하는 문제가 발생했다. type은 6가지이고 color는 2가지이다. 그래서 createPiece만 12개가, isPawn()과 같이 확인을 하는 메소드가 8개가 생겼다.
  + 여기서 representation, point를 가져오는 method를 추가하려다 무언가 잘못되었다는 생각이 강하게 들었다.
  + color, type enum을 piece 밖으로 빼내기만 하면 위의 무수히 중복되는 메소드들은 매우 간결해진다.
  + 리팩토링의 기준 중 하나는 최소한의 클래스, 최소한의 메소드를 통해 간결하게 코드를 작성하는 것이다. 그런데 객체지향을 준수함으로써 이 기준을 달성할 수 없다면 어떤 것을 따라야 할까?
- Comparator는 어디에 두어야 할까??
  + 처음 생각한 곳은 Constants 에 추가하여 color, type과 같이 작성하려 했다.
  + 하지만 piece object를 어떻게 비교하는지 다른 클래스에서 굳이 알 필요가 없고, color, type과 같이 직접적으로 데이터를 불러와 사용하는 것이 아니기에 piece class 내에 작성하였다.
- 현재의 project에서 interface를 적용하는 게 맞을까??
  + interface 역시 위의 객체지향과 같이 유지 보수를 쉽게 하기 위한 하나의 방법이다.
  + a,b,c,d 의 기능이 필요할 때 각 기능의 interface를 구현하고 사용한다면 a를 수정해야 할 때 a의 구현체를 새로 작성하거나 수정하면 된다.
  + 만약 chess project에 적용한다면 Board interface, PieceFactory interface, Piece interface를 만들고 사용하면 된다.
  + 하지만 이 project는 체스의 규칙이 바뀌지 않은 한 수정이 이루어지지 않기에 interface를 적용한다고 해도 복잡성만 증가할 뿐 크게 이점이 없어 보인다.
- 모든 것에서 얻는 것이 있다면 잃는 것이 생기는 tradeoff가 존재한다.
  + 객체지향 등 많은 패러다임이 존재하지만, 항상 정답인 것은 존재하지 않는다.
  + 이를 명심하고 왜 그것을 사용해야 하는지? 사용하면 뭐가 좋은지? 어떠한 단점이 있는지? 충분히 고민하고 적용하는 실력을 쌓자!!!

## 기타

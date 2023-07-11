### 개행문자
- 윈도우와 MAC의 개행문자는 다르기에 종종 오류를 발생시킨다.
- 이러한 문제점을 해결하기 위해 "\n"을 쓰기 보다는 System.getProperty("line.separator") 을 사용하자.

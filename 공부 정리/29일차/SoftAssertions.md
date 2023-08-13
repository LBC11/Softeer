### 등장 배경
- 하나의 method에서 일반적인 assert를 이용한 검증이 여러 개일 경우 앞의 검증에서 실패할 경우 뒤의 검증을 진행하지 않고 중단된다.

### SoftAssertions의 특징
- 테스트 중단 없이 모든 검사 수행: 하나의 테스트 케이스 내에서 여러 검증을 수행하더라도 중간에 오류가 발생해도 테스트는 계속 진행된다.
- 통합된 오류 보고: 테스트가 완료된 후에 모든 실패 사항을 한꺼번에 보여준다.
- 코드의 가독성 향상: 여러 검증 사항을 명확하게 표현한다.

```
SoftAssertions softAssertions = new SoftAssertions();
softAssertions.assertThat(someValue).isEqualTo(expectedValue1);
softAssertions.assertThat(anotherValue).isEqualTo(expectedValue2);
softAssertions.assertAll();  // 모든 검증 사항을 검토하고 오류가 있으면 여기서 실패를 보고합니다.
```

### SpringBootTest에서 사용 방법
- @ExtendWith(SoftAssertionsExtension.class): Junit5의 테스트 확장 모델과 SoftAssertions을 자동으로 통합한다.
- @InjectSoftAssertions: 자동으로 SoftAssertions 인스턴스를 주입받는다.
- 위의 방식으로 주입받은 SoftAssertions를 사용하면, 테스트 마지막에 자동으로 assertAll()이 호출된다.

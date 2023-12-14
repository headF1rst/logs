#papercut 

파라미터로 한번에 테스트하고 싶은 메서드를 람다식으로 넣는다.

```java
@Test
@DisplayName("스터디 만들기")
void create_new_study() {
		Study study = new Study();

		assertAll(
				() -> assertNotNull(study);
				() -> assertEquals(StudyStatus.DRAFT, study.getStatus(), 
								() -> "스터디를 처음 만들면" + StudyStatus.DRAFT + " 상태다.");
				() -> assertTrue(study.getLimit() > 0, "스터디 최대 참석 인원은 0보다 커야한다.")
		);
}
```

`assertThat`을 단건으로 사용하면 테스트 실패시 바로 종료되어 그 이후에 나오는 테스트 구문까지 실행되지 않지만, `assertAll`로 감싸면 테스트가 실패해도 전체 테스트를 실행하고 결과를 출력해준다는 장점이 있다.

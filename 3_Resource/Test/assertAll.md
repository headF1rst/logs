`assertThat`을 단건으로 사용하면 테스트 실패시 바로 종료되어 그 이후에 나오는 테스트 구문까지 실행되지 않지만, `assertAll`로 감싸면 테스트가 실패해도 전체 테스트를 실행하고 결과를 출력해준다는 장점이 있다.
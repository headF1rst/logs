#papercut 

리소스의 특정 부분만 변경한다.

리소스를 완전히 덮어버리는 [[PUT]]과 달리 요청에 `username` 필드가 빠져있더라도 `username` 필드가 삭제되지 않는다.

![[스크린샷 2023-10-20 오후 7.30.38.png]]
![[스크린샷 2023-10-20 오후 7.30.47.png]]

몇몇 서버에서는 `PATCH`를 못받는 경우도 있다. `POST`를 대안으로 사용하면 된다.
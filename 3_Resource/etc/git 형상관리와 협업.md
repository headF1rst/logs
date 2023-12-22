
형상관리는 회사의 자산이며 신규 입사자를 위한 중요한 이력이다.

하나의 PR에 커밋이 너무 많으면 동료들이 리뷰하기 어렵다.
여러 커밋에 걸쳐서 동일한 클래스가 수정되어있다면 해당 클래스를 어떤 의미로 수정했는지 동료가 파악하기 어렵다.

draft PR 올리고 시작

draft PR -> open -> reset인가 [[rebase]]를 통해서 리뷰하기 편하게 커밋정리.

한 클래스는 한 파일에서만 수정된다. 
(한 클래스는 한 커밋에서만 수정된다는걸 말하고 싶었던듯?)

코드 리뷰 피드백을 반영하면서 다시 
클래스가 여러 커밋에서 수정될 수 있다. 

커밋1: User 작업
커밋2: User 등록 작업.
...
커밋: (리뷰반영) ~~ << User 수정
커밋: (리뷰반영) ~~ << User  수정

이런 경우에는 커밋 정리를 다시한다.

develop 머지 전에 rebase 필수 (settings > always suggest updating pull request branches 켜는것도 방법)

https://www.youtube.com/watch?v=su-rtXzThjE&t=1412s
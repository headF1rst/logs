`suspend` 지시어가 붙은 suspending function은 다른 suspending function을 부를 수 있는 능력이 생긴다.

[[코루틴 빌더]]에서 다른 함수를 받을 때, 이 삼수는 suspend 함수로 간주된다. -> suspending lambda

`suspension point` : 코루틴이 **중지 되었다가 재개될 수 있는 지점** (중지 될 수도 있는). suspend 함수도 suspension point이다.

### suspend 함수 활용


### 코루틴 라이브러리에서 제공하는 suspend 함수

#papercut 

`Closure`: [[3_Resource/Kotiln/lambda|lambda]]가 시작하는 시점에, 람다가 참조하고 있는 변수들을 모두 포획한 데이터 구조.

덕분에 코틀린은 진정한 1급 시민이 될 수 있으며 자바의 [[3_Resource/Java/lambda|lambda]]의 제약사항과 달리 다음 코드가 가능하다.

```kotlin
var targetFruitName = "바나나"
targetFruitName = "수박"

fiterFruit(fruits) { it.name == targetFruitName }
```
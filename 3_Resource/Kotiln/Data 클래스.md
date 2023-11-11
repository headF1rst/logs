#papercut 

`data` 키워드를 class 앞에 붙여주면 [[eqauls & hashCode]], [[toString]]을 만들어준다.

named argument까지 활용하면 [[빌더]] 패턴을 사용할 수도 있다. (물론 data 클래스여서 [[빌더]] 패턴을 사용할 수 있는건 아니다.)

```kotlin
data class PersonDto(
	val name: String,
	val age: Int,
)

fun main() {
	val dto1 = PersonDto(age = 20, name = "고산하") // named argument
	val dto2 = PersonDto("고산하", 20)

		println(dto1 == dto2)
}
```
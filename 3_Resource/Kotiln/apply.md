#papercut 

[[Scope Functions]] 중 한 종류. [[3_Resource/Kotiln/lambda|lambda]]를 받아, 객체 그 자체가 반환된다.

객체 설정을 할 때 객체를 수정하는 로직이 call chain 중간에 필요할 때 사용된다.
### Test Fixture를 만드는 경우

`Person`의 `hobby`가 어떤 요구사항에 의해서 생성자에 존재하지 않을 수 있다. (`Person`의 최초 생성 시점에는 `name`과 `age`만 받다가, 회원가입 이후에 `hobby`를 추가적으로 받는 경우.)

이런 `Person`에 대한 test fixture를 만들어야 할 때 `apply`를 통해 `hobby`를 추가로 설정해 줄 수 있다.
```kotlin
fun createPerson(
	name: String,
	age: Int,
	hobby: String,
): Person {
	return Person(
		name = name,
		age = age,
	).apply {
		this.hobby = hobby
	}
}
```
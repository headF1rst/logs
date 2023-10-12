
`Type Alias`: 타입에 **별명**을 붙일 수 있게 해준다.

```kotlin
fun handleCacheStore(store: Map<PersonDtoKey, MutableList<PersonDto>>) {
}
```

store의 타입은 상당히 길고 store 변수의 타입을 여기저기 계속 갖고가야 하기 때문에 번거롭다.

`type alias`를 통해 **긴 제네릭 클래스 타입을 대체할 수 있다.**
```kotlin
typealias PersonDtoStore = Map<PersonDtoKey, MutableList<PersonDto>>

fun handleCacheStore(store: PersonDtoStore) {
}
```


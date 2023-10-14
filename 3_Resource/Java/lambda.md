#papercut 

`lambda`: 이름없는 함수. 메서드 자체를 직접 파라미터로 넘겨주는 **것처럼** 쓸 수 있다. (실제 인자를 받는 것은 `Predicate` 인터페이스이다.)

즉, 자바에서 함수(람다)는 변수에 할당되거나 파라미터로 전달할 수 없다. (2급 시민) (코틀린의 [[3_Resource/Kotiln/lambda|lambda]]는 1급 시민)

```java
// 람다식
변수 -> 변수를 이용한 함수
(변수1, 변수2) -> 변수1과 변수2를 이용한 함수
```

```java
filterFruits(fruits, fruit -> fruit.getName().equals("사과"))
```

```java
private static List<Fruit> filterFruits(List<Fruit> fruits, Predicate<Fruit> fruitFilter) {
	return fruits.stream()
		.filter(fruitFilter)
		.collect(Collectors.toList());
}
```

람다는 `메서드 레퍼런스`를 활용할 수도 있다.
### 변수 제약 사항

자바의 람다에서 람다 밖에 있는 변수를 사용하는 경우 `final`인 변수 혹은 실질적으로 final인 변수만 사용할 수 있다.
```java
String name = "킴"
name = "손"

filterPlayer(players, (player) -> name.equals(player.getName()));
```

단, 코틀린의 [[3_Resource/Kotiln/lambda|lambda]]는 [[Closure]]를 사용하기 때문에 이러한 제약사항이 없다.


`lambda`: 이름없는 함수. 메서드 자체를 직접 파라미터로 넘겨주는 **것처럼** 쓸 수 있다. (실제 인자를 받는 것은 `Predicate` 인터페이스이다.)

즉, 자바에서 함수(람다)는 변수에 할당되거나 파라미터로 전달할 수 없다. (2급 시민)

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
### 코틀린의 람다

반면, 코틀린에서는 함수 자체가 값이 될 수 있다. 변수에 함수를 할당할수도, 파라미터로 넘길 수도 있다. (1급 시민)


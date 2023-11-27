#papercut 

`Primitive Type`은 기본값을 가지며 **스택에 저장**된다. 

따라서 메서드의 매개변수로 전달되더라도 해당 메서드에서 별도의 스택에 다른 주소값을 가진 독립적인 변수로 선언된다.

```java
public static void main() {
	int num = 1;
	add(num);
	println(num); // 1
}

public void add(int num) {
	num = num + 1;
	println(num) // 2
}
```

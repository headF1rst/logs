#papercut 

`record`: 필드 유형과 이름만 필요한 불변 데이터 클래스.

자바 16부터 도입된 `record` 타입을 사용하면 불변 객체 생성에 필요한 보일러 플레이트 코드를 제거할 수 있다.

기존 불변 객체 생성을 위한 코드는 다음과 같다.
```java
public class Person {
	private final String name;
	private final String address;

	public Person(String name, String address) {
		this.name = name;
		this.address = address;
	}

	@Override
	public int hashCode() {
		return Objects.hash(name, address);
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj) {
			return true;
		} else if (!(obj instanceof Person)) {
			return false;
		} else {
			Person other = (Person) obj;
			return Objects.equals(name, other.name)
			 && Objects.equals(address, other.address);
		}
	}

	@Override
	public String toString() {
		return "Person [name =" + name + ", address = " + address + "]"
	}
}
```

```java
record public class Person(
	String name,
	String address
) {
}
```


https://colevelup.tistory.com/28
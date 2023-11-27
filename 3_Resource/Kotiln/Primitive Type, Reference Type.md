#papercut 

코틀린은 기본적으로 모든 변수를 **null이 들어갈 수 없게 끔** 설계하였다.

null을 포함할 수 있는 변수는 아예 다른 타입으로 간주하며 `타입?`을 사용해서 표현한다.

코틀린은 [[Reference Type]]만을 사용하도록 설계된 것 처럼 보이지만 내부적으로는 상황에 따라 타입을 다르게 사용한다.

자료형에 `null`이 들어갈 수 있다면 [[Reference Type]]으로 선언하고
자료형에 `null`이 들어갈 수 없다면 [[Primitive Type]]으로 선언된다.

자바에서는 연산시 [[Reference Type]]을 사용하게 되면 boxing과 unboxing이 일어나서 불필요한 객체 생성이 발생한다.
코틀린의 경우 숫자, boolean, 문자에 대해서는 겉으로 [[Reference Type]]을 사용하지만, 상황에 따라서 [[Primitive Type]]을 써준다.

코틀린이 알아서 처리하기 때문에 boxing/unboxing을 고려하지 않아도 된다.

https://www.devkuma.com/docs/kotlin/datatype/primitive-reference/
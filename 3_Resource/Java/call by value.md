#papercut 

Java는 Call by Value.

자바에는 `Call By Reference`가 없다

객체를 메서드의 파라미터로 넘길 때 직접적인 참조를 넘기는 것이 아니라 주소값을 **복사해서** 넘기기 때문에 메서드 내에서 객체의 값을 변경한 것이 원본에 반영이 되더라도 `Call by Value` 이다.

Java에는 [[Primitive Type]]과 [[3_Resource/Java/Reference Type]]이 존재한다.
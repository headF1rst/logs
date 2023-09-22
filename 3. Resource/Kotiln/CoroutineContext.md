`CoroutineContext`: 코루틴과 관련된 여러 **데이터**를 갖고있다.

### Map과 Set의 특징을 가진 자료구조와 같다
- CoroutineContext에 저장되는 데이터는 `key - value`로 이루어져 있다.
	- key - value 하나하나를 `Element` 라 부른다.
	- Element를 `+` 기호를 통해 합치거나 context에 Element를 추가할 수 있다.
```kotlin
// + 기호를 통한 Element 합성
CoroutineName("나만의 코루틴") + SupervisorJob()

// context에 Element를 추가
coroutineContext + CoroutineName("나만의 코루틴")
```
- Set과 비슷하게 동일한 Key를 가진 데이터는 하나만 존재할 수 있다.
- `minusKey()` 함수를 통해 Context에서 Element를 제거할 수 있다.
```kotlin
coroutineContext.minusKey(CoroutineName.Key)
```

`Dispatcher`: 코루틴이 어떤 스레드에 배정될지를 관리한다.

[[CoroutineContext]]에 포함되는 대표적인 데이터.
### Dispatcher의 종류

- `Dispatchers.Default`: CPU 자원을 많이 쓸 때 권장.
- `Dispatchers.IO`: I/O 작업에 최적화
- `Dispatchers.Main`: 보통 UI 컴포넌트를 조작하기 위한 디스패처.
- Java의 스레드풀인 ExecutorService를 디스패처로 변환
	- `asCoroutineDispatcher()` 확장함수를 이용해 ExecutorService를 디스패처로 전환할 수 있다.
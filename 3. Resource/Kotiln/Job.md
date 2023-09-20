## Job을 통한 코루틴 제어

### 시작 신호 - Job.start()
`launch(start = CoroutineStart.LAZY)` : 코루틴이 즉시 실행되지 않도록 제어

`Job.start()` : 코루틴이 시작되도록 하는 신호.
```kotlin
fun main(): Unit = runBlocking {
	val job = launch(start = CoroutineStart.LAZY) {
		printWithThread("Hello launch")
	}

	delay(1_000L)
	job.start()
}
```
### 취소 신호 - Job.cancel()
`Job.cancel()` : 코루틴을 취소.
```kotlin
fun main(): Unit = runBlocking {
	1. val job = launch {
	2. 	(1..5).forEach {
	3.		printWithThread(it)
	4.		delay(500) 
	  	}
	  }

	5. delay(1_000L)
	6. job.cancel()
}
// 1
// 2
```
실행 순서
1 → 5 → 2 → 3(1출력) → 4 → 5 → 2 → 3(2출력) → 4 → 6
### 대기 신호 - Job.join()
```kotlin
fun main(): Unit = runBlocking {
	val job1 = launch {
		delay(1_000)
		printWithThread("Job 1")
	}

	val job2 = launch {
		delay(1_000)
		printWithThread("Job 2")
	}
}
```
- Job 1과 Job 2가 출력되는데 1.1초 소요된다.
    - job1에서 1초 기다리는 동안 job2가 시작되어 함께 1초를 기다리기 때문.
```kotlin
fun main(): Unit = runBlocking {
	val job1 = launch {
		delay(1_000)
		printWithThread("Job 1")
	}
	job1.join() // job1 코루틴이 끝날때 까지 대기	

	val job2 = launch {
		delay(1_000)
		printWithThread("Job 2")
	}
}
```
- Job 1과 Job 2가 출력되는데 2초 이상 소요된다.
    - 첫 번째 코루틴이 끝날때까지 완전히 기다렸기 때문.
#papercut 

`RestTemplate`: 서버 프로그램에서 API를 호출하기 위해 사용되는 `RestClient 모듈` 중 하나. [[Blocking]] 방식으로 동작한다.

`RestTemplate`은 `Multi-Thread`와 [[Blocking]] 방식을 사용한다.

![[스크린샷 2023-10-19 오전 9.41.04.png]]

요청자 어플리케이션 구동시에 [[Thread pool]]이 생성된다. Request는 먼저 큐에 쌓이고 유휴한 스레드에 할당되어 처리된다. 즉, 요청 하나당 한개의 스레드가 할당된다.
각 스레드에서는 [[Blocking]]방식으로 처리되어 응답이 올때까지 해당 스레드는 다른 요청에 할당될 수 없다.

유휴한 스레드가 없으면 요청은 큐에 대기하게 된다. 네트워크 병목이나 DB 통신 문제가 여러 스레드에서 발생하면 가용 가능한 스레드수가 줄어들고 전체 서비스가 매우 느려지는 문제가 발생한다. ([[Non-Blocking]]인 [[WebClient]]가 대안)

```kotlin
@SpringBootApplication  
class RestclientsApplication {  
    @Bean  
    fun init(): ApplicationRunner {  
        return ApplicationRunner {  
            val rt = RestTemplate()  
            val res: Map<String, Map<String, Double>>? =  
                rt.getForObject("https://open.er-api.com/v6/latest", Map::class.java) as Map<String, Map<String, Double>>?  
            val rate = res?.get("rates").let { it?.get("KRW") }  
  
            println(rate) // 1353.243973
        }  
    }  
}  
  
fun main(args: Array<String>) {  
    runApplication<RestclientsApplication>(*args)  
}
```

RestTemplate이 Deprecated 되었다는 얘기가 있지만 실제로는 그렇지 않다. Spring MVC에서 동기 프로그래밍을 작업한다면 [[WebClient]] 보다 `RestTemplate`을 사용하는게 성능면에서도 조금 더 좋다.
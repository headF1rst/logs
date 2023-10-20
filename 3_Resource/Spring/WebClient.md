#papercut 

`WebClient`: 서버 프로그램에서 API를 호출하기 위해 사용되는 `RestClient 모듈` 중 하나. [[Non-Blocking]] 방식으로 동작한다.

또다른 HttpClient 모듈이면서 [[Blocking]]방식으로 동작하는 [[RestTemplate]]과 달리, `WebClient`는 `Single Thread`와 [[Non-Blocking]]방식을 사용한다. Core 당 1개의 스레드를 사용한다.

`WebClient`는 이벤트에 반응형으로 동작하도록 설계되었으며 비동기성을 보장하는 Spring React 프레임워크를 사용한다. `WebClient`를 사용하려면 `Spring WebFlux`의존성이 필요하다.

![[스크린샷 2023-10-20 오후 4.18.32.png]]
- 각 요청은 `Event Loop`내에 Job으로 등록된다.
- `Event Loop`는 Job을 Workers에게 요청한 후, 결과를 기다리지 않고([[Non-Blocking]]) 다른 Job을 처리한다.
- `Event Loop`는 Workers로부터 **callback** 응답이 오면, 그 결과를 요청자에게 제공한다.

[[Non-Blocking]]인 WebClient를 사용함으로서 요청자와 제공자 사이의 네트워크 병목을 줄이고 성능을 향상시킬 수 있다.

```kotlin
@SpringBootApplication  
class RestclientsApplication {  
    @Bean  
    fun init(): ApplicationRunner {  
        return ApplicationRunner {  
            val wc: WebClient = WebClient.create("https://open.er-api.com")  
            val wcRes: Map<String, Map<String, Double>>? = wc.get().uri("/v6/latest").retrieve().bodyToMono(Map::class.java).block()  
                    as Map<String, Map<String, Double>>?  
            val wcRate = wcRes?.get("rates").let { it?.get("KRW") }  
            println(wcRate)  
        }  
    }  
}  
  
fun main(args: Array<String>) {  
    runApplication<RestclientsApplication>(*args)  
}
```
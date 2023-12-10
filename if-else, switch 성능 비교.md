
최근에 회사 팀원분의 추천으로 '크리에이티브 프로그래머'라는 책을 읽어보았는데, 비판적 사고에 대한 중요성을 강조하고 있다. 

비판적 사고란 정보를 받아들일 때 단순히 수용하지 않고, 의심하고 분석하는 과정을 말한다.
보통 새로운 기술을 강의나 책을통해서 배울때면, A기술은 이러한 특징이있고 B기술은 이러한 특징이 있기 때문에 어떤 상황에서는 A를, 어떤 상황에서는 B를 사용하는게 적합하다라고 설명하는 경우가 많은데, 유명한 책에서 그렇다니까 그냥 그렇구나~ 하고 넘어가고는 했다.

자바에서 분기문은 if-else문과 switch문 두가지로 작성할 수 있다. 개인적으로 if-else 보다는 switch문이 직관적이라고 생각해서 switch문을 선호하지만, 성능을 비교했을땐 어떨까? 

많은 블로그글에서 switch문이 if-else를 썻을때보다 성능에 이점이 있다고 말하지만 과연 정말 그럴지 비판적 사고를 갖고 알아보도록 하자.


둘의 성능을 측정하기위해 JMH를 사용하였으며 다음과 같은 시나리오에서 테스트를 진행해보았다.

- if-else와 switch문의 성능차이 (3개 분기)
- if-else와 switch문의 성능차이 (10개 분기)
- 자바 11, 17, 21에 따른 성능 차이 (버전업에 따른 switch문의 성능 개선 증명)
### 3개 분기에 대한 성능차이

```java
@State(Scope.Benchmark)  
@BenchmarkMode(Mode.AverageTime)  
@OutputTimeUnit(TimeUnit.MICROSECONDS)  
public class IfElseVSSwitchTest {  
  
    private Integer num;  
  
    @Setup  
    public void generateRandomNumber() {  
        Random random = new Random();  
        this.num = random.nextInt(2) + 1;  
    }  
  
    @Benchmark  
    public int ifElseStatement() {  
        int temp = 0;  
  
        for (int i=0; i<10000; ++i) {  
            generateRandomNumber();  
  
            if (num == 1) {  
                temp = 1;  
            } else if (num == 2) {  
                temp = 2;  
            } else if (num == 3) {  
                temp = 3;  
            }  
            temp = 4;  
        }  
        return temp;  
    }  
  
    @Benchmark  
    public int switchStatement() {  
        int temp = 0;  
  
        for (int i=0; i<10000; ++i) {  
            generateRandomNumber();  
  
            switch (num) {  
                case 1:  
                    temp = 1;  
                    break;  
                case 2:  
                    temp = 2;  
                    break;  
                case 3:  
                    temp = 3;  
                    break;  
                default:  
                    temp = 4;  
                    break;  
            }  
        }  
        return temp;  
    }  
}
```


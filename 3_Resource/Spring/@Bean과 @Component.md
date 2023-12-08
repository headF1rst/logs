#papercut 

`@Bean`은 메서드 레벨에서만 설정 가능하다.

`@Bean`이 설정된 팩토리 메서드를 포함하는 구성 클래스에 `@Configuration`이 붙으며 해당 클래스내의 모든 Bean들은 ComponentScan의 대상이되어 스프링 컨테이너에 빈으로 등록된다. (싱글톤 객체로 등록된다.)

주로 외부 라이브러리를 Bean으로 등록하여 싱글톤 객체로 효율적으로 관리하고 싶을때 `@Bean`이 사용된다.

`@Component`는 클래스 레벨에서 선언되며 ComponentScan의 대상이되어 스프링 컨테이너에 빈으로 등록된다.

`@Controller`, `@Service`, `@Repository`등에 메타 어노테이션으로 등록되어있다.
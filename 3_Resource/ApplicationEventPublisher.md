
`ApplicationEventPublisher`: 단일 서비스에서 강결합 문제, 트랜잭션 문제, 비동기 동작이 필요한 경우 활용.

회원 가입시 회원 가입 쿠폰 발행, 회원 가입 이메일 전송이 이루어져야 하는 요구사항이 있다. 이때 이메일 전송 실패, 쿠폰 발급이 실패하는 경우 회원 가입을 다시 해야한다.
### 시스템 강결합 문제
```kotlin
@Service
class MemberSignUpService(
	private val memberRepository: MemberRepository,
	private val couponIssueService: CouponIssueService,
	private val emailSenderService: EmailSenderService,
) {

	@Transactional
	fun signUp(dto: MemberSignUpRequest) {
		val member = createMember(dto.toEntity()) // member 엔티티 영속화
		emailSenderService.sendSignUpEmail(member) // 외부 시스템 회원 가입 이메일 전송
		couponIssueService.issueSignUpCoupon(member.id!!) // 회원가입 쿠폰 발급
	}
}
```

`signUp` 메서드는 엔티티 영속화, 회원 가입 이메일 전송, 쿠폰 발급이라는 많은 책임을 부여받고 있다.
그 결과, `MemberSignUpService`에 많은 의존성이 필요해지고, 의존성 사이에 강한 결합이 이루어진다.
### 트랜잭션 문제

쿠폰 발급 시점에 예외가 발생한다면 쿠폰 발급과 회원가입 로직은 동일 트랜잭션으로 묶여있기 때문에 롤백이 진행된다. 하지만, 외부 시스템을 통한 이메일 전송의 경우, 롤백과 무관하게 이메일을 전송하게 된다.
결국 회원 가입에 실패했지만 이메일을 전송하게 되는 문제가 발생한다.
### 해결
![[스크린샷 2023-10-10 오후 9.49.16.png]]

`EventListner`는 이벤트 생성 주체가 발행한 이벤트(`회원 가입 완료`)에 반응하고, 생성 주체가 발행한 이벤트를 전달받아, 이벤트에 담긴 데이터를 기반으로 기능을 수행한다.
### EventListener 등록

`@EventListener`: 이벤트 발행 시점에 바로 리스닝을 진행.
	- 쿠폰 발행에서 예외가 발생해도 롤백되지 않고 이메일 전송 진행.
`@TransactionalEventListener`: 트랜잭션 커밋 이후에 리스닝을 진행.
	- 쿠폰 발행에서 예외 발생하면 커밋되지 않았기 때문에 이메일 전송을 진행하지 않는다.

```kotlin
@Service
class MemberSignUpService(
	private val memberRepository: MemberRepository,
	private val couponIssueService: CouponIssueService,
	private val eventPublisher: ApplicationEventPublisher, // 의존성 추가
) {

	@Transactional
	fun signUp(dto: MemberSignUpRequest) {
		val member = createMember(dto.toEntity()) // member 엔티티 영속화
		eventPublisher.publishEvent(MemberSignedUpEvent(member)) // 회원가입 완료
		couponIssueService.issuSignUpCoupon(member.id!!) // 쿠폰 발급
	}
}

data class MemberSignedUpEvent(
	val member: Member
)

@Component
class MemberEventHandler(
	private val emailService: EmailSenderService
) {
	// 회원가입 완료 이벤트 리스너 Bean 등록
	@TransactionalEventListener
	fun memberSignedUpEventListener(event: MemberSignedUpEvent) {
		emailSenderService.sendSignUpEmail(event.member)
	}
}
```

[[주문 등록 및 전송 시나리오]]
[ApplicationEventPublisher 기반으로 강결합 및 트랜잭션 문제 해결](https://cheese10yun.github.io/event-transaction/)
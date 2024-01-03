#papercut 

JPA를 사용할 때 쿼리 메서드를 통해 where 조건을 추가할 수 있다.
ex) findById, findByIdAndName, findBy....

검색 조합이 다양해 질수록 만들어야하는 쿼리 메서드의 수가 많아지고 메서드의 이름도 길어지는 문제가 있다.

`Specification`을 이용하여 조건을 상황에 맞게 **조합**하는 **조회 쿼리**를 작성할 수 있다.
### findAll 메서드

JpaRepository를 상속받는 인터페이스를 정의하고 `JpaSpecificationExecutor`도 상속받는다.

```java
List<T> findAll(@Nullable Specification<T> spec);
```

`findAll()` 메서드에 조회 조건을 조합한 `Specification`을 인자로 전달하여 원하는 쿼리를 작성할 수 있다.
### 예제

```kotlin
interface BreakTimeHistoryRepository: JpaRepository<BreakTimeHistory, UUID>, JpaSpecificationExecutor<BreakTimeHistory> {
}
```

```kotlin
data class BreakTimeHistoryInput(
	val loginId: String?, 
	val name: String?,
	val tcCenter: String?,
	val from: LocalDate?,
	val to: LocalDate?,
): PageableInput()
```

```kotlin
@Service
class BreakTimeHistoryService(
	private val breakTimeHistoryRepository: BreakTimeHistoryRepository,
) {
	fun findAll(search: BreakTimeHistoryInput): Page<BreakTimeHistory> {
		return breakTimeHistoryRepository.findAll(
			BreakTimeHistorySpecification.findAll(search), PageRequest.of(search.pageNum - 1, search.pageSize)
		)
	}
}
```

```kotlin
class BreakTimeHistorySpecification {
	companion object {
		fun findAll(search: BreakTimeHistoryInput) =
			Specification
				.where(if (Objects.nonNull(search.from)) whereBreakTimeFrom(search.from!!) else null)
				.and(if (Objects.nonNull(search.to)) whereBreakTimeTo(search.to!!) else null)
				.and(orderByStartBreakTime())

		private fun whereBreakTimeFrom(breakTimeFrom: LocalDate) = Specification<BreakTimeHistory> { root, _, criteriaBuilder -> 
			criteriaBuilder.greaterThanOrEqualTo(root.get("workDay"), breakTimeFrom)
		}

		private fun whereBreakTimeTo(breakTimeFrom: LocalDate) = Specification<BreakTimeHistory> { root, _, criteriaBuilder ->
			criteriaBuilder.lssThanOrEqualTo(root.get("workDay"), breakTimeFrom)
		}

		private fun orderByStartBreakTime() = Specification<BreakTimeHistory> { root, query, criteriaBuilder ->
			query.orderBy(criteriaBuilder.desc(root.get<LocalDate>("workDay")), criteriaBuilder.desc(root.get<LocalTime>("startTime")))
			null
		}
	}
}
```

[[GraphQL]]이랑 조합이 좋을듯하다..

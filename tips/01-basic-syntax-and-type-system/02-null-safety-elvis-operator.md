# Null 안전성: Elvis 연산자 `?:`

## 개요
Elvis 연산자 `?:`는 코틀린의 null 처리 도구 중 하나로, null 값에 대한 기본값을 제공하는 간결한 방법입니다. 이 연산자의 이름은 옆으로 돌렸을 때 엘비스 프레슬리의 헤어스타일과 닮았다고 해서 붙여졌습니다.

## Java에서는?

Java에서 null 값에 대한 기본값을 제공하려면 삼항 연산자나 `Objects.requireNonNullElse` (Java 9+)를 사용해야 합니다:

```java
// Java - 삼항 연산자 사용
String displayName = username != null ? username : "Guest";

// Java 9+ - Objects 클래스 사용
String displayName = Objects.requireNonNullElse(username, "Guest");
```

## Kotlin에서는?
코틀린의 Elvis 연산자 `?:`를 사용하면 위의 코드를 더 간결하게 작성할 수 있습니다:

```kotlin
// Kotlin
val displayName = username ?: "Guest"
```

Elvis 연산자는 "왼쪽 표현식이 null이 아니면 왼쪽 표현식을 사용하고, null이면 오른쪽 표현식을 사용한다"는 의미입니다.

## 실제 사용 예시

### 기본 사용법
```kotlin
val name: String? = "Kotlin"
val nonNullName: String = name ?: "Unknown"  // name이 null이 아니므로 "Kotlin"

val nullableName: String? = null
val defaultName: String = nullableName ?: "Guest"  // nullableName이 null이므로 "Guest"
```

### 안전 호출 연산자와 함께 사용
```kotlin
data class User(val name: String, val email: String?)

val user: User? = getUser()
val email: String = user?.email ?: "No email provided"
```

### 예외 발생시키기
Elvis 연산자의 오른쪽에는 `throw` 표현식도 사용할 수 있습니다:

```kotlin
val name: String = person?.name ?: throw IllegalArgumentException("Person name is required")
```

### 함수 조기 반환
함수 내에서 조기 반환을 위해 `return`과 함께 사용할 수도 있습니다:

```kotlin
fun getShippingAddress(customer: Customer?): Address {
    customer ?: return getDefaultAddress()
    
    // customer가 null이 아닌 경우에만 실행됨
    return customer.shippingAddress ?: customer.address
}
```

## 주의사항
- Elvis 연산자의 오른쪽 표현식은 왼쪽 표현식이 null일 때만 평가됩니다(지연 평가).
- 중첩된 Elvis 연산자를 사용할 수 있지만, 가독성을 위해 적절히 괄호를 사용하는 것이 좋습니다.
- Elvis 연산자의 결과 타입은 왼쪽과 오른쪽 표현식의 공통 상위 타입입니다.

## 팁
Elvis 연산자는 다음과 같은 상황에서 특히 유용합니다:
- 기본값 제공
- 예외 처리
- 조기 반환 패턴
- 체이닝된 null 검사 간소화

## 결론
Elvis 연산자 `?:`는 코틀린에서 null 값을 우아하게 처리하는 강력한 도구입니다. 이를 통해 코드를 더 간결하고 표현력 있게 만들 수 있으며, 특히 안전 호출 연산자 `?.`와 함께 사용할 때 그 효과가 극대화됩니다. Java 개발자들이 코틀린으로 전환할 때 빠르게 익숙해지고 애용하게 될 기능입니다.

# Null 안전성: 단언 연산자 `!!`

## 개요
코틀린의 단언 연산자 `!!`는 nullable 타입을 non-null 타입으로 강제 변환하는 연산자입니다. 이 연산자는 "이 값은 절대 null이 아니다"라고 컴파일러에게 알려주는 역할을 합니다. 하지만 실제로 값이 null이면 `NullPointerException`이 발생하므로 주의해서 사용해야 합니다.

## Java에서는?
Java에서는 명시적인 null 체크 없이 객체를 사용하면 자동으로 `NullPointerException`이 발생합니다:

```java
// Java
public String getUpperCase(String text) {
    // text가 null이면 NullPointerException 발생
    return text.toUpperCase();
}
```

## Kotlin에서는?
코틀린에서는 nullable 타입을 명시적으로 non-null로 변환해야 합니다. 단언 연산자 `!!`를 사용하면 이를 수행할 수 있습니다:

```kotlin
// Kotlin
fun getUpperCase(text: String?): String {
    // text가 null이면 NullPointerException 발생
    return text!!.uppercase()
}
```

단언 연산자는 "이 값이 null이 아님을 보장한다. null이라면 예외를 발생시켜라"라는 의미입니다.

## 실제 사용 예시

### 기본 사용법
```kotlin
val name: String? = "Kotlin"
val nonNullName: String = name!! // name이 null이 아니므로 안전
val length: Int = nonNullName.length // 일반 메서드 호출 가능
```

```kotlin
val nullableName: String? = null
val willCrash: String = nullableName!! // NullPointerException 발생!
```

### 메서드 체이닝에서의 사용
```kotlin
data class User(val name: String, val address: Address?)
data class Address(val city: String)

fun getCity(user: User?): String {
    // user와 address가 모두 null이 아니라고 확신할 때
    return user!!.address!!.city
}
```

## 주의사항
- 단언 연산자는 코틀린의 null 안전성을 우회하는 방법이므로 **최후의 수단**으로만 사용해야 합니다.
- 가능하면 안전 호출 연산자 `?.`와 Elvis 연산자 `?:`를 사용하는 것이 좋습니다.
- 단언 연산자를 사용할 때는 해당 값이 null이 아님을 확실히 알고 있어야 합니다.
- 단언 연산자가 발생시키는 `NullPointerException`은 Java의 NPE와 동일하게 런타임 오류입니다.

## 적절한 사용 시나리오
단언 연산자는 다음과 같은 상황에서 제한적으로 사용하는 것이 좋습니다:

1. **초기화 지연**: 초기화가 보장되지만 컴파일러가 이를 감지할 수 없는 경우
   ```kotlin
   private var _binding: FragmentBinding? = null
   
   override fun onCreateView(...): View {
       _binding = FragmentBinding.inflate(...)
       return _binding!!.root // onCreateView 이후에는 항상 초기화됨
   }
   ```

2. **테스트 코드**: 테스트에서 null이 아님이 확실한 경우
   ```kotlin
   @Test
   fun `test user creation`() {
       val user = createUser()
       assertNotNull(user)
       assertEquals("John", user!!.name) // assertNotNull 이후에는 안전
   }
   ```

3. **외부 API와의 계약**: API 문서나 계약에 의해 null이 반환되지 않음이 보장된 경우

## 더 나은 대안
대부분의 경우, 단언 연산자 대신 다음과 같은 대안을 고려하세요:

```kotlin
// 안전 호출과 Elvis 연산자 사용
val city = user?.address?.city ?: "Unknown"

// requireNotNull 함수 사용 (더 명확한 오류 메시지)
val nonNullUser = requireNotNull(user) { "User must not be null" }

// 조건부 실행
user?.let { 
    // user가 null이 아닐 때만 실행
    println(it.name)
}
```

## 결론
단언 연산자 `!!`는 코틀린의 null 안전성 시스템에서 "안전망을 제거"하는 연산자입니다. 이는 Java 스타일의 null 처리로 돌아가는 것과 같으므로, 정말 필요한 경우에만 제한적으로 사용해야 합니다. 코틀린의 철학은 null 안전성을 통해 `NullPointerException`을 방지하는 것이므로, 가능한 한 안전 호출 연산자 `?.`와 Elvis 연산자 `?:`를 활용하는 것이 좋습니다.

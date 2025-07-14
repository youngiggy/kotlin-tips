# Val vs Var: 불변성과 가변성의 명시적 구분

## 개요
코틀린에서는 변수 선언 시 `val`과 `var` 키워드를 사용하여 불변(immutable)과 가변(mutable) 변수를 명시적으로 구분합니다. 이는 코드의 가독성을 높이고 의도를 명확히 하며, 불변성을 통한 안전한 코드 작성을 장려합니다.

## Java에서는?
Java에서는 변수의 불변성을 나타내기 위해 `final` 키워드를 사용합니다:

```java
// Java
final String name = "John"; // 불변 변수
String address = "Seoul";   // 가변 변수
```

하지만 `final` 키워드는 선택적이며, 사용하지 않는 경우가 많아 어떤 변수가 변경될 수 있는지 코드만 보고 즉시 파악하기 어렵습니다.

## Kotlin에서는?
코틀린에서는 모든 변수 선언 시 `val` 또는 `var` 키워드를 반드시 사용해야 합니다:

```kotlin
// Kotlin
val name: String = "John"    // 불변 변수 (final과 유사)
var address: String = "Seoul" // 가변 변수
```

- `val` (value): 한 번 초기화되면 값을 변경할 수 없는 불변 변수 (Java의 `final`과 유사)
- `var` (variable): 값을 변경할 수 있는 가변 변수

## 실제 사용 예시

### 기본 사용법
```kotlin
val pi = 3.14159  // 불변 변수
// pi = 3.14  // 컴파일 오류: Val cannot be reassigned

var counter = 0   // 가변 변수
counter = 1       // 정상 작동
```

### 타입 추론
```kotlin
val name = "John"  // String 타입으로 추론됨
var age = 30       // Int 타입으로 추론됨
```

### 지연 초기화
```kotlin
val user: User     // 선언만 하고 초기화는 나중에
if (isAdmin) {
    user = getAdminUser()
} else {
    user = getRegularUser()
}
// 이후에는 user 값을 변경할 수 없음
```

### 참조 불변성 vs 객체 불변성
```kotlin
val list = mutableListOf(1, 2, 3)  // 참조는 불변이지만
list.add(4)                        // 객체 내용은 변경 가능

// 객체 내용도 불변으로 만들려면
val immutableList = listOf(1, 2, 3)  // 불변 리스트
// immutableList.add(4)  // 컴파일 오류: 불변 리스트는 수정 불가
```

## 주의사항
- `val`로 선언된 변수의 참조는 변경할 수 없지만, 참조하는 객체의 내부 상태는 변경될 수 있습니다.
- 클래스의 프로퍼티도 `val`과 `var`로 선언할 수 있습니다.
- `val`은 불변성을 보장하지만, 지연 초기화(lazy initialization)는 가능합니다.

## 권장 사항
- 기본적으로 모든 변수를 `val`로 선언하고, 필요한 경우에만 `var`로 변경하세요.
- 이는 함수형 프로그래밍의 원칙을 따르며, 코드의 안정성을 높입니다.
- 특히 다중 스레드 환경에서 불변 객체는 동시성 문제를 줄이는 데 도움이 됩니다.

## 실제 활용 예
```kotlin
class User(
    val id: Int,           // 불변 프로퍼티
    val name: String,      // 불변 프로퍼티
    var email: String      // 가변 프로퍼티
) {
    val formattedId = "USER-$id"  // 파생 프로퍼티 (불변)
    var lastLoginDate: Date? = null  // 가변 프로퍼티
}

fun processUser(user: User) {
    // user.id = 2  // 컴파일 오류: Val cannot be reassigned
    user.email = "new.email@example.com"  // 정상 작동
}
```

## 결론
코틀린의 `val`과 `var` 키워드는 변수의 의도를 명확히 하고 불변성을 장려하는 언어 설계의 좋은 예입니다. 이를 통해 코드의 가독성이 향상되고, 불변성을 기본으로 하는 안전한 코드 작성이 가능해집니다. Java에서 `final` 키워드를 일관되게 사용하지 않는 개발자들에게 코틀린의 이러한 강제적 구분은 더 안전하고 예측 가능한 코드를 작성하도록 유도합니다.

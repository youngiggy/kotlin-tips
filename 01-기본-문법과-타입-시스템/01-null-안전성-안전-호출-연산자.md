# Null 안전성: 안전 호출 연산자 `?.`

## 개요
코틀린의 가장 강력한 기능 중 하나는 컴파일 타임에 `NullPointerException`을 방지하는 타입 시스템입니다. 안전 호출 연산자 `?.`는 이 시스템의 핵심 요소로, null 값에 대한 메서드 호출을 안전하게 처리합니다.

## Java에서는?
Java에서 null 체크는 다음과 같이 명시적으로 수행해야 합니다:

```java
// Java
public String getUpperCaseCity(Person person) {
    if (person != null) {
        Address address = person.getAddress();
        if (address != null) {
            City city = address.getCity();
            if (city != null) {
                return city.getName().toUpperCase();
            }
        }
    }
    return null;
}
```

이러한 중첩된 null 체크는 코드를 장황하게 만들고 가독성을 떨어뜨립니다.

## Kotlin에서는?
코틀린의 안전 호출 연산자 `?.`를 사용하면 위의 코드를 다음과 같이 간결하게 작성할 수 있습니다:

```kotlin
// Kotlin
fun getUpperCaseCity(person: Person?): String? {
    return person?.address?.city?.name?.uppercase()
}
```

안전 호출 연산자 `?.`는 "왼쪽 표현식이 null이 아니면 메서드나 프로퍼티에 접근하고, null이면 전체 표현식의 결과로 null을 반환한다"는 의미입니다.

## 실제 사용 예시

### 기본 사용법
```kotlin
val name: String? = "Kotlin"
val length: Int? = name?.length  // name이 null이 아니므로 length는 6
```

```kotlin
val name: String? = null
val length: Int? = name?.length  // name이 null이므로 length도 null
```

### 메서드 체이닝
```kotlin
data class Address(val street: String?, val city: String?, val zipCode: String?)
data class User(val name: String, val address: Address?)

val user: User? = getUser()
val city: String? = user?.address?.city  // 안전하게 중첩 프로퍼티 접근
```

### 컬렉션과 함께 사용
```kotlin
val users: List<User>? = getUsers()
val firstUserCity = users?.firstOrNull()?.address?.city
```

## 주의사항
- 안전 호출 연산자를 사용하면 결과 타입은 항상 nullable이 됩니다.
- 연속된 안전 호출은 체인의 어느 부분이든 null이면 전체 표현식이 null이 됩니다.
- 안전 호출 후에 non-null 값이 필요하다면 Elvis 연산자 `?:`와 함께 사용하세요.

## 팁
안전 호출 연산자는 다음과 같은 상황에서 특히 유용합니다:
- 외부 API나 데이터베이스에서 가져온 데이터 처리
- 사용자 입력 처리
- 네트워크 응답 처리

## 결론
안전 호출 연산자 `?.`는 코틀린의 null 안전성 기능 중 가장 자주 사용되는 요소입니다. 이를 통해 코드를 더 간결하게 작성하면서도 런타임에 `NullPointerException`이 발생할 위험을 크게 줄일 수 있습니다. Java 개발자가 코틀린으로 전환할 때 가장 먼저 익숙해져야 할 기능 중 하나입니다.

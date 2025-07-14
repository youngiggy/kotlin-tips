# 기본 함수 선언: 간결함과 명확함의 조화

## 개요
코틀린의 함수 선언은 Java보다 더 간결하고 표현력이 풍부합니다. 함수 선언 시 반환 타입, 매개변수, 가시성 제어자 등을 더 명확하게 표현할 수 있으며, 다양한 축약형을 통해 코드를 간결하게 작성할 수 있습니다.

## Java에서는?
Java에서 함수(메서드)는 다음과 같이 선언합니다:

```java
// Java
public int add(int a, int b) {
    return a + b;
}

// 반환값이 없는 경우
public void greet(String name) {
    System.out.println("Hello, " + name + "!");
}
```

## Kotlin에서는?
코틀린에서 함수는 `fun` 키워드로 시작하며, 매개변수 이름 뒤에 타입을 명시합니다. 반환 타입은 매개변수 목록 뒤에 콜론(`:`)으로 구분하여 표시합니다:

```kotlin
// Kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}

// 반환값이 없는 경우 (Unit은 생략 가능)
fun greet(name: String): Unit {
    println("Hello, $name!")
}
```

## 실제 사용 예시

### 기본 함수 선언
```kotlin
fun calculateArea(width: Double, height: Double): Double {
    return width * height
}
```

### 단일 표현식 함수
함수 본문이 단일 표현식인 경우, 중괄호와 return 문을 생략하고 등호(`=`)로 표현할 수 있습니다:

```kotlin
fun calculateArea(width: Double, height: Double): Double = width * height
```

### 반환 타입 추론
단일 표현식 함수의 경우, 반환 타입을 생략하면 컴파일러가 자동으로 추론합니다:

```kotlin
fun calculateArea(width: Double, height: Double) = width * height
```

### 기본 매개변수 값
함수 매개변수에 기본값을 지정할 수 있어, 호출 시 해당 인자를 생략할 수 있습니다:

```kotlin
fun greet(name: String, greeting: String = "Hello"): String {
    return "$greeting, $name!"
}

// 호출 방법
val message1 = greet("John")  // "Hello, John!"
val message2 = greet("John", "Hi")  // "Hi, John!"
```

### 명명된 인자
함수 호출 시 매개변수 이름을 명시하여 가독성을 높이고 순서에 상관없이 인자를 전달할 수 있습니다:

```kotlin
fun createUser(name: String, email: String, age: Int = 0) {
    // 사용자 생성 로직
}

// 호출 방법
createUser(name = "John", email = "john@example.com")
createUser(email = "john@example.com", name = "John", age = 30)
```

### 가변 인자 (vararg)
함수가 가변 개수의 인자를 받을 수 있도록 `vararg` 키워드를 사용할 수 있습니다:

```kotlin
fun sum(vararg numbers: Int): Int {
    return numbers.sum()
}

// 호출 방법
val total = sum(1, 2, 3, 4, 5)  // 15
```

### 스프레드 연산자 (*)
배열을 가변 인자로 전달할 때 스프레드 연산자(`*`)를 사용합니다:

```kotlin
val numbers = intArrayOf(1, 2, 3, 4, 5)
val total = sum(*numbers)  // 15
```

### 지역 함수
함수 내부에 다른 함수를 정의할 수 있습니다:

```kotlin
fun processData(data: List<String>): List<String> {
    // 지역 함수 정의
    fun validate(item: String): Boolean {
        return item.isNotEmpty() && item.length <= 100
    }
    
    // 지역 함수 사용
    return data.filter { validate(it) }
}
```

### 확장 함수
기존 클래스에 새로운 함수를 추가할 수 있습니다:

```kotlin
fun String.addExclamation(): String {
    return "$this!"
}

// 사용 방법
val excited = "Hello".addExclamation()  // "Hello!"
```

## 주의사항
- 코틀린에서는 함수가 최상위 레벨에 선언될 수 있으며, 클래스 내부에 있을 필요가 없습니다.
- 기본 매개변수 값을 사용할 때는 오버로딩된 함수와의 충돌에 주의해야 합니다.
- Java에서 코틀린 함수를 호출할 때는 명명된 인자를 사용할 수 없습니다.

## 결론
코틀린의 함수 선언은 Java보다 더 간결하고 표현력이 풍부합니다. 기본 매개변수 값, 명명된 인자, 단일 표현식 함수 등의 기능을 통해 코드의 가독성과 유지보수성을 높일 수 있습니다. 특히 단일 표현식 함수와 기본 매개변수 값은 보일러플레이트 코드를 크게 줄여주어 더 깔끔한 코드 작성을 가능하게 합니다.

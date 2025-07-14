# 문자열 템플릿: 문자열 내 변수와 표현식 사용하기

## 개요
코틀린의 문자열 템플릿(String Templates)은 문자열 내에 변수나 표현식을 직접 삽입할 수 있는 기능입니다. 이를 통해 문자열 연결 작업을 더 간결하고 가독성 있게 작성할 수 있습니다.

## Java에서는?
Java에서 문자열에 변수를 포함시키려면 문자열 연결 연산자(`+`)나 `String.format()` 메서드를 사용해야 합니다:

```java
// Java - 문자열 연결 연산자 사용
String name = "John";
int age = 30;
String message = "My name is " + name + " and I am " + age + " years old.";

// Java - String.format() 사용
String formattedMessage = String.format("My name is %s and I am %d years old.", name, age);
```

## Kotlin에서는?
코틀린에서는 문자열 템플릿을 사용하여 변수나 표현식을 직접 문자열에 삽입할 수 있습니다:

```kotlin
// Kotlin
val name = "John"
val age = 30
val message = "My name is $name and I am $age years old."
```

달러 기호(`$`)를 사용하여 변수를 참조하거나, `${}`를 사용하여 표현식을 삽입할 수 있습니다.

## 실제 사용 예시

### 기본 변수 참조
```kotlin
val name = "Kotlin"
val greeting = "Hello, $name!"  // "Hello, Kotlin!"
```

### 표현식 삽입
```kotlin
val a = 10
val b = 20
val result = "The sum of $a and $b is ${a + b}"  // "The sum of 10 and 20 is 30"
```

### 객체 프로퍼티 접근
```kotlin
data class User(val name: String, val age: Int)

val user = User("Alice", 25)
val info = "User: ${user.name}, Age: ${user.age}"  // "User: Alice, Age: 25"
```

### 조건식 사용
```kotlin
val score = 85
val result = "Result: ${if (score >= 70) "Pass" else "Fail"}"  // "Result: Pass"
```

### 컬렉션 처리
```kotlin
val fruits = listOf("Apple", "Banana", "Cherry")
val fruitInfo = "First fruit: ${fruits.first()}, Last fruit: ${fruits.last()}"
// "First fruit: Apple, Last fruit: Cherry"
```

### 여러 줄 문자열에서 사용
```kotlin
val name = "Kotlin"
val multiline = """
    Hello, $name!
    Welcome to string templates.
    You can use ${'$'}name to refer to variables.
"""
```

## 이스케이프 처리
달러 기호(`$`)를 문자 그대로 사용하려면 백슬래시(`\`)로 이스케이프하거나, 위 예제처럼 `${'$'}`를 사용할 수 있습니다:

```kotlin
val price = "Price: \$25.99"  // "Price: $25.99"
val literal = "To use a variable, write ${'$'}variableName"  // "To use a variable, write $variableName"
```

## 성능 고려사항
문자열 템플릿은 컴파일 시점에 최적화되어 `StringBuilder`를 사용한 문자열 연결과 동일한 바이트코드를 생성합니다. 따라서 성능 걱정 없이 사용할 수 있습니다.

## 주의사항
- 복잡한 표현식은 가독성을 위해 별도의 변수로 분리하는 것이 좋습니다.
- 중첩된 표현식은 코드 가독성을 떨어뜨릴 수 있으므로 적절히 사용해야 합니다.

## 결론
코틀린의 문자열 템플릿은 Java의 문자열 연결이나 `String.format()`보다 더 간결하고 가독성이 좋은 방법을 제공합니다. 특히 여러 변수나 표현식을 포함하는 복잡한 문자열을 구성할 때 코드의 가독성과 유지보수성을 크게 향상시킵니다. 이는 코틀린이 제공하는 간결한 구문의 좋은 예시 중 하나입니다.

# 람다 표현식 기초: 간결한 함수형 프로그래밍

## 개요
람다 표현식은 코틀린에서 함수형 프로그래밍을 구현하는 핵심 요소입니다. 이름 없는 함수(익명 함수)를 간결하게 표현할 수 있어 코드의 가독성을 높이고 보일러플레이트 코드를 줄여줍니다. 특히 컬렉션 조작, 비동기 처리, 이벤트 핸들링 등에서 유용하게 사용됩니다.

## Java에서는?
Java 8부터 람다 표현식이 도입되었지만, 코틀린의 람다 표현식보다 문법이 다소 복잡합니다:

```java
// Java
// 익명 클래스 사용 (Java 8 이전)
button.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) {
        System.out.println("Button clicked");
    }
});

// 람다 표현식 사용 (Java 8 이후)
button.setOnClickListener(v -> System.out.println("Button clicked"));

// 함수형 인터페이스 사용
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(name -> System.out.println(name));
```

## Kotlin에서는?
코틀린의 람다 표현식은 더 간결하고 유연합니다:

```kotlin
// Kotlin
// 기본 람다 표현식
button.setOnClickListener { v -> println("Button clicked") }

// 매개변수가 하나인 경우 'it' 키워드 사용 가능
button.setOnClickListener { println("Button clicked") }

// 컬렉션 처리
val names = listOf("Alice", "Bob", "Charlie")
names.filter { it.startsWith("A") }
     .forEach { println(it) }
```

## 실제 사용 예시

### 기본 람다 구문
```kotlin
// 기본 형태: { 매개변수 -> 본문 }
val sum = { x: Int, y: Int -> x + y }
println(sum(3, 5))  // 8

// 타입 추론을 활용한 간결한 표현
val numbers = listOf(1, 2, 3, 4, 5)
val doubled = numbers.map { it * 2 }  // [2, 4, 6, 8, 10]
```

### 단일 매개변수와 'it' 키워드
단일 매개변수를 가진 람다에서는 매개변수를 명시적으로 선언하지 않고 `it` 키워드를 사용할 수 있습니다:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val evenNumbers = numbers.filter { it % 2 == 0 }  // [2, 4]
```

### 여러 줄의 람다
람다 본문이 여러 줄인 경우, 마지막 표현식이 반환값이 됩니다:

```kotlin
val calculateGrade = { score: Int ->
    println("Calculating grade for score: $score")
    when {
        score >= 90 -> "A"
        score >= 80 -> "B"
        score >= 70 -> "C"
        score >= 60 -> "D"
        else -> "F"
    }
}

val grade = calculateGrade(85)  // "B"
```

### 함수 타입
람다는 함수 타입의 변수에 할당할 수 있습니다:

```kotlin
// (Int, Int) -> Int는 두 개의 Int 매개변수를 받아 Int를 반환하는 함수 타입
val operation: (Int, Int) -> Int = { x, y -> x + y }

// 함수 타입을 매개변수로 받는 고차 함수
fun calculate(x: Int, y: Int, op: (Int, Int) -> Int): Int {
    return op(x, y)
}

val result = calculate(10, 5, { a, b -> a * b })  // 50
```

### 후행 람다 문법
함수의 마지막 매개변수가 함수 타입인 경우, 람다를 괄호 밖으로 빼낼 수 있습니다:

```kotlin
// 일반적인 호출
calculate(10, 5, { a, b -> a * b })

// 후행 람다 문법
calculate(10, 5) { a, b -> a * b }

// 함수의 유일한 인자가 람다인 경우 괄호 생략 가능
numbers.filter { it > 0 }
```

### 익명 함수
람다 표현식 대신 익명 함수를 사용할 수도 있습니다:

```kotlin
val sum = fun(x: Int, y: Int): Int {
    return x + y
}

// 표현식 형태의 익명 함수
val multiply = fun(x: Int, y: Int) = x * y
```

### 클로저(Closure)
람다는 외부 변수를 캡처하여 사용할 수 있습니다:

```kotlin
fun createCounter(): () -> Int {
    var count = 0
    return { count++ }  // 외부 변수 count를 캡처
}

val counter = createCounter()
println(counter())  // 0
println(counter())  // 1
println(counter())  // 2
```

## 주의사항
- 람다에서 외부 변수를 수정할 때는 해당 변수가 `var`로 선언되어 있어야 합니다.
- 람다가 객체를 캡처하면 메모리 누수가 발생할 수 있으므로 주의해야 합니다.
- 복잡한 로직은 가독성을 위해 명명된 함수로 분리하는 것이 좋습니다.

## 결론
코틀린의 람다 표현식은 함수형 프로그래밍을 간결하고 표현력 있게 구현할 수 있게 해줍니다. 특히 컬렉션 처리, 비동기 작업, 이벤트 핸들링 등에서 코드의 가독성과 유지보수성을 크게 향상시킵니다. Java에서 코틀린으로 전환할 때 람다 표현식의 간결함과 유연성은 큰 장점으로 작용합니다.

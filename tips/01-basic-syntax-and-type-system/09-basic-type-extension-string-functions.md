# 기본 타입 확장: 문자열 함수

## 개요
코틀린은 String 타입에 다양한 유용한 확장 함수를 제공합니다. 이러한 확장 함수들은 문자열 처리를 더 간결하고 표현력 있게 만들어 줍니다. Java에서는 별도의 유틸리티 클래스나 복잡한 메서드 체이닝을 사용해야 하는 작업들을 코틀린에서는 String 타입에 직접 호출할 수 있습니다.

## Java에서는?
Java에서는 문자열 작업을 위해 String 클래스의 기본 메서드나 별도의 유틸리티 클래스를 사용해야 합니다:

```java
// Java
String text = "  Hello World  ";
String trimmed = text.trim();
boolean isEmpty = text.isEmpty();
boolean contains = text.contains("Hello");
String[] parts = text.split(" ");
String repeated = String.join("", Collections.nCopies(3, "abc"));
```

## Kotlin에서는?
코틀린에서는 String 타입에 확장 함수가 추가되어 있어 더 자연스럽게 사용할 수 있습니다:

```kotlin
// Kotlin
val text = "  Hello World  "
val trimmed = text.trim()
val isEmpty = text.isEmpty()
val contains = text.contains("Hello")
val parts = text.split(" ")
val repeated = "abc".repeat(3)
```

## 실제 사용 예시

### 문자열 검증 함수
```kotlin
val text = "Hello123"
val empty = ""
val blank = "   "

// 비어있는지 확인
println(empty.isEmpty())  // true
println(text.isEmpty())   // false

// 공백 문자만 있는지 확인
println(blank.isBlank())  // true
println(text.isBlank())   // false

// 비어있지 않은지 확인
println(text.isNotEmpty())  // true
println(text.isNotBlank())  // true

// 숫자로만 구성되어 있는지 확인
println("123".isDigitsOnly())  // true
println(text.isDigitsOnly())   // false
```

### 문자열 변환 함수
```kotlin
val text = "hello world"

// 첫 글자만 대문자로
val capitalized = text.replaceFirstChar { it.uppercase() }  // "Hello world"

// 각 단어의 첫 글자를 대문자로
val titleCase = text.split(" ").joinToString(" ") { 
    it.replaceFirstChar { char -> char.uppercase() } 
}  // "Hello World"

// 대소문자 변환
val upperCase = text.uppercase()  // "HELLO WORLD"
val lowerCase = text.lowercase()  // "hello world"
```

### 문자열 분할과 결합
```kotlin
val csv = "apple,banana,orange"
val items = csv.split(",")  // ["apple", "banana", "orange"]

// 구분자로 결합
val joined = items.joinToString(" | ")  // "apple | banana | orange"

// 접두사, 접미사와 함께 결합
val formatted = items.joinToString(
    separator = ", ",
    prefix = "[",
    postfix = "]"
)  // "[apple, banana, orange]"

// 변환과 함께 결합
val upperJoined = items.joinToString { it.uppercase() }  // "APPLE, BANANA, ORANGE"
```

### 문자열 패딩과 정렬
```kotlin
val text = "Hello"

// 패딩 추가
val padded = text.padStart(10, '*')  // "*****Hello"
val rightPadded = text.padEnd(10, '-')  // "Hello-----"

// 중앙 정렬 (확장 함수로 구현)
fun String.center(width: Int, padChar: Char = ' '): String {
    if (this.length >= width) return this
    val padding = width - this.length
    val leftPadding = padding / 2
    val rightPadding = padding - leftPadding
    return padChar.toString().repeat(leftPadding) + this + padChar.toString().repeat(rightPadding)
}

val centered = text.center(10, '*')  // "**Hello***"
```

### 문자열 필터링과 변환
```kotlin
val text = "Hello123World456"

// 문자만 필터링
val lettersOnly = text.filter { it.isLetter() }  // "HelloWorld"

// 숫자만 필터링
val numbersOnly = text.filter { it.isDigit() }  // "123456"

// 조건에 맞는 문자 제거
val noNumbers = text.filterNot { it.isDigit() }  // "HelloWorld"

// 문자 변환
val transformed = text.map { 
    if (it.isDigit()) '*' else it 
}.joinToString("")  // "Hello***World***"
```

### 문자열 시작/끝 검사
```kotlin
val filename = "document.pdf"
val url = "https://example.com"

// 시작 문자열 검사
val isHttps = url.startsWith("https")  // true
val isSecure = url.startsWith("https://")  // true

// 끝 문자열 검사
val isPdf = filename.endsWith(".pdf")  // true
val isImage = filename.endsWith(".jpg", ".png", ".gif")  // false (여러 확장자 동시 검사)
```

### 문자열 반복과 생성
```kotlin
val separator = "=".repeat(50)  // "=================================================="
val spaces = " ".repeat(10)  // "          "

// 조건부 반복
val stars = "*".takeIf { true }?.repeat(5) ?: ""  // "*****"
```

### 문자열 부분 추출
```kotlin
val text = "Hello, World!"

// 안전한 부분 문자열 추출
val safe = text.take(5)  // "Hello"
val fromEnd = text.takeLast(6)  // "World!"
val dropped = text.drop(7)  // "World!"
val droppedLast = text.dropLast(1)  // "Hello, World"

// 조건부 추출
val letters = text.takeWhile { it.isLetter() }  // "Hello"
val afterComma = text.dropWhile { it != ',' }.drop(2)  // "World!"
```

### 문자열 치환
```kotlin
val text = "Hello World Hello"

// 첫 번째 매칭만 치환
val replacedFirst = text.replaceFirst("Hello", "Hi")  // "Hi World Hello"

// 모든 매칭 치환
val replacedAll = text.replace("Hello", "Hi")  // "Hi World Hi"

// 정규식 사용
val noNumbers = "abc123def456".replace(Regex("\\d+"), "")  // "abcdef"
```

## 주의사항
- 일부 확장 함수는 내부적으로 정규식이나 복잡한 로직을 사용하므로 성능에 주의해야 합니다.
- `isDigitsOnly()` 같은 함수는 빈 문자열에 대해 `false`를 반환합니다.
- 문자열 변환 함수들은 대부분 새로운 String 객체를 생성하므로 메모리 사용량을 고려해야 합니다.

## 결론
코틀린의 String 확장 함수는 문자열 처리를 더 간결하고 표현력 있게 만들어 줍니다. Java에서는 별도의 유틸리티 클래스나 복잡한 메서드 체이닝을 사용해야 했던 작업들을 코틀린에서는 String 타입에 직접 호출할 수 있어 코드의 가독성과 유지보수성이 향상됩니다. 특히 문자열 검증, 변환, 분할, 필터링 등의 작업을 더 자연스럽게 표현할 수 있습니다. 
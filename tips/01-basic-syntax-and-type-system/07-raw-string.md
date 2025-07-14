# Raw 문자열: 이스케이프 문자 없이 여러 줄 문자열 작성하기

## 개요
코틀린의 Raw 문자열(Raw String)은 이스케이프 문자 없이 여러 줄의 텍스트를 그대로 표현할 수 있는 기능입니다. 따옴표 세 개(`"""`)로 둘러싸인 문자열로, 특수 문자나 줄바꿈을 그대로 포함할 수 있어 정규식, JSON, XML, SQL 쿼리 등을 작성할 때 매우 유용합니다.

## Java에서는?
Java에서 여러 줄 문자열을 표현하려면 각 줄 끝에 `+` 연산자를 사용하거나 `StringBuilder`를 사용해야 합니다:

```java
// Java - 문자열 연결 사용
String query = "SELECT id, name, email " +
               "FROM users " +
               "WHERE active = true " +
               "ORDER BY name";

// Java - 이스케이프 문자 사용
String json = "{\n" +
              "  \"name\": \"John\",\n" +
              "  \"age\": 30,\n" +
              "  \"city\": \"New York\"\n" +
              "}";
```

Java 15부터는 텍스트 블록(Text Block) 기능이 추가되어 코틀린의 Raw 문자열과 유사한 기능을 제공합니다:

```java
// Java 15+ - 텍스트 블록
String json = """
              {
                "name": "John",
                "age": 30,
                "city": "New York"
              }
              """;
```

## Kotlin에서는?
코틀린에서는 Raw 문자열을 사용하여 여러 줄 텍스트를 쉽게 표현할 수 있습니다:

```kotlin
// Kotlin - Raw 문자열
val query = """
    SELECT id, name, email
    FROM users
    WHERE active = true
    ORDER BY name
"""

val json = """
    {
      "name": "John",
      "age": 30,
      "city": "New York"
    }
"""
```

## 실제 사용 예시

### 기본 사용법
```kotlin
val address = """
    123 Main St
    Apt 4B
    New York, NY 10001
"""
```

### 들여쓰기 제거 (trimIndent)
```kotlin
val code = """
    fun hello() {
        println("Hello, World!")
    }
""".trimIndent()
```

### 특정 마진 제거 (trimMargin)
```kotlin
val text = """
    |This is line 1
    |This is line 2
    |This is line 3
""".trimMargin()

// 다른 접두사 사용
val text2 = """
    #First line
    #Second line
    #Third line
""".trimMargin("#")
```

### 정규식 패턴
```kotlin
val regex = """
    \d+      # 하나 이상의 숫자
    \s+      # 하나 이상의 공백
    \w+      # 하나 이상의 단어 문자
""".trimIndent().toRegex(RegexOption.COMMENTS)
```

### HTML/XML 작성
```kotlin
val html = """
    <!DOCTYPE html>
    <html>
    <head>
        <title>Raw Strings Example</title>
    </head>
    <body>
        <h1>Hello, Kotlin!</h1>
        <p>This is a raw string example.</p>
    </body>
    </html>
""".trimIndent()
```

### SQL 쿼리
```kotlin
val query = """
    SELECT u.id, u.name, u.email, 
           a.street, a.city, a.country
    FROM users u
    JOIN addresses a ON u.id = a.user_id
    WHERE u.active = true
      AND a.country = 'USA'
    ORDER BY u.name
""".trimIndent()
```

### 문자열 템플릿과 함께 사용
```kotlin
val name = "Kotlin"
val version = 1.5

val info = """
    Language: $name
    Version: $version
    Released: 2021
""".trimIndent()
```

## 주의사항
- Raw 문자열 내에서도 문자열 템플릿(`$` 또는 `${}`)을 사용할 수 있습니다.
- 달러 기호(`$`)를 문자 그대로 사용하려면 `${'$'}`와 같이 이스케이프해야 합니다.
- `trimIndent()`와 `trimMargin()`은 문자열의 공통 들여쓰기를 제거하지만, 원본 문자열을 수정하지 않고 새 문자열을 반환합니다.

## 결론
코틀린의 Raw 문자열은 여러 줄 텍스트를 작성할 때 가독성과 유지보수성을 크게 향상시킵니다. 특히 정규식, SQL 쿼리, JSON, XML 등 특수 문자가 많이 포함된 텍스트를 작성할 때 이스케이프 문자의 사용을 줄여 코드를 더 깔끔하게 만들어 줍니다. Java 개발자가 코틀린으로 전환할 때 가장 환영할 만한 기능 중 하나입니다.

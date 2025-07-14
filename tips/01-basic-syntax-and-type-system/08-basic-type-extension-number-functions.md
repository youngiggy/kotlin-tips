# 기본 타입 확장: 숫자 함수

## 개요
코틀린은 기본 타입(Int, Double, Float 등)에 다양한 유용한 확장 함수를 제공합니다. 이러한 확장 함수들은 숫자 처리를 더 간결하고 표현력 있게 만들어 줍니다. Java에서는 별도의 유틸리티 클래스나 메서드를 사용해야 하는 작업들을 코틀린에서는 기본 타입에 직접 호출할 수 있습니다.

## Java에서는?
Java에서는 기본 타입에 메서드를 추가할 수 없으므로, 유틸리티 클래스나 래퍼 클래스의 정적 메서드를 사용해야 합니다:

```java
// Java
int min = Math.min(5, 10);
int max = Math.max(5, 10);
double abs = Math.abs(-5.5);
int rounded = Math.round(5.7f);
```

## Kotlin에서는?
코틀린에서는 기본 타입에 확장 함수가 추가되어 있어 더 자연스럽게 사용할 수 있습니다:

```kotlin
// Kotlin
val min = 5.coerceAtMost(10)
val max = 5.coerceAtLeast(10)
val abs = (-5.5).absoluteValue
val rounded = 5.7f.roundToInt()
```

## 실제 사용 예시

### 범위 제한 함수
```kotlin
// 값을 특정 범위 내로 제한
val limited = 15.coerceIn(0, 10)  // 10
val atLeast = (-5).coerceAtLeast(0)  // 0
val atMost = 100.coerceAtMost(50)  // 50
```

### 반올림 함수
```kotlin
val pi = 3.14159
val roundedDown = pi.roundToInt()  // 3
val roundedUp = 3.99.roundToInt()  // 4
val ceilingValue = pi.ceil()  // 4.0
val floorValue = pi.floor()  // 3.0
val truncated = pi.truncate()  // 3.0
```

### 부호 관련 함수
```kotlin
val absolute = (-10).absoluteValue  // 10
val sign = (-10).sign  // -1
val isNegative = (-10).isNegative()  // true
val isPositive = 10.isPositive()  // true
```

### 진법 변환
```kotlin
val decimal = 255
val hex = decimal.toString(16)  // "ff"
val binary = decimal.toString(2)  // "11111111"
val octal = decimal.toString(8)  // "377"
```

### 비트 연산
```kotlin
val a = 0b1010  // 10 (이진수)
val b = 0b1100  // 12 (이진수)

val and = a and b  // 8 (0b1000)
val or = a or b  // 14 (0b1110)
val xor = a xor b  // 6 (0b0110)
val shift = a shl 1  // 20 (0b10100) - 왼쪽으로 1비트 시프트
val rightShift = b shr 2  // 3 (0b0011) - 오른쪽으로 2비트 시프트
```

### 숫자 변환
```kotlin
val int = 42
val long = int.toLong()  // 42L
val double = int.toDouble()  // 42.0
val float = int.toFloat()  // 42.0f
val short = int.toShort()  // 42 (Short 타입)
val byte = int.toByte()  // 42 (Byte 타입)
```

### 수학 함수
```kotlin
import kotlin.math.*

val squareRoot = 16.0.sqrt()  // 4.0
val power = 2.0.pow(3.0)  // 8.0
val log = 100.0.log10()  // 2.0
val sin = PI.sin()  // 0.0
val cos = PI.cos()  // -1.0
```

### 범위 생성
```kotlin
val range = 1..10  // 1부터 10까지의 범위
val chars = 'a'..'z'  // 'a'부터 'z'까지의 범위

// 역순 범위
val reverseRange = 10 downTo 1  // 10부터 1까지 역순 범위

// 특정 간격의 범위
val stepRange = 1..10 step 2  // 1, 3, 5, 7, 9
```

## 주의사항
- 일부 확장 함수는 `kotlin.math` 패키지를 임포트해야 사용할 수 있습니다.
- 부동소수점 연산은 정밀도 문제가 있을 수 있으므로 주의해야 합니다.
- 비트 연산은 정수 타입에만 사용할 수 있습니다.

## 결론
코틀린의 기본 타입 확장 함수는 숫자 처리를 더 간결하고 표현력 있게 만들어 줍니다. Java에서는 별도의 유틸리티 클래스나 정적 메서드를 사용해야 했던 작업들을 코틀린에서는 기본 타입에 직접 호출할 수 있어 코드의 가독성과 유지보수성이 향상됩니다. 특히 범위 제한, 반올림, 비트 연산 등의 작업을 더 자연스럽게 표현할 수 있습니다.

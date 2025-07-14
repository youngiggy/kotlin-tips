# 컬렉션 생성과 기본 연산

## 개요
코틀린은 다양한 컬렉션 타입과 이를 생성하고 조작하는 풍부한 API를 제공합니다. 불변(immutable) 컬렉션과 가변(mutable) 컬렉션을 명확히 구분하며, 간결하고 표현력 있는 방식으로 컬렉션을 다룰 수 있습니다. 이 팁에서는 코틀린의 기본 컬렉션 타입과 생성 방법, 그리고 자주 사용되는 기본 연산에 대해 알아봅니다.

## Java에서는?
Java에서 컬렉션을 생성하고 조작하는 방법은 다소 장황합니다:

```java
// Java
// 리스트 생성
List<String> immutableList = Collections.unmodifiableList(
    Arrays.asList("Apple", "Banana", "Cherry")
);

List<String> mutableList = new ArrayList<>();
mutableList.add("Apple");
mutableList.add("Banana");
mutableList.add("Cherry");

// 맵 생성
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 100);
map.put("Banana", 200);
map.put("Cherry", 300);

// 집합 생성
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Cherry");
```

## Kotlin에서는?
코틀린에서는 컬렉션 생성과 조작이 더 간결하고 직관적입니다:

```kotlin
// Kotlin
// 불변 컬렉션 생성
val immutableList = listOf("Apple", "Banana", "Cherry")
val immutableMap = mapOf("Apple" to 100, "Banana" to 200, "Cherry" to 300)
val immutableSet = setOf("Apple", "Banana", "Cherry")

// 가변 컬렉션 생성
val mutableList = mutableListOf("Apple", "Banana", "Cherry")
val mutableMap = mutableMapOf("Apple" to 100, "Banana" to 200)
val mutableSet = mutableSetOf("Apple", "Banana")

// 가변 컬렉션 수정
mutableList.add("Date")
mutableMap["Date"] = 400
mutableSet.add("Date")
```

## 실제 사용 예시

### 리스트(List) 생성과 기본 연산
```kotlin
// 불변 리스트 생성
val fruits = listOf("Apple", "Banana", "Cherry")

// 빈 리스트 생성
val emptyList = emptyList<String>()

// 가변 리스트 생성
val mutableFruits = mutableListOf("Apple", "Banana")
mutableFruits.add("Cherry")  // 요소 추가
mutableFruits.removeAt(0)    // 인덱스로 요소 제거
mutableFruits.remove("Banana")  // 값으로 요소 제거

// 리스트 기본 연산
val first = fruits.first()  // 첫 번째 요소
val last = fruits.last()    // 마지막 요소
val size = fruits.size      // 크기
val isEmpty = fruits.isEmpty()  // 비어있는지 확인
val contains = fruits.contains("Apple")  // 요소 포함 여부
val indexOf = fruits.indexOf("Banana")   // 요소의 인덱스
```

### 맵(Map) 생성과 기본 연산
```kotlin
// 불변 맵 생성
val fruitPrices = mapOf(
    "Apple" to 100,
    "Banana" to 200,
    "Cherry" to 300
)

// 빈 맵 생성
val emptyMap = emptyMap<String, Int>()

// 가변 맵 생성
val mutableFruitPrices = mutableMapOf(
    "Apple" to 100,
    "Banana" to 200
)
mutableFruitPrices["Cherry"] = 300  // 요소 추가/수정
mutableFruitPrices.remove("Apple")  // 요소 제거

// 맵 기본 연산
val keys = fruitPrices.keys       // 모든 키
val values = fruitPrices.values   // 모든 값
val entries = fruitPrices.entries // 모든 엔트리
val applePrice = fruitPrices["Apple"]  // 키로 값 접근
val hasKey = fruitPrices.containsKey("Banana")  // 키 포함 여부
val hasValue = fruitPrices.containsValue(300)   // 값 포함 여부
```

### 집합(Set) 생성과 기본 연산
```kotlin
// 불변 집합 생성
val uniqueFruits = setOf("Apple", "Banana", "Cherry")

// 빈 집합 생성
val emptySet = emptySet<String>()

// 가변 집합 생성
val mutableUniqueFruits = mutableSetOf("Apple", "Banana")
mutableUniqueFruits.add("Cherry")  // 요소 추가
mutableUniqueFruits.remove("Apple")  // 요소 제거

// 집합 기본 연산
val hasBanana = uniqueFruits.contains("Banana")  // 요소 포함 여부
val intersection = uniqueFruits.intersect(setOf("Banana", "Date"))  // 교집합
val union = uniqueFruits.union(setOf("Date", "Elderberry"))  // 합집합
val difference = uniqueFruits.subtract(setOf("Apple", "Date"))  // 차집합
```

### 배열(Array) 생성과 기본 연산
```kotlin
// 배열 생성
val stringArray = arrayOf("Apple", "Banana", "Cherry")
val intArray = intArrayOf(1, 2, 3, 4, 5)
val emptyArray = emptyArray<String>()

// 특정 크기의 배열 생성
val zeros = IntArray(5)  // [0, 0, 0, 0, 0]
val fives = IntArray(5) { 5 }  // [5, 5, 5, 5, 5]
val squares = IntArray(5) { i -> i * i }  // [0, 1, 4, 9, 16]

// 배열 기본 연산
val firstElement = stringArray[0]  // 인덱스로 접근
stringArray[1] = "Blueberry"  // 요소 수정
val arraySize = stringArray.size  // 배열 크기
val containsCherry = "Cherry" in stringArray  // 요소 포함 여부
```

### 컬렉션 변환
```kotlin
// 리스트 ↔ 집합 변환
val fruitList = listOf("Apple", "Banana", "Apple", "Cherry")
val fruitSet = fruitList.toSet()  // 중복 제거: [Apple, Banana, Cherry]
val backToList = fruitSet.toList()  // 다시 리스트로: [Apple, Banana, Cherry]

// 맵 변환
val fruitPairs = listOf("Apple" to 100, "Banana" to 200, "Cherry" to 300)
val fruitPriceMap = fruitPairs.toMap()  // 리스트를 맵으로 변환

// 배열 ↔ 리스트 변환
val fruitArray = arrayOf("Apple", "Banana", "Cherry")
val fruitListFromArray = fruitArray.toList()  // 배열을 리스트로 변환
val backToArray = fruitListFromArray.toTypedArray()  // 리스트를 배열로 변환
```

## 주의사항
- 불변 컬렉션(`listOf`, `setOf`, `mapOf`)은 내부 요소를 수정할 수 없습니다.
- 가변 컬렉션(`mutableListOf`, `mutableSetOf`, `mutableMapOf`)은 내부 요소를 수정할 수 있습니다.
- 불변 컬렉션도 내부 객체의 상태가 변경 가능하다면 그 객체의 상태는 변경될 수 있습니다.
- 코틀린의 컬렉션은 Java 컬렉션과 완벽하게 호환됩니다.

## 결론
코틀린의 컬렉션 API는 Java보다 더 간결하고 표현력이 풍부합니다. 불변 컬렉션과 가변 컬렉션을 명확히 구분하여 의도를 명확하게 표현할 수 있으며, 다양한 유틸리티 함수를 통해 컬렉션을 쉽게 조작할 수 있습니다. 특히 컬렉션 생성 구문의 간결함은 코드의 가독성을 크게 향상시킵니다.

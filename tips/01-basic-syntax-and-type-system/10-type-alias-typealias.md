# 타입 별칭(typealias)

## 개요
코틀린의 `typealias` 키워드는 기존 타입에 새로운 이름을 부여할 수 있는 기능입니다. 복잡한 타입 시그니처를 간단하게 만들거나, 의미를 명확하게 하고자 할 때 사용합니다. 이는 새로운 타입을 만드는 것이 아니라, 기존 타입에 대한 별칭을 만드는 것으로 런타임에는 동일한 타입으로 취급됩니다.

## Java에서는?
Java에서는 타입 별칭을 직접 지원하지 않습니다. 복잡한 타입을 간단하게 표현하려면 제네릭이나 상속을 사용해야 합니다:

```java
// Java - 복잡한 타입 시그니처
Map<String, List<User>> userGroups = new HashMap<>();
Function<String, List<User>> userFinder = name -> findUsersByName(name);

// 또는 래퍼 클래스 생성
class UserGroups extends HashMap<String, List<User>> {
    // 추가 메서드들...
}
```

## Kotlin에서는?
코틀린에서는 `typealias`를 사용해 타입에 별칭을 부여할 수 있습니다:

```kotlin
// Kotlin - 타입 별칭 사용
typealias UserGroups = Map<String, List<User>>
typealias UserFinder = (String) -> List<User>

val userGroups: UserGroups = mapOf()
val userFinder: UserFinder = { name -> findUsersByName(name) }
```

## 실제 사용 예시

### 복잡한 제네릭 타입 간소화
```kotlin
// 복잡한 제네릭 타입들
typealias StringMap = Map<String, String>
typealias UserList = List<User>
typealias UserMap = Map<String, User>
typealias EventHandler = (Event) -> Unit

// 사용
val config: StringMap = mapOf("host" to "localhost", "port" to "8080")
val users: UserList = listOf(user1, user2, user3)
val userIndex: UserMap = users.associateBy { it.id }
val onClickHandler: EventHandler = { event -> println("Clicked: $event") }
```

### 함수 타입 별칭
```kotlin
// 함수 타입에 의미 있는 이름 부여
typealias Validator<T> = (T) -> Boolean
typealias Transformer<T, R> = (T) -> R
typealias AsyncCallback<T> = (T?) -> Unit

// 사용 예시
class ValidationEngine {
    private val validators = mutableListOf<Validator<User>>()
    
    fun addValidator(validator: Validator<User>) {
        validators.add(validator)
    }
    
    fun validate(user: User): Boolean {
        return validators.all { it(user) }
    }
}

// 검증 함수들
val emailValidator: Validator<User> = { user -> user.email.contains("@") }
val ageValidator: Validator<User> = { user -> user.age >= 18 }
```

### 컬렉션 타입 별칭
```kotlin
// 자주 사용되는 컬렉션 타입들
typealias StringList = List<String>
typealias IntSet = Set<Int>
typealias StringToIntMap = Map<String, Int>

// 중첩된 컬렉션 타입
typealias Matrix = List<List<Int>>
typealias Graph = Map<String, List<String>>
typealias CategoryItems = Map<String, List<Product>>

// 사용
val categories: CategoryItems = mapOf(
    "electronics" to listOf(laptop, phone),
    "books" to listOf(novel, textbook)
)

val adjacencyList: Graph = mapOf(
    "A" to listOf("B", "C"),
    "B" to listOf("D"),
    "C" to listOf("D")
)
```

### 도메인 특화 타입 별칭
```kotlin
// 도메인 의미를 명확하게 하는 타입 별칭
typealias UserId = String
typealias Email = String
typealias PhoneNumber = String
typealias Timestamp = Long

// 화폐 관련
typealias Amount = BigDecimal
typealias Currency = String
typealias Price = Pair<Amount, Currency>

// 좌표계
typealias Latitude = Double
typealias Longitude = Double
typealias Coordinate = Pair<Latitude, Longitude>

// 사용
data class User(
    val id: UserId,
    val email: Email,
    val phone: PhoneNumber,
    val createdAt: Timestamp
)

data class Product(
    val name: String,
    val price: Price,
    val location: Coordinate
)
```

### 콜백과 리스너 타입 별칭
```kotlin
// 이벤트 처리 관련 타입 별칭
typealias ClickListener = (View) -> Unit
typealias TextChangeListener = (String) -> Unit
typealias LoadingListener = (Boolean) -> Unit
typealias ErrorListener = (Throwable) -> Unit

// 비동기 처리 관련
typealias SuccessCallback<T> = (T) -> Unit
typealias ErrorCallback = (Exception) -> Unit
typealias CompletionCallback = () -> Unit

// 사용 예시
class ApiService {
    fun fetchUser(
        id: UserId,
        onSuccess: SuccessCallback<User>,
        onError: ErrorCallback
    ) {
        // API 호출 로직
        try {
            val user = callApi(id)
            onSuccess(user)
        } catch (e: Exception) {
            onError(e)
        }
    }
}
```

### 제네릭 타입 별칭
```kotlin
// 제네릭 타입 별칭
typealias Result<T> = Either<String, T>  // Either는 가정된 타입
typealias Optional<T> = T?
typealias Factory<T> = () -> T
typealias Predicate<T> = (T) -> Boolean

// 사용
fun <T> createOptional(value: T): Optional<T> = value
fun <T> createFactory(provider: Factory<T>): Factory<T> = provider

val stringFactory: Factory<String> = { "Hello World" }
val numberPredicate: Predicate<Int> = { it > 0 }
```

### 플랫폼 타입과 호환성
```kotlin
// 플랫폼별 타입 별칭 (Android 예시)
typealias AndroidContext = android.content.Context
typealias AndroidView = android.view.View
typealias AndroidIntent = android.content.Intent

// 크로스 플랫폼 개발에서 유용
typealias PlatformContext = AndroidContext  // Android에서
// typealias PlatformContext = NSObject     // iOS에서 (Kotlin Multiplatform)
```

### 함수 합성과 파이프라인
```kotlin
// 함수 합성을 위한 타입 별칭
typealias Pipeline<T> = (T) -> T
typealias Middleware<T> = (T, (T) -> T) -> T

// 사용
val stringPipeline: Pipeline<String> = { input ->
    input.trim()
        .lowercase()
        .replace(" ", "_")
}

val loggingMiddleware: Middleware<String> = { input, next ->
    println("Processing: $input")
    val result = next(input)
    println("Result: $result")
    result
}
```

## 주의사항
- `typealias`는 새로운 타입을 만들지 않습니다. 컴파일 시점에서는 별칭이고 런타임에는 원래 타입입니다.
- 타입 안전성을 위해서는 `inline class`나 `value class`를 고려해보세요.
- 너무 많은 타입 별칭은 코드를 복잡하게 만들 수 있으므로 적절히 사용해야 합니다.
- 제네릭 타입 별칭을 사용할 때는 타입 파라미터의 의미를 명확히 해야 합니다.

## 결론
`typealias`는 복잡한 타입 시그니처를 간단하게 만들고, 코드의 가독성을 향상시키는 유용한 기능입니다. 특히 함수 타입, 제네릭 타입, 컬렉션 타입 등이 복잡할 때 의미 있는 이름을 부여하여 코드를 더 이해하기 쉽게 만들 수 있습니다. 단, 새로운 타입을 만드는 것이 아니라 기존 타입의 별칭이라는 점을 명심하고 사용해야 합니다. 
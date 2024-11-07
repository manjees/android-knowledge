# 1. remember vs rememberSaveable
- 둘 다 Jetpack Compose에서 상태를 저장하는 데 사용되지만, 저장 범위와 재생성 시점에 차이가 있음

|remember|rememberSaveable|
| - | - |
| - recomposition 동안 상태 유지: remember는 recomposition이 발생하더라고 상태를 유자함<br/>- 화면 회전, 프로세스 종료 후 상태 소멸: 화면 회전과 같은 configuration change이나 앱이 메모리에서 제거되는 상황이 발생하면 상태가 사라짐| - 구성 변경에 따라 상태 유지: rememberSaveable은 rememberdhk 달리 구성 변경이나 프로세스 종료와 같은 상황에서도 상태를 유지함<br/>- Bundle로 저장 가능한 값만 가능: 기본적으로 Bundle로 저장할 수 있는 타입만 지원함<br/>- 커스텀 객체나 복잡한 데이터 구조를 사용할 때는 Saver를 정의해야함|

- saver 예시
```kotlin
data class Person(val name: String, val age: Int)

// Saver 정의
val personSaver = Saver<Person, Map<String, Any>>(
    save = { person ->
        // 저장할 형태로 변환 (Map 형태로 저장)
        mapOf("name" to person.name, "age" to person.age)
    },
    restore = { map ->
        // 저장된 형태로부터 Person 객체를 복원
        val name = map["name"] as? String
        val age = map["age"] as? Int
        if (name != null && age != null) {
            Person(name, age)
        } else {
            null
        }
    }
)

@Composable
fun PersonScreen() {
    // rememberSaveable에 Saver를 사용하여 Person 객체 저장
    var person by rememberSaveable(saver = personSaver) {
        mutableStateOf(Person("Alice", 25))
    }
}
```

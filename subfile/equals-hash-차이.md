> equals()와 hashCode()는 **객체가 동일한지 또는 같은 값을 가지고 있는지를 판단**하기 위해 사용되지만, 역할과 목적이 약간 다름

||equals()|hashCode()|
| - | - | - |
| 목적 | - 두 객체의 **내용이 같은지**(값이 같은지)를 비교 | - 객체의 **고유한 해시 코드를 생성**하여 컬렌션(Set, Map등)에서 효율적으로 객체를 관리할 수 있도록 도움|
| 사용 방식 | - 두 인스턴스 필드 값을 비교하여 내용이 동일한지를 판별| - 객체의 내용을 기반으로 해시코드를 반환<br/>- 같은 값을 가진 두 객체는 동일한 해시코드를 반환해야 하지만, 다른 값을 가진 객체는 다른 해시 코드를 반환할 가능성이 높음|
| 예시 | - 각 속성의 값이 같은 경우 true를 반환| - 두 객체가 같은 값을 가지면 동일한 해시 코드를 반환하므로, HashMap이나 HashSet과 같은 컬렌션에서 객체의 위치를 빠르게 찾을 수 있음|

# 왜 두 메소드가 함께 필요한가?
- **equals**()는 두 객체가 실제로 같은 내용을 가지고 있는지 확인하고,
- **hashCode**()는 객체를 효율적으로 검색하고 컬렉션에서 관리하기 위해 사용됩니다

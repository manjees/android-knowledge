# LaunchMode
- Activity가 시작될 떄의 인스턴스 생성 및 관리 방식을 결정하는 속성
- AndroidManifest.xml 에서 Activity에 지정할 수 있음

|Mode|Description|
| - | - |
| Standard | - 각 요청마다 새로운 Activity 인스턴스를 생성<br>- Activity가 호출될 때마다 새로 생성되기 때문에, 동일한 Activity가 여러 개 쌓일 수 있음<br>- 특별한 설정이 없을 경우 기본적으로 이 모드가 적용|
| SingleTop | - 현재 Task의 최상단에 동일한 Activity가 존재할 경우 새로 생성하지 않고 기존 Activity 인스턴스를 재사용<br>- 만약 동일한 Activity가 최상단에 없다면, 새로운 인스턴스를 생성하여 추가<br>- 동일한 화면을 여러번 호출할 가능성이 있는 경우 적합<br>- 최상단에 동일한 Activity가 있을 경우에만 재사용함 -> 해당 Activity를 보고 있는 경우에 호출 시 onNewIntent()를 호출하지 않음|
| singleTask | - Task 내에 오직 하나의 인스턴스만 존재하도록 보장<br>- 동일한 Activity 호출시 기존 인스턴스를 재사용하여, 해당 Task의 최상단에 위치시킴<br>- 메인 화면이나 고유한 화면에 사용 -> 새로운 Task를 생성하는 성격을 가짐|
| singleInstance | - 해당 Activity가 전용 Task를 단독으로 사용하도록 함<br>- 해당 모드의 Activity는 다른 Activity와 Task를 공유하지 않으면, 오직 하나의 인스턴스만 존재<br>- 다이얼로그 처럼 독립적으로 표시되어야 하는 곳에 사용|

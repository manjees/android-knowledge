# Component
- 시스템 및 사용자가 애플리케이션과 상호 작용할 수 있는 진입점
- 각 구성 요소는 고유한 기능과 생명 주기를 가지고 있음

## 1. Activity
- 사용자와 상호 작용하기 위해 UI 관련 자원을 제공하는 독립적이고 재사용 가능한 컴포넌트
- 모든 안드로이드 애플리케이션은 사용자와 상호 작용하고 앱에 진입하기 위해 적어도 하나의 액티비티를 가져야함
- 모든 액티비티는 자체적인 생명주기를 가지고 있음 ([액티비티라이프사이클](https://github.com/manjees/android-knowledge/blob/main/subfile/activity-lifecycle.md))
  
|Activity|Description|
| - | - |
| AppCompatActivity | - 안드로이드 앱의 하위 호환성을 제공<br/>- 최신 디자인 요소를 이전 안드로이드 버전에서도 사용할 수 있게 함<br/>- <strong>거의 대부분 사용</strong>|
| FragmentActivity | - <strong>**프래그먼트**</strong>를 다루기 위해 설계된 액티비티<br/>- 앱에서 동적 UI 구성을 지원하고 여러 화면을 나누어 사용할 수 있게 해주며 프래그먼트의 생명주기를 관리함|
| ComponentActivity | - Jetpack 컴포넌트들과의 연동을 용이하게 해주는 기본 클래스<br/>- ViewModel, LiveData, Lifecycle와 같은 Jetpack 라이브러리를 쉽게 통합할 수 있도록 설계<br/>- 안드로이드 컴포넌트의 생명주기 및 상태 관리 기능을 제공하여 더 높은 수준의 모듈화를 지원<br/>- AppCompat 라이브러리에 종속되지 않는 AppCompatActivity보다 가벼운 클래스이여 <strong>Jetpack 라이브러리의 생명주기 관리 기능과 긴밀하게 통합</strong>|
| PreferenceActivity | - 앱의 설정 화면을 쉽게 구현할 수 있도록 설계된 특화 액티비티<br/>- Preference API와 연동하여 설정 옵션을 관리하고 UI를 자동으로 구성해줌<br/>- 최근에는 AppCompatActivity와 PreferanceFragmentCompat을 조합새 사용함|

## 2. Service
- UI 없이 백그라운드에서 장기적인 작업을 수행하는 컴포넌트
- <strong>앱이 화면에 표시되지 않더라도 특정 작업을 계속 진행할 수 있도록 지원</strong> -> 예) 음악 재생, 위치 정보를 주기적으로 업데이트
#### 서비스 특징
- 백그라운드 작업 수행: 서비스는 사용자와 직접 상호 작용하지 않고, 오래걸리는 작업을 백그라운드에서 수행
- 서비스 유형
  - Started Service
    - <strong>독립적으로 작업을 수행</strong>
    - 서비스가 한 번 시작되면, 작업을 수행한 후 직접 종료되거나, 다른 구성 요소에서 명시적으로 stopService()를 호출하여 종료
    - 무기한으로 실행되는 것이 가능
    - 생명 주기: onCreate() -> onStartCommand() -> onDestroy()
  - Bound Service
    - <strong>클라이언트와 지속적인 통신을 위한 서비스</strong>
    - 서비스가 다른 구성 요소에 바인딩되며, 클라이언트와의 상호 작용을 통해 서비스가 실행
    - bindService()로 시작된 서비스를 IBinder 인터페이스를 통해 클라이언트가 서비스와 통신할 수 있도록 함
    - 주로 클라이언트-서버 통신이나 DB 작업 등에 사용되며 클라가 unbindService()를 호출해 연결 해제하면 서비스 종료됨
    - 생명 주기: onCreate() -> onBind() -> onUnbind() -> onDestroy()

## 3. Broadcast Receiver
- 안드로이드 앱에서 시스템 이벤트나 다른 앱의 알림을 수신하기 위해 사용되는 컴포넌트
- 예를 들어, 베터리 상태 변경, 네트워크 연결 상태 변경, 앱 설치 완료 등의 이벤트에 반응하고자 할 때 사용 가능
#### 특징
- 이벤트 기반 수신
  - 이벤트 기반으로 특정 상황에 반응함 (시스템이 부팅될 때 특정 동작을 수행가거나, 충전 상태가 변경되면 광련된 로직을 실행)
- 생명 주기 없음
  - <strong>별도의 생명 주기</strong>가 없으며, 등록(registerReceiver())될 때만 이벤트를 수신 -> 등록된 후에는 이벤트가 발생할 때마다 onReceive() 메소드가 호출됨
  - 이 메소드는 빠르게 작업을 수행하고 종료되어야 하며, 복잡한 직업이 필요한 경우 Service와 연동하여 별도로 처리하는 것이 권장됨
- 두 가지 등록 방식
  - 정적 등록
    - AndroidManifest.xml 파일에 등록하여 시스템 전반에서 이벤트를 항상 수신 (부팅 후 동작 설정 같은 경우)
  - 동적 등록
    - 앱이 실행 중일 때 Context.registerReceiver() 메소드로 코드 내에서 수신 등록 (특정 화면이나 상황에서만 이벤트를 수신하고자 할 때 유용)
   
## 4. Content Provider
- <strong>앱 간 데이터 공유</strong>를 가능하게 하는 컴포넌트
- 앱 내부 데이터베이스나 파일에 접근해야 하는 다른 앱에서 Content Provider를 통해 데이터를 요청하고, 권한이 부여된 경우 안전하게 데이터 주고 받기 가능
#### 특징
- 데이터 공유
  - 앱이 외부에 데이터를 공유할 때 Content Provider가 사용됨
  - 연락처, 캘린더, 갤러리와 같은 시스템 데이터를 다른 앱에서 접근할 때 활요
- 표준화된 인터페이스 (URI 사용)
  - URI라는 표준화된 인터페이스를 통해 데이터를 제공 (content:://contracts/company)
- CRUD 연상 지원
  - DB처럼 Create, Read, Update, Delete 연산을 지원
  - insert(), query(), update(), delete() 메소드로 수행
  - DB 뿐 아니라 파일이나 다른 데이터 저장소에서도 유사한 방식으로 데이터에 접근 가능
- 권한 관리
 - 권한 설정 시스템을 사용하여 접근 제어 가능

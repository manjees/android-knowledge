# Fragment
- UI의 재사용 가능한 구성 요소로, 액티비티 위에서 UI 요소를 제공하며 사용자와 상호작용함
- 일반적으로 Fragment는 액티비티에 속하며 Fragment 매니저에 의해 관리됨

# Fragment Lifecycle
- 각각 고유한 생명주기를 가지고 있으며, 이는 뷰와 리소스의 수명을 올바르게 관리하는 데 중요한 개념
- Fragment 클래스는 생명주기 상태가 변경되었거나 액티비티에 연결되었음을 Fragment에 알리는 핵심 콜백을 제공

### onCreate()
- Fragment가 생성되고 **FragmentManager**에 추가된 후 호출됨
- 이 단계에서 **getActivity**()와 같은 액티비티 관련 작업을 수행하면 안 됨

### onCreateView()
- Fragment의 UI를 생성하는 주요 작업이 이곳에서 처리됨
- 적절한 **View**를 반환하면 Fragment의 뷰 생명주기가 시작
- Fragment 생성자에서 레이아웃 리소스 ID를 전달하여 자동으로 뷰를 인플레이트할 수도 있음

### onViewCreated()
- **onCreateView**()에서 반환한 뷰가 완전히 초기화된 후 호출
- UI 초기화와 뷰 작업은 이 단계에서 수행하는 것이 적절

### onStart()
- Fragment가 사용자에게 보이기 시작할 때 호출
- 보통 액티비티의 onStart()와 연결되며, 여러번 호출될 수 있음

### onResume()
- Fragment가 포그라운드에 나타나 사용자와 상호작용할 준비가 되었을 때 호출
- 액티비티의 onResume()과 연결

### onPause()
- 사용자가 Fragment를 떠나기 시작할 때 호출
- 일부 Fragment가 여전히 부분적으로 보일 수 있는 상태에서도 호출될 수 있음

### onStop()
- Fragment가 사용자에게 더 이상 보이지 않을 때 호출

### onDestroyView()
- Fragment에 연결된 뷰가 해제될 때 호출
- 뷰 생명주기가 종료되며, 뷰와 관련된 리소스를 정리해야함

### onDestroy()
- Fragment가 제거되거나 **FragmentManager**가 소멸될 때 호출
- 모든 남은 리소스를 해제하거나 작업을 중단하는 적절한 시점

> 리소스 관리<br/>- 뷰와 관련된 리소스: ViewBinding 또는 DataBinding 객체는 onDestroyView()에서 null로 설정<br/>- LifecycleObserver: onPause() 또는 onStop()에서 카메라, 센서, 위치 서비스와 같은 리소스 집약적인 작업을 해제<br/>- 콜백 해제: Listener나 Observer는 Fragment가 제거될 때 메모리 누수를 방지하기 위해 해제합니다. 예를 들어, LiveData의 Observer는 onDestroy()에서 제거할 수 있음

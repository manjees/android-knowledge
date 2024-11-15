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

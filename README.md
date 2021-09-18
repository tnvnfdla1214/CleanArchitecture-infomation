# Clean Architecture 란

### :wrench: 프로젝트 사용 사례
+ ToDO
***
### :lollipop: Clean Architecture 의 구조 및 특징
<img src = "https://user-images.githubusercontent.com/48902047/133888188-781eea8d-d64f-4fc8-bb1d-6a7128e520a2.JPG" width="80%" height="80%"> 

클린 아키텍처의 구조는 위와 같이 총 4가지 계층으로 되어 있다.
이렇게 계층을 나누는 이유는 계층을 분리하여 관심사를 분리시키기 위해서이며 이런 아키텍처가 동작하기 위해서는 의존성 규칙을 지켜야 한다. 한마디로 각 분리된 클래스가 한가지 역할만 하고 서로 의존을 어떻게 할지 규칙이 정해져있고 지켜야한다는 말이다.
의존성 규칙은 모든 소스코드 의존성은 반드시 외부에서 내부로, 고수준 정책을 향해야 한다. (위 그림에서는 원 안쪽으로 갈수록 의존성이 낮아진다.)  즉 안드로이드를 예로들면 비즈니스 로직을 담당하는 ViewModel과 같은 코드들이 DB 또는 Web 같이 구체적인 세부 사항에 의존하지 않아야 한다. 이를 통해 비즈니스 로직(고수준 정책)은 세부 사항들(저수준 정책)의 변경에 영향을 받지 않도록 할 수 있다.

이렇게 나눔으로써 얻는 이점들은 다음과 같습니다. 가장 중요한건 Testable과 유지보수 및 협업이라 볼 수 있겠습니다.

- 코트 테스트 커버리지 증대
- 쉽게 패키지 구조 탐색 가능
- 집중화된 클래스에 따른 프로젝트 유지 관리 증대
- 새 기능을 빠르게 적용 가능
- 이후의 개발에도 안정적인 구현
- 명확한 규율로 전반적으로 따라야 할 베스트 프랙티스

1. **Entities** : 엔티티는 가장 일반적인 비즈니스 규칙을 캡슐화하고 DTO(Data Transfer Object)도 포함하는 전사적 비즈니스 규칙이다. 외부가 변경되면 이러한 규칙이 변경 될 가능성이 가장 적다.
2. **Use cases** : 유스케이스는 Intereactor라고도 하며 소프트웨어의 애플리케이션 별 비즈니스 규칙을 나타낸다. 이 계층은 데이터베이스, 공통 프레임 워크 및 UI에 대한 변경으로부터 격리된다.
3. **Interface Adapters (Presenters)** : 인터페이스 어댑터는 데이터를 Entity 및 UseCase의 편리한 형식(Format) 에서 데이터베이스 및 웹에 적용 할 수있는 형식으로 변환한다. 이 계층에는 MVP의 Presenter, MVVM의 ViewModel 및 게이트웨이 (= Repositories)가 포함된다. 즉 순수한 비즈니스 로직만을 담당하는 역할을 하게 된다.
4. **Frameworks & Drivers (Web, DB)** : 프레임워크와 드라이버는 웹 프레임 워크, 데이터베이스, UI, HTTP 클라이언트 등으로 구성된 가장 바깥 쪽 계층이다.

헷갈릴 수 있으므로 MVC와 MVP 패턴의 분리 방식으로 한 그림은 다음과 같다.

<img src = "https://user-images.githubusercontent.com/48902047/133888391-12b15b72-0a8c-46b9-b3e5-6546099dfb81.png">
<img src = "https://user-images.githubusercontent.com/48902047/133888444-2276b5a9-841c-4198-86c2-5d976a1f0522.png">
<img src = "https://user-images.githubusercontent.com/48902047/133888449-f39f49b3-9fef-49f6-aefe-9c3ee0c7e9ff.png">
<img src = "https://user-images.githubusercontent.com/48902047/133888450-40ab56ca-32d3-4f25-9db7-84023df5def5.png">
<img src = "https://user-images.githubusercontent.com/48902047/133888454-7390bf3b-ecb1-4dbc-b127-f0e611905bd9.png">

1. **Presentation** : UI(Activity, Fragment), Presenter 및 ViewModel을 포함한다. 즉 화면과 입력에 대한 처리 등 UI와 직접적으로 관련된 부분을 담당합니다. 또한 Presentation 레이어는 Domain과 Data 레이어를 포함하고 있다는 특징이 있다.
2. **Domain** : 애플리케이션의 비즈니스 로직을 포함하고 비즈니스 로직에서 필요한 Model 과 UseCase를 포함하고 있다. 기존 MVVM을 하고 클린아키텍처를 공부한다면 UseCase를 처음보실텐데 한번만 더 짚고 넘어가자면 각 개별 기능 또는 비즈니스 논리 단위라고 보시면 된다. 그래서 UseCase는 보통 한 개의 행동을 담당하고 UseCase의 이름만 보고 이게 무슨 기능을 가졌을지 짐작하고 구분할 수 있어야한다. 추가로 Domain 레이어는 Presentation, Data 레이어와 어떤 의존성도 맺지 않고 독립적이다는 특징이 있다.
3. **Data** : Repositoy 구현체, Cache, Room DB, Dao, Model 서버API(Retrofit2) 을 포함하고 있으며 로컬 또는 서버 API와 통신하여 데이터를 CRUD 하는 역할을 한다. 또한 Mapper 클래스도 포함하고 있는데 DB로 부터 받아온 데이터모델과 UI에 맞는 데이터모델간의 변환을 해주는 역할을 한다. 추가로 Domain 레이어를 포함하고있다는 특징이 있습니다.

***

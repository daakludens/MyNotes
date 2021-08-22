# n-tier architecture or layer architecture
여러 레이어로 생성된 애플리케이션 구조
프로젝트 크기나 기능으로 레이어 개수는 달라지지만 보통 4개의 레이어를 가지게 된다
- presentation : UI 및 브라우저의 request를 받고 요청된 값을 리턴해주는 레이어
- business : 요청에 필요한 로직 수행
- persistence : database의 모델을 받아오는 형식
- database : 데이터 저장 및 제공

<br>

### 특징
**separation of concern**
각 레이어는 자신의 역할에만 집중할 수 있고 다른 레이어의 과정에 대해 알 필요가 없다
독립성 보장

<br>

**closed layer**
레이어를 뛰어 넘어 바로 다른 레이어로 갈 수 없음
무조건 그 다음 단계의 레이어를 거쳐서 이동할 수 있다
- 한 레이어 내 변경 사항이 있어도 다른 레이어에 영향이 없어 확장성과 유지 보수성이 있음
- 공통으로 사용되는 기능에 접근 제어를 해 무작위로 쓰이는 걸 방지할 수 있음

<br>

### 주의 사항
**architecture sinkhole anti-pattern**
자칫하면 특정 안티 패턴이 생길 수 있음 → 레이어 내 로직이 없고 그냥 신호와 데이터만 왔다 갔다 하는 상황
80대 20 법칙으로, 안티 패턴이 20이면 괜찮지만, 안티 패턴의 비율이 과반수가 된다면 아키텍처 구조 상 문제가 있는 거다.
그럴 때는 구조를 바꾸거나 open layer로 변경해 레이어 간 점프가 가능하게 해도 된다.
*** 대신 open layer로 하면 레이어 간 독립성이 떨어질 수 있는 가능성은 염두해 둬야 한다

<br>

**monolithic architecture**
레이어 별로 구분되어 있다 해도 애플리케이션 전체가 하나의 덩어리로 돌아가고 있는 모양새다.
이 구조의 안전성, 확장성, 성능 등에 관한 이슈가 있다.

<br>

### 전반적인 특징
해당 구조는 리팩토링이나 배포 과정에서 조금의 변화가 생기면 애플리케이션 전체를 다뤄야 해서 무거운 편이다. 대신 레이어 구분이 잘 되어 있어서 테스트는 편리하고 클래식한 구조로 초기 러닝 커브는 완만한 편이다.

<br>

# microservice architecture
하나의 애플리케이션을 여러 개의 서비스로 분리해 운영

<br>

### 특징
작은 팀이 서비스를 별도로 관리 가능
서비스 간 강한 연결(tight coupling)을 지양하기 위해 보통 한 서버에 하나의 서비스를 배포(PAAS, Platform as a service)해 네트워크로 연결 시킨다. 서비스 한 개를 수정하고 배포하는 데 있어 다른 서비스는 영향이 없어야 한다.
연결성을 갖추면서 디펜던시가 강하지 않은 API 기술을 선택하는 게 굉장히 중요하다.

<br>

### 장점
각 서비스에 맞는 기능을 쓸 수 있다(굳이 한 기술에 국한되지 않고, 해당 서비스에 가장 적합한 기술을 고를 수 있다!!!) 하향 평준화 되지 않고 최대의 효과를 낼 수 있다.
또한 신기술을 도입하고 싶으나 호환성 또는 성능에 확신이 필요할 때 한 서비스에 시험해 빠르게 결과를 보고 판단할 수 있다.

<br>

### 사이드
why we should not use @Transactioanl on controller layer?
in concept of separation of concern
컨트롤러에 transaction을 거는 건 기능적으론 가능하나 각 레이어의 역할을 고려한다면 잘못된 적용이다. 로직인 만큼 비즈니스 레이어에서 써야 한다.

<br>

### 출처
[https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html)
[https://www.oreilly.com/library/view/building-microservices/9781491950340/ch01.html](https://www.oreilly.com/library/view/building-microservices/9781491950340/ch01.html)

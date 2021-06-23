# ArrayList
## ArrayList란
사이즈 조절이 가능한 리스트로 내부는 `Object 클래스의 배열`로 이뤄져 있다.
Java 8 버전은 객체 최초 생성 시 배열에 10개의 데이터를 넣을 수 있다.

3개의 생성자가 있는데
- 아규먼트가 없는 기본 생성자
- capacity(길이)를 지정할 수 있는 생성자
- Collection을 파라미터로 넘기는 생성자
만약 Collection으로 넘어왔을 때 사이즈가 0을 넘으면(값이 있다면) 자동으로 Arrays.copyOf()를 실행해 데이터를 복사해 넣는다.
그리고 넘겨 받은 값이 null이라면 NPE를 발생시킨다.

<br>

## 기본으로 생성된 사이즈보다 더 큰 값이 들어가면 어떻게 동적으로 사이즈가 늘어나는가?
ArrayList는 add() 메서드를 이용해서 마지막으로 삽입된 값 다음으로 데이터를 넣어준다.
add 메서드 내부로 계속 들어가게 되면 ensureExplicitCapacity() 메서드가 있는데, 이는 현재 데이터 개수와 최대 개수를 확인한다.
만약 최대 개수를 넘긴다면 grow() 메서드를 호출한다.
grow() 메서드는 자바 8버전 이후부터 현재 사이즈의 50%를 더한 값을 최대 개수로 산정한다. 그 다음 Arrays.copyOf() 메서드를 호출해 새로 만든 ArrayList에 데이터를 집어 넣는다.

<br>

## 삭제
만약 배열 안에 특정 데이터를 삭제하고 싶으면 데이터의 인덱스를 명시해 지울 수 있다.
데이터가 지워지면 그 다음 순서에 있던 모든 데이터는 빈 자리를 채우기 위해 앞으로 한 칸씩 움직이고 전체 배열 개수는 1 감소한다.
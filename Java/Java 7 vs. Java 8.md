# Java 7 vs. Java 8
## Java 7

<br>

## Java 8 Addition
### forEach iteration
java.lang.interable에서 forEach가 추가되었음.
java.util.function.Consumer 객체를 아규먼트로 받아서 사용한다.

### java.util.function.consumer
자바8에서 기본으로 제공하는 functional interface다.
하나의 파라미터를 아규먼트로 받아들이고 리턴값은 없도록 한다.

<br>

### Functional interface and Lambda expression
functional interface는 하나의 메서드만 정의 되어 있는 interface를 의미한다. 

<br>

### Java Stream API

<br>

### Optional
NPE 체크와 가독성이 생기는 장점이 있다.
자바에서 레퍼런스 타입은 무조건 null을 가질 수 있다.

모든 객체마다 null check를 하는 게 불편할 수 있다. (if 문으로 다 검사해야 한다)

메서드 처음마다 인자의 null 검사를 하고 시작한다.

null check에 대한 고민들이 Optional로 이어지게 된 거다.

<br>

### Date API
일반 Date와 달리 추가된 LocalDate, LocaTime은 불변 객체로써 안정성을 가진다.


<br>

### default method in Interface

<br>


## Java 8 Improvements
### Collection

<br>

### Concurrency

<br>

### IO operation

<br>

[링크](https://www.javabrahman.com/java-8/java-8-java-util-function-consumer-tutorial-with-examples/#:~:text=Consumer%20is%20an%20in,object%20without%20returning%20any%20result.)

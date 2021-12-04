# Thread
## Thread란?
실행 기능으로 JVM이 여러 thread가 동시에(concurrent) 동작할 수 있도록 지원한다.
쓰레드 시작 순서는 각 쓰레드에 부여된 우선순위에 따라 진행된다.

<br>

## Daemon thread
백그라운드에서 도는 thread로 JVM이 관리한다.
예시로는 garbage collection이 daemon thread이다.
user thread와 daemon thread가 있는데, user thread가 다 종료 되기 전까지 daemon thread는 종료되지 않고 계속해서 뒷배경에서 돈다.
Daemon thread는 보통 user thread와 같이 작업을 하기 때문에 user thread가 없으면 daemon thread도 같이 종료된다.
모든 thread 중에서 제일 낮은 우선 순위를 가지게 된다.

<br>

## synchronization(monitor)
synchronization은 여러 쓰레드가 하나의 기능을 공유할 때 하나의 쓰레드가 작업 중이면 다른 쓰레드는 접근할 수 없게 한다.
모니터라는 기능을 사용하는데, 모니터는 뮤텍스를 가지고 공유되는 기능을 사용할 수 있는 쓰레드를 정해주고, 나머지 쓰레드는 대기 시킨다.

<br>

## context switching
CPU가 현재 진행 중인 task(프로세스나 쓰레드)의 상태를 저장하고 다른 task로 넘어가 작업을 진행한다.
진행 정보는 PCB(Process Control Block)에 저장해 Register에 적재한다.
멀티 태스킹처럼 보이지만 실제로는 여러 task를 왔다 갔다 하면서 중단한 부분부터 일정 기간 동안 작업한다.
하지만 유저가 보기에는 실시간으로 여러 작업이 한꺼번에 진행된다고 생각할 만큼 빠른 처리 속도를 보인다

대신 context switching은 많은 cost를 요구하는 작업이다. 
또한 process와 thread가 있다면 process가 더 많은 cost를 요구한다.
왜냐하면 thread는 stack 외 다른 메모리를 공유하지 않지만 process는 전체 메모리를 공유하기에 불러와야 하는 메모리가 더 많기 때문이다.
변경되는 부분이 상대적으로 많음.

<br>

### critical section
여러 프로세스가 공유하는 단위(섹션)
이는 synchronized 같은 기능으로 atomicity가 보장되어야지 안정적으로 애플리케이션이 운영된다.
하나의 프로세스가 critical section에서 진행하고 있으면 다른 프로세스는 해당 critical section에 접근할 수 없다.

<br>

### mutex
critical section을 쓸 수 있는 락(lock)
프로세스가 critical section을 이용할 때 lock을 얻어야 하고, 실행이 끝나면 lock을 다시 리턴한다.
문제는 다른 쓰레드도 엑세스를 하려고 lock을 요구하면서 계속해서 lock을 얻을 수 있는지 체크하며 무한 루프를 돌면서 CPU를 불필요하게 사용하게 된다.(스핀락) 
대신 context switching이 발생할 일은 없다. context switching보다 단기 스핀락이 성능 상 더 효율적이다.

semaphore
mutex와 같은 lock 개념으로 wait과 signal 기능을 가진다.

<br>

## IO
Java에서 사용하는 Input stream과 Output stream으로 다른 소스에 데이터를 입력 또는 출력할 수 있도록 하는 기능이다.


<br>

### 블로킹 IO
IO가 발생하면 synchronized 기능으로 인해서 쓰레드가 블로킹 되고, 커널에서 진행 중인 작업이 끝날 때까지 쓰레드는 대기 상태로 돌입한다.
따라서 쓰레드는 계속 놀면서 자원을 낭비하게 된다.

<br>

### Non-blocking IO
똑같이 커널에서 작업이 진행 중이더라도 쓰레드가 대기하지 않고 다른 작업을 할 수 있게 해준다. 따라서 동시성이 올라가면서 전체 작업 속도가 향상된다.
다만 커널의 작업 완료 여부는 알 수 없기 떄문에 쓰레드가 주기적으로 커널로 돌아와서 작업 완료를 확인해야 한다. 그에 따른 코스트가 들어가긴 하지만 
완전 대기 상태 돌입보다는 성능면에서 낫다.


[IO](https://limdongjin.github.io/concepts/blocking-non-blocking-io.html#ibm-%E1%84%8B%E1%85%A1%E1%84%90%E1%85%B5%E1%84%8F%E1%85%B3%E1%86%AF)

<br>

## volatile
자바에서 사용하는 키워드로 main memory에 변수를 담는 기능이다.
멀티 쓰레드 환경에서 각 쓰레드는 CPU 캐시를 갖고 있다. 성능을 높이기 위해서 쓰레드가 쓰는 변수를 캐시에 저장해 사용한다.

![image](https://user-images.githubusercontent.com/71559880/124209764-1c4ccc00-db25-11eb-849f-a5d90540e554.png)

<br>

따라서 각 thread의 값을 불러올 때 캐시마다 저장된 값이 달라 변수 불일치 문제가 발생하게 된다.
아무리 쓰레드1에서 변수 값을 바꿔도 쓰레드1의 캐시에만 변화가 적용될 뿐 main memory는 기존 값을 그대로 가지고 있어 쓰레드2가 변수를 호출하면 main memory의 기존 값을 사용하게 된다.

그래서 여러 쓰레드에 걸쳐서 같이 사용되는 변수의 경우 volatile 키워드를 붙이면 main memory에 저장하며 불일치 문제를 해결할 수 있다.
하나의 쓰레드가 읽기와 쓰기를 하고 나머지 쓰레드가 읽기는 하는 환경에서 안정적으로 값을 불러올 수 있다.
하지만 여러 쓰레드가 쓰기 기능을 사용하려 한다면 synchronized 키워드를 사용해 atomicity를 보장하는 게 더 낫다.           
[volatile](https://nesoy.github.io/articles/2018-06/Java-volatile)

<br>

## thread safe
여러 쓰레드가 동시에 같은 기능을 사용하려 해도 문제가 없는 경우를 의미한다.          

자바에서는 thread safe 상태를 만들기 위해 여러 방법이 사용될 수 있다.
- synchronization
- final keyword
- atomic variable
- volatile keyword

<br>

## thread local
특정 쓰레드만 접근이 가능한 저장소다.                
Map 형식으로 자료 구조가 되어 있고 get과 set 메서드로 값을 불러오거나 저장이 가능하다.              
삭제하고 싶으면 remove 메서드를 쓰면 된다.            

Thread pool과 같이 쓸 때는 주의해야 한다.            
ThreadLocal을 사용한 쓰레드를 회수한 다음 다시 호출할 때, ThreadLocal 쓰레기 값이 그대로 남아 예기치 못한 결과를 낼 수 있다.             
때문에 Thread pool과 ThreadLocal을 동시에 사용하는 경우엔 수동적으로 ThreadLocal 내용을 삭제해야 안전하고 사용 가능하다.             
[설명](https://www.baeldung.com/java-threadlocal)

<br>

## CPU and Multi core

<br>

### 과연 Thread 관리만이 답일까
클린 코드에서 동시성에 대한 챕터에서 멀티 쓰레드 환경 내에서 개발자가 지킬 마음 가짐에 대해 말한다.
물론 위의 기술적인 부분도 생각을 해야 하지만 애초에 애플리케이션 구조의 깔끔한 설계와 SRP를 지키는 원칙이 있어야 한다 말한다.
그런 의미에서 객체지향적이면서 깔끔한 SOLID 원칙 등을 잘 지켜야 한다는 의미인 것 같다.
역시 개발은 어렵다...

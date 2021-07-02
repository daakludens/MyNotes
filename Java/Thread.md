# Thread
## Thread란?

<br>

## synchronization(monitor)
synchronization은 여러 쓰레드가 하나의 기능을 공유할 때 하나의 쓰레드가 작업 중이면 다른 쓰레드는 접근할 수 없게 한다.
모니터라는 기능을 사용하는데, 모니터는 뮤텍스를 가지고 공유되는 기능을 사용할 수 있는 쓰레드를 정해주고, 나머지 쓰레드는 대기 시킨다.

<br>

## context switching
### critical section
여러 프로세스가 공유하는 단위(섹션)
이는 synchronized 같은 기능으로 atomicity가 보장되어야지 안정적으로 애플리케이션이 운영된다.
하나의 프로세스가 critical section에서 진행하고 있으면 다른 프로세스는 해당 critical section에 접근할 수 없다.

### mutex
critical section을 쓸 수 있는 락(lock)
프로세스가 critical section을 이용할 때 lock을 얻어야 하고, 실행이 끝나면 lock을 다시 리턴한다.
문제는 다른 쓰레드도 엑세스를 하려고 lock을 요구하면서 계속해서 lock을 얻을 수 있는지 체크하며 무한 루프를 돌면서 CPU를 불필요하게 사용하게 된다.(스핀락) 
대신 context switching이 발생할 일은 없다. context switching보다 단기 스핀락이 성능 상 더 효율적이다.

semaphore
mutex와 같은 lock 개념으로 wait과 signal 기능을 가진다.

<br>

## IO 블로킹

<br>

## Non-blocking IO

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

# StringBuffer vs StringBuilder
## 정의
StringBuffer와 StringBuilder는 문자열 연산에 사용되는 객체다.
new로 선언한 다음 인스턴스.append() 함수를 이용해 문자열을 추가할 수 있다.

<br>

## 차이
StringBuffer는 `synchronized`를 지원하기 때문에 멀티 쓰레드 환경에서 thread-safe하다.
StringBuilder는 synchronized 지원이 없기에 멀티 쓰레드 환경에서 위험하다.

StringBuilder는 StringBuffer보다 나중에 나왔는데, 이유는 StringBuffer가 싱글 쓰레드 환경에서 쓰기엔 과한 기술이라는 점이다.
그래서 싱글 쓰레드 환경에서 쓸 수 있는 StringBuilder가 나왔다는 거다.
또한 StringBuilder가 StringBuffer보다 속도가 빠르기 때문에 여러 쓰레드가 쓰이는 상황이 아니라면 StringBuilder를 쓰는 게 권장된다.

<br>

## String과 차이점
String은 불변(immutable) 객체로 문자열 연산이 일어날 때마다 새로운 객체를 생성해 문자열을 집어 넣고, 이전 String 객체는 gc 관리 대상이 된다.
연산이 자주 일어나는 상황에선 쓰레기 객체가 heap 영역에 계속 쌓이면서 메모리 부족으로 성능에 악영향을 끼칠 수 있다.

그래서 연산이 자주 발생한다면 가변(mutable) 객체인 StringBuilder와 StringBuffer를 쓰는 게 좋다.

# String
## 왜 String 객체로 반복 연산을 하면 안 되는가?
String 객체는 문자열을 다루는 자료형이다.

String은 불변 객체이기 때문에 가공이 불가능하다.             
그래서 String으로 문자열을 연산하면 원래 있던 객체에 더해지는 게 아니라 `매번 새로운 String 객체를 생성`해 그 안에 연산된 문자열을 집어 넣는다.

![image](https://user-images.githubusercontent.com/71559880/123015332-18bd9480-d403-11eb-8058-389c09186d0e.png)

<br>

이 과정에서 생겨나는 String 객체는 쓰레기 값으로 Java에서 gc(garbage collection) 대상이 된다.
gc는 언제 수행될지 파악할 수 없고, major gc가 발생하게 되면 실행 중엔 애플리케이션 동작이 멈추기 때문에 성능이 저하된다는 특징이 있다.
그래서 되도록 major gc가 발생하지 않도록 쓰레기 객체를 생성하지 않도록 코드를 짜는 게 중요하다.

<br>

그래서 문자열 연산이 많은 경우에는 똑같이 문자열을 다루면서 가변 객체인 StringBuffer나 StringBuilder를 사용해야 한다.

<br>

## 왜 String은 불변 객체인가?
우선 불변 객체라는 것은 객체 내 데이터를 바꾸지 못하는 객체를 의미한다. 객체의 레퍼런스는 변경 가능하다.
String에서 불변 객체로 만들기 위해 final 키워드를 붙인다.

자바에서는 JVM이 String을 캐싱하기 위한 `String Pool`이 존재한다.
String 객체의 효율성을 극대화 하기 위해 pool에 담아두고 호출될 때 값을 제공한다.
String Pool이 있는 환경에서 String을 불변 객체로 만들면 장점이 2개 생긴다.
- 많은 사용자가 같은 String을 공유하는데 누군가 임의로 값을 변경하게 되면 다른 사용자들도 변경된 값을 사용하면서 오류가 발생할 위험을 방지할 수 있다.
- 변경될 위험이 없기에 메모리에 String의 literal 객체 copy 하나만 저장하면 되서 메모리 공간 이용 효율이 높아진다. 이를 `interning`이라고 한다.
- DB 패스워드나 토큰 키 등 문자열이 불변이면 외부에서 공격이 들어와도 방어할 수 있는 보안이 형성될 수 있다.
- 멀티 쓰레드 환경에서 추가적인 synchronization 설정이 필요 없다. 

<br>

## interning?
interning이라는 단어는 처음 보는데...
우리가 자주 쓰는 인턴의 개념으로도 쓰지만 "구금하다, 억제하다" 라는 의미도 있다고 한다.(왠지 서글퍼지는 이유는 뭘까...)
어쨌든 자바 String에서 interning은 여기저기서 쓰이는 동일한 문자열을 메모리 내 한 공간에 저장해 공유한다는 의미다.
Java 7부터 intern된 객체는 힙 메모리 내 지정된 공간에 저장된다고 한다. 이 공간의 크기는 고정되어 있어서 너무 많은 intern 객체를 저장하면 문제가 생긴다고 한다.         
[intern이란](https://stackoverflow.com/questions/10578984/what-is-java-string-interning)

결과적으로 String을 불변 객체로 만들어 문자열을 요청할 때마다 메모리 내 객체의 동일한 레퍼런스를 제공해 메모리 점유율을 낮추는데 의미가 있다.

<br>

## String literal vs String Object
Object는 String 문자열을 생성할 때 new() 연산자를 사용해 만드는 방법이다.
이때는 무조건 새로운 객체가 생성되면서 객체의 메모리 주소도 다르다.

literal은 String s = "..."; 방식으로 문자열을 선언한다.
이 방식은 String이 intern이 되면서 기존에 있던 String 객체를 재활용할 수 있다.
불필요하게 객체를 새로 생성할 필요 없이 기존 객체를 optimize 가능하기 때문에 literal 방식으로 사용하는 게 권장된다.

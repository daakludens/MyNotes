# equals() vs hashCode()
## 정의
equals()와 hashCode()는 전반적으로 두 대상이 일치하는지 비교하기 위해 사용된다.       
하지만 둘은 비교 기준이 다르다.

<br>

### equals
equals는 `value`가 같은지 체크를 하고 hashCode는 `객체`가 같은지 체크를 한다는 차이가 있다.

더 자세하게 보자면 equals는 기본적으로 this == obj로 단순하게 값을 비교한다. 
그래서 기본 자료형의 경우 true가 나오지만 참조 자료형(객체, Collection 등)은 같은 value를 가지고 있어도 false가 나온다.
제대로 동작하게 하려면 equals를 override해서 객체 내 value를 비교하도록 설정해야 한다.

<br>

### hashCode
hashCode는 Hash 함수로 Collection에서 사용된다. 같은 객체의 같은 hash 코드(값)를 반환하며, 다른 객체라고 해도 굳이 다른 hash 코드를 반환하진 않는다.

hash 값은 데이터를 hashCode에 넣어 정수형 hash를 도출하며, 저장된 객체의 hash 주소를 찾는데 사용된다.
hash 주소를 따라가면 해당 객체의 bucket으로 소개해준다. bucket은 객체의 키-값 페어를 담으며 Java 8부터 TreeMap으로 이뤄진 array 구조로 이뤄져 있다. 
TreeMap array 구조를 사용하는 이유는 속도가 빠르기 때문인데, 이는 hash 값이 bucket의 인덱스 값과 일치하는 곳을 찾아 key를 대조해 value를 가져오면서
키와 값이 1대1 관계를 가지는 연관배열구조로 구성되어 있기 때문이다. 유일하게 일치하는 값만 찾아 시간 복잡도가 O(1)을 가진다.
주소값은 보통 31을 곱한 숫자와 이름을 붙여서 찾는다. 31은 홀수로 결과가 overflow 되면 왼쪽이 아닌 오른쪽으로 shift 하기 때문에 데이터를 잃을 위험이 없다.

다만 주의할 점은 hash 값을 도출하는 해시 함수는 유한한 값을 낸다. 즉, 다른 키값을 넣어도 같은 hash 값을 내는 경우가 발생한다는 거다.
이는 해시 충돌이라고 하며 해시 충돌이 일어나면 시간 복잡도가 O(N)이 되면서 굉장히 비효율적인 코드가 된다.
시간 복잡도가 바뀌는 이유는 유일한 값이던 hash 값이 여러 경우의 수를 가지게 되면서 유일성이 떨어지기 때문이다.

그래서 hash 코드가 겹치지 않는 hash 함수를 만드는 게 중요하며, 그 과정에서 너무 많은 시간이 소요되지 않도록 설계하는 게 중요하다.
해결법으로 separate chaining과 open addressing이 있다. separate chaining은 충돌이 난 새로운 데이터를 기존 데이터에 연결해 해시를 유지시키며 같이 저장한다. 
open addressing은 비어 있는 해시를 찾아서 데이터를 넣고 해시 값을 변경한다.

hash 함수를 이용하는 Collection 객체를 메모리에서 찾으려고 할 때는 두 단계를 거친다.
- hashCode()를 사용해서 bucket 찾기
- bucket에서 equals()를 이용해 값 찾기

<br>

## 우리가 오버라이딩할때 어떤 것들을 고려하며 해야할까?
만약 equals를 override 했다면 안전 상 hashCode도 override 해주는 게 좋다.

equals만 override하는 경우 두 번의 new로 객체를 생성해도 둘은 equals로 같은 객체라고 인식이 된다. 
하지만 두 객체를 동일한 hash 객체에 집어 넣을 때 다른 객체로 인식되어 각기 다른 bucket으로 값이 저장된다.

override를 하지 않으면 equals()가 override 되어 있어도 모든 객체를 다르게 인식해 다른 해시 코드를 부여하기 때문이다.



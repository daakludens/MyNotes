# Serialization
## Serialization이란?
객체를 byte Stream으로 변환한다. 그러면 변환된 데이터를 네트워크로 넘기거나 파일로 변환 또는 DB에 데이터로 보관하는 일이 가능하다.

클래스에 적용할 때는 `Serializable`을 implement하면 된다.
Serializable 같은 경우도 ObjectInputStream과 ObjectOutputStream을 implement 받는다.

여기서 데이터를 직렬화 대상에서 제외하고 싶은 경우 `transient` 키워드를 쓰면 된다.

<br>

## serialVersionUID
가끔 보면 final String으로 위의 변수를 선언하는 경우를 볼 수 있다.
이는 클래스 버전을 관리하기 위함이다.
JVM은 serialize된 객체에 버전 관리가 안 되어 있으면 임의로 생성해서 직렬화된 객체를 저장한다.
하지만 객체에 변화가 있은 다음 직렬화된 데이터를 불러오면 다른 객체로 인식하고 InvalidClassException 예외를 던진다.
따라서 serialVersionUID를 지정해 내용이 변하더라도 같은 객체임을 보장할 수 있다.
직렬화하는 객체에는 무조건 붙이는 게 권장된다.

<br>

## 주의할 점
직렬화와 역직렬화는 데이터 타입에 굉장히 엄격하게 비교해서 보기 때문에 조금이라도 어긋나면 에러가 나기 쉽상이다.
따라서 어느 개발자는 변화가 너무 잦거나 오랫동안 찾게 되지 않을 객체의 경우에는 직렬화를 하지 않는게 좋다고 한다.

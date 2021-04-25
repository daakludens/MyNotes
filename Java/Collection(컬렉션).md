# 컬렉션
목록성 데이터를 다루는 자료 구조

자료 구조는 데이터를 담는 그릇이다.
데이터의 형태나 특징에 따라서 그에 맞는 그릇에 담아야 효율적으로 데이터를 보관할 수 있다.

자료 구조는 크게 4가지 형태로 나눌 수 있다.
1. List
순서가 있다
2. Set
순서가 중요하지 않음
3. Queue
FIFO(First In First Out) 형식을 따름
여기서 FIFO는 처음 들어온 데이터가 먼저 나간다는 형식을 의미한다.
4. Map
키-값(key-value)형식으로 저장

List, Set, Queue는 Collection 인터페이스를 구현하며, Map은 별도의 인터페이스로 구현되어 있다. 

![image](https://user-images.githubusercontent.com/71559880/115981677-7040af00-a5d0-11eb-8502-868f78315c46.png)

<br>

여기서 Collection 인터페이스는 Iterator 인터페이스를 구현한다. Iterator 인터페이스의 메서드를 보면 특징을 알 수 있다.
- 추가 데이터를 확인하는 hasNext() 메서드
- 현재 위치를 다음 요소로 옮겨주는 next() 메서드
- 데이터를 삭제하는 remove() 메서드
등이 있다.
즉, 해당 메서드들로 데이터를 순차적으로 가져올 수 있음을 의미한다. Map의 경우 값을 가져오는 기준이 key가 되므로 데이터의 순차와 관계가 없는 데이터 구조이다.

## List
순서가 있는 데이터 목록을 다룰 때 사용되며, 주로 ArrayList, Vector, Stack, LinkedList를 많이 사용한다.
여기서 ArrayList와 Vector는 거의 동일한 특징을 가지는 확장 가능한 배열이며, 차이점으로 `Vector는 thread-safe`하나 ArrayList는 thread-safe하지 않다.

Stack의 경우 Vector를 확장해 만들었으며 LIFO(Last In First Out)를 지원한다. LIFO는 FIFO인 Queue와 다르게 마지막에 들어간 요소(element)가 먼저 나오는 구조다.

<br>

## FIFO(Queue)와 LIFO(Stack)
처음 들었을 때 둘 다 축약어에 비슷해서 헷갈릴 수 있다.
이럴 땐 이름의 의미를 곱씹어 보면 기억하기 쉽다.
FIFO인 Queue는 영어로 기다리는 줄을 뜻한다.
이마트 같은 매장에서 계산하기 위해 카트를 끌고 한 줄로 대기하며 순서를 기다리는 모습을 생각하면 된다.
뭐가 됐건 먼저 온 순서대로 무조건 계산 처리를 한다.

LIFO인 Stack은 쌓여져 있는 물건 더미를 가르킬 때 사용한다.
예를 들어 카지노에서 칩 더미를 한 줄로 위에서 부터 쌓아 보관한다.
베팅에 성공해 칩을 얻으면 기존의 줄의 위에 쌓아서 더 높게 쌓고 베팅할 때 위에 있는 칩부터 집어서 쓰게 된다.

<br>

### ArrayList
`순서에 유의미`하며 `값이 중복 가능`한 목록으로 기본 생성 시 10개 데이터를 넣을 수 있는 자료구조다.
입력할 데이터 개수가 확실하다면 생성할 때 명시하는 게 좋지만, 아니라면 기본 생성 뒤 추가해나가는 게 좋다.

ArrayList에 여러 자료형을 넣을 수는 있지만, 보통 하나의 동일한 자료형을 보관하는 용도로 쓰인다.
굳이 다양하게 넣고 싶다면 List보다 `DTO`를 사용하는 게 낫다.
또한 컬렉션의 경우에는 제네릭 방식으로 자료형을 명시해주는 게 좋다

```
ArrayList arrayList1 = new ArrayList();

// 권장
ArrayList<String> arrayList2 = new ArrayList<>();
```

<br>

add() 메서드는 element 하나를 입력하고 addAll() 메서드는 매개 변수로 넘어온 컬렉션 데이터를 담는다.
두 메서드 모두 맨 뒤에 입력하거나 인덱스 값을 넣어 입력할 위치를 지정할 수 있다.

주의할 점은 ArrayList list1을 생성해서 ArrayList list2에 list1을 매개변수로 넘겨 줬을 때, list1과 list2는 데이터 뿐만 아니라 객체를 참조하는 주소값까지 같아진다.
즉, list1의 데이터 목록을 변경하면 그대로 list2에도 영향이 가게 된다. 이를 shallow copy라고 한다.
원본 컬렉션인 list1을 보존하고 list2를 바꾸고 싶다면 deep copy 방식인 arraycopy() 메서드를 이용해 복사해야 안전하다.

앞서 썼듯 ArrayList 내 데이터는 중복 가능하다. 따라서 get() 메서드로 값을 가져올 때 어느 위치의 값을 가져와야 할지 정해야 하는 경우가 있다.
그때는 indexOf()나 lastIndexOf() 메서드를 사용해 위치를 지정할 수 있다.

remove() 메서드에 매개변수로 인덱스 값이나 제거하고 싶은 데이터를 넣을 수 있다. 객체를 넘기는 경우 remove()는 동일 데이터들의 첫번째 데이터만 삭제하고, removeAll() 메서드를 사용하면 동일 데이터 전체를 지울 수 있다.

그리고 ArrayList는 Vector와 달리 thread-safe하지 않다.
그래서 멀티 쓰레드 환경에서 List를 만들 때 Collections.synchronizedList(new ArrayList())... 같은 방식으로 만들어줘야 한다.

<br>

### Stack
Stack은 Vector를 확장해 만든 컬렉션으로 thread-safe하나 같은 LIFO 방식의 ArrayDeque보다 속도가 느리다는 특징이 있다.

Stack의 메서드 중 peek()과 pop()이 있는데, peek()은 가장 나중에 들어온 데이터를 리턴하고, pop()은 나중에 들어온 데이터를 리턴하며 삭제한다.
LIFO 방식에는 pop()가 오히려 어울린다 한다.

<br>

## Set
Set은 순서에 상관 없이 해당 데이터가 존재하는지 확인하기 위해 사용되는 컬렉션이다.
따라서 `중복 데이터는 허용하지 않는다`.
예를 들어 중복된 사용자들의 총 사용자 수를 집계할 때, Set의 특징을 이용해 쉽게 값을 얻을 수 있다.

Set 컬렉션은 `데이터 정렬`에 따라 성능이 좌우된다.

<br>

### HashSet
hash table에 저장하는 방식으로 Set 중에 가장 성능이 좋다.
중복 데이터 확인 여부가 중요한 컬렉션이라 equals()와 hashCode() 메서드가 주요하게 쓰인다.

HashSet에서 중요한 기능은 `load factor`다.
load factor는 데이터 개수 / 저장 공간으로 계산한다. 데이터 개수가 늘어나면 그만큼 재정리(rehash) 작업이 자주 일어나 성능이 느려지게 되고, 필요 이상으로 저장 공간을 늘리면 데이터 검색에 시간이 걸려 성능이 느려진다. 때문에 제일 적합한 로드 팩터를 예상해 저장 공간을 설정하는 게 중요하다.

<br>

### TreeSet
red-black 트리(tree)라는 이진(binary) 트리 타입으로 저장하며 HashSet보다 성능이 느리다.

<br>

### LinkedHashSet
연결된 목록 타입으로 구성된 hash table에 저장한다. 저장된 순서로 값이 정렬되는 대신 성능이 제일 느리다.

<br>

### Queue
FIFO 형식이다. 예시로 서비스 이용자가 들어와서 웹사이트를 요청할 때 들어온 순서대로 처리해줄 때 사용한다.

<br>

### LinkedList
LinkedList는 `List`, `Queue`, `Deque` 인터페이스 3개를 모두 구현하고 있다.
여기서 Deque는 맨 앞의 값 또는 맨 뒤의 값을 빼는데 유용하게 사용된다.

![image](https://user-images.githubusercontent.com/71559880/115987159-c45a8c00-a5ee-11eb-9420-fe2778ddff7f.png)

<br>

### ArrayList vs LinkedList
ArrayList의 경우 전체 값을 불러올 때 굉장히 빠르지만 값이 삭제되는 경우 그 데이터 뒤의 값을 전부 한 칸 씩 옮기면서 성능 저하가 생길 수 있다.
LinkedList는 앞뒤 노드와 연결되어서 값이 제거 되도 앞뒤 노드만 연결하면 되기에 삭제 기능은 더 빠르다는 장점이 있다.
결론적으로 ArrayList는 값을 호출하거나 변경하는 요청이 잦으면 유용하고 LinkedList는 중간중간에 값을 삭제하는 경우가 많을 때 더 유용하다.

<br>

## Map
키-값 형태로 저장되는 데이터 구조로 키나 값만 저장할 수 없으며 키는 고유해야 하지만 값의 경우 중복되도 괜찮다.
메서드로는 주로 입력, 호출, 삭제로 put(), get(), remove()가 사용된다.

Map을 구현하는 클래스는 HashMap, TreeMap, LinkedHashMap 등 있으며 그 중에 `Hashtable`이라는 클래스도 있다.
Hashtable은 구현은 했으나 일반 Map 객체와 차이점이 존재한다.
먼저 Hashtable은 Enumeration 객체를 사용해 데이터를 처리하고 키-값 쌍으로 데이터를 순환해 처리할 수 없으며 Iteration 도중 데이터를 안전하게 삭제하는 법을 제공하지 않는다.
또한 null을 키-값에 저장할 수 없으나 멀티 쓰레드 환경에서는 안전하다.
Map이 thread-safe하지 않기에 안전하게 사용하려면 Collections.synchronizedMap(new HashMap()..)을 사용하거나 ConcurrentHashMap을 쓰면 된다.

<br>

### HashMap
HashMap의 경우 키는 기본 자료형과 참조 자료형 둘 다 사용할 수 있다.
하지만 키를 직접 생성할 경우 hashCode()와 equals()를 구현해놓아야 한다. 왜냐하면 HashMap에 객체가 들어가게 되면 버켓이라는 곳에 저장된다.
버켓은 키가 다르더라도 hashCode()의 결과가 동일하면 버켓에 여러 값이 들어가게 되고, 정확한 값을 찾으려면 equals()를 이용해야 하기 때문이다.

put() 메서드를 이용해서 키-값을 입력할 때, 이미 존재하는 키로 넣으면 새로운 값으로 대체된다. 만약 get() 메서드로 존재하지 않는 키를 불러오면 ArrayIndexOutOfBoundsException을 발생시키면서 null을 반환한다.

Map 내 전체 키를 가져오려면 KeySet 명령어를 사용해야 한느데, 말 그대로 Set 자료형을 띄기 때문에 순서에 구애 받지 않고 중복된 데이터가 없다.
```
Set<String> KeySet = map.KeySet();
```
<br>

만약 값만 가져오는 경우는 map.values()로 가져올 수 있다.
키-값 쌍을 가져오려면 entrySet() 메서드에 담아서 getKey()와 getValue()로 각각 하나씩 가져올 수 있다.

```
HashMap<String, String> map = new HashMap<>();
...
Set<Map.Entry<String, String>> entries = map.entrySet();
for(Map.Entry<String, String> tempEntry:entries) {
  System.out.println(tempEntry.getKey() + "=" + tempEntry.getValue());
}
```

<br>

### TreeMap
기본적인 Map 형식에서 키를 정렬하는 특징이 있다. 데이터가 많아질 수록 정렬 기능으로 느려질 수 있지만, 수가 적당하며 정렬할 필요가 있다면 HashMap보다 TreeMap을 사용하는게 유용하다.

<br>

### properties
Properties 클래스를 이용해서 여러 속성값을 저장하고 필요에 따라 빼서 쓸 수 있다.
이는 스프링 부트에서 사용되는 application.properties에 우리가 직접 값을 넣을 수 있는 걸 자바에서는 객체를 생성에 직접 주입하는 방식으로 구현하고 있다.
```
Properties prop = new Properties();
prop.setProperty("name", "Coffee Bean");
prop.setProperty("menu", "Americano");
...
```

이러면 user.dir이라는 경로에 파일이 저장되면서 properties 파일에 키-값이 저장되는 걸 볼 수 있다.

<br>

### 출처
자바의 신 22장-24

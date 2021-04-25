# 컬렉션
목록성 데이터를 다루는 자료 구조

자료 구조는 데이터를 담는 그릇이다.
데이터의 형태나 특징에 따라서 그에 맞는 그릇에 담아야 효율적으로 데이터를 보관할 수 있다.

자료 구조는 크게 4가지 형태로 나눌 수 있다.
1. List
순서가 있음
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

### ArrayList

### LinkedList

### Stack

## Set

### HashSet

### Queue

## Map

### HashMap

### TreeMap


### 출처
자바의 신

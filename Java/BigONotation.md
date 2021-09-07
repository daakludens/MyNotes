# Big O Notation
##

![image](https://user-images.githubusercontent.com/71559880/124890894-73660b80-e013-11eb-912d-81777ba593f2.png)

<br>

출처:[Big O Notation](https://www.bigocheatsheet.com/)

## HashMap
보통은 O(1)이지만 최악의 경우(= hash code collision) O(n)이 된다.    
최악의 경우란 해시값 충돌이 발생해서 동일한 해시값을 가지는 버킷이 2개 이상 생성될 때 발생한다.
이때는 한 번이 아니라 충돌되는 개수만큼 검색이 되기 때문에 그만큼 시간이 더 걸린다.

<br>

## stack vs queue
Stack과 Queue의 insert 및 delete 시간이 과연 다를까? -> 같다...

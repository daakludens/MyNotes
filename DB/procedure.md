# Procedure
## Procedure란
쿼리문을 일련의 과정으로 실행시키는 방법.

함수 : 일련의 과정을 거쳐 결과를 반환           
프로시져 : 여러 쿼리문을 동시에 수행

<br>

## 실행 예시
더미 데이터를 만드는 예시로 아이디 i를 1씩 증가시키면서 더미 데이터를 INSERT함.

```java
DELIMITER $$
DROP PROCEDURE IF EXISTS generateGame$$
 
CREATE PROCEDURE generateGame()
BEGIN
    DECLARE i INT DEFAULT 1;

    WHILE i <=20 DO
        INSERT INTO game(id, title, genre, description, release_date, price, developer, publisher, rating, sales, status)
        VALUES(i, concat("game", i), 3, "게임 설명", now(), 30000, 2, 1, 5.5, 14, 4);
        SET i = i + 1;
    END WHILE;
END$$
DELIMITER ;

CALL generateGame;
```

<br>

## delimiter 
구획 문자. 문장이나 구역 분리 등에 쓰이는 문자. db 쿼리문의 끝을 알리는 세미 콜론과 같은 역할.           
프로시저의 처음과 끝에 사용해서 하나의 프로시저를 표현할 수 있다.          
또한 내부 쿼리문은 statement마다 세미 콜론을 쓰기 때문에 구분할 용도로도 괜찮다.           
필수적인 기능은 아니지만 norm으로 사용되는 듯하다. 

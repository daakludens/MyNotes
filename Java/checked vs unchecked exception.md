# Checked Exception vs Unchecked Exception
### checked exception
- 컴파일 때 실행
- 프로그램의 외적 영향으로 생김
- throws 키워드나 try-catch 문을 사용해 예외 처리 가능
- 사용자가 해당 예외로 부터 복구할 수 잇다
- 예외 발생 시 rollback을 하지 않음
- 예시 : IOException, SQLException

<br>

### unchecked exception
- 런타임 때 실행
- 프로그램 내부에서 발생
- thrwos나 try-catch문은 쓰지 않음
- 사용자가 해당 예외에서 복구할 수 없다
- 예외 발생 시 rollback을 해야 함
- 예시 : 

<br>

예외 처리 방법에는 예외 복구, 회피, 전환의 방법이 있다
- 예외 복구 : 그 자리에서 바로 예외를 수습한다
- 예외 회피 : 해당 메서드를 부른 클래스에 예외를 던진다
- 예외 전환 : 더 명시적인 예외로 바꿔서 예외를 던진다.

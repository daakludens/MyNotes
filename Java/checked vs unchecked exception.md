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

<br>

참고로 검사 처리(checked exception)은 적절히 쓰면 안정성 있는 API가 되지만 남용하게 되면 쓰기 불편하고 어려운 API가 된다.
따라서 정말 써야할 때만 적용해주는 게 좋다.
적용할 때도 먼저 옵셔널 반환으로 충분한지 체크한 다음, 정보가 불충분해 보인다면 다른 데이터를 넘겨주는 방식으로 처리하자.

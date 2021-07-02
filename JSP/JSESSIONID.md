# JSESSIONID
## 정의
JSESSIONID는 was(톰캣 또는 네티 같은)에서 세션을 구분하기 위해 발급되는 키다.
세션의 키는 JSESSIONID, 값은 세션 아이디가 된다.

<br>

## 전송 방법
클라이언트 요청이 들어왔을 때 was는 세션에 JSESSIONID가 있는지 확인을 한다.
없다면 새로 JSESSIONID를 생성해 set-cookie http 헤더에 넣어 전송한다.
그러면 다음부터 클라이언트는 받았던 쿠키를 매 요청마다 보내고 서버는 보내온 쿠키 내 JSESSIONID가 메모리에 저장된 JSESSIONID와 일치하는지 확인한다.
동일한 JSESSIONID임이 확인되면 같은 유저임을 파악할 수 있다.



- 세션에서 `JSESSIONID` 가 어떻게 관리될까?
- 서버에서 클라이언트로 어떻게 전달할까
- 클라이언트는 이것을 어떻게 관리할까
- 클라이언트는 서버에 이것을 어떻게 전달할까


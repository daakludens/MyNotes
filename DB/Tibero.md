# Tibero 6 설치
## 라이선스 발급
회원 가입해 로그인 후 데모 라이선스 발급                  
공식 사이트 : [https://technet.tmaxsoft.com/ko/front/main/main.do](https://technet.tmaxsoft.com/ko/front/main/main.do)

주의할 점은 라이선스 신청 작성란에 host명을 제대로 적어줘야 한다.            
host명은 설정의 컴퓨터 정보의 디바이스 이름 또는 터미널에서 명령어를 치면 나온다.
```
# Windows
hostname

# Linux
uname -n
```

<br>

라이선스는 나중에 다운 받은 파일 밑에 tibero6\license 폴더에 넣어준다.                
라이선스 받기 : [https://webobj.tistory.com/20](https://webobj.tistory.com/20)

<br>

## 다운로드
다운 받을 때는 지정해준 값 그대로 설정

### instance configuration
```
TB_HOME : C:\TmaxData\tibero6
TB_SID : tibero
Database Name : tibero 
Listener Port : 8629
```

<br>

다운 받은 파일은 C 드라이브 밑에 Tibero 폴더를 새로 생성한 다음 그 안에 넣어주자

<br>

### 환경 변수 편집

```
# 시스템 변수
TB_HOME : C:\Tibero\tibero6
TB_SID : tibero

# 시스템 변수 Path
%TB_HOME%\bin
%TB_HOME%\client\bin
```

<br>

## 설정 및 실행
### cmd 관리자 권한으로 실행

`tbinstall %tb_home% %tb_sid%` 적용 시도 때 에러가 날 때 uninstall을 한 번 해주고 다시 install하면 된다.

```
cd c:\Tibero\tibero6\bin

tbuninstall.vbs

tbinstall %tb_home% %tb_sid%
```

<br>

```
C:\Tibero\tibero6\config> gen_tip.bat
```

<br>

tbboot nomount가 안 되면 한 번 clean을 하고 다시 입력

```
# 부팅 전 지우기
tbdown clean

# tibero 실행
tbboot nomount

# tibero 중지
tbdown immediate
```

Tibero 설치 방법 : [https://webobj.tistory.com/21](https://webobj.tistory.com/21)

<br>

## 외부 접속
![image](https://user-images.githubusercontent.com/71559880/118369495-901b2f80-b5de-11eb-82ca-79100ed7094e.png)

<br>

### listener
- Oracle의 경우 localhost에서는 바로 접속이 가능하지만 외부에서 접속하려면 클라이언트에 있는 `listener`를 같이 설정해야 연결 가능하다
    리스너 설정을 위해 `tnsnames.ora` 파일을 수정

- Tibero의 경우에는 `tbdsn.tbr` 파일을 통해서 리스너를 정의한다
    tbdsn.tbr은 클라이언트가 Tibero의 `데이터베이스에 접속하기 위해 필요한 정보를 가지고 있는 환경 설정 파일`이다.
    Windows 경로 : C:\Tibero\tibero6\client\config\tbdsn.tbr

```
tibero=(
    (INSTANCE=(HOST=localhost)
              (PORT=8629)
              (DB_NAME=tibero)
    )
)

WATERNET=(
    (INSTANCE=(HOST=192.168.0.77)
              (PORT=8629)
              (DB_NAME=tibero)
    )
)
```

tbdsn.tbr 파일 구조 설명 : [https://technet.tmaxsoft.com/upload/download/online/tibero/pver-20140808-000002/tibero_admin/appendix_A.html](https://technet.tmaxsoft.com/upload/download/online/tibero/pver-20140808-000002/tibero_admin/appendix_A.html)          
설정에다가 tbsql 문으로 호출할 때 리스너를 같이 붙여서 명령어를 쳐야 한다

```
#SQL> 들어가기 위한 명령어
tbsql

SQL>
# 사용자 연결
connect sys/tibero

# 리스너를 통해서 waternet 접속
tbsql sys@WATERNET
tbsql sys/tibero@WATERNET
tibero

# 확인용
select tname from tab;
```

티베로 접속 방법 : [http://blog.naver.com/PostView.nhn?blogId=dark009&logNo=220004760464&parentCategoryNo=&categoryNo=58&viewDate=&isShowPopularPosts=true&from=search](http://blog.naver.com/PostView.nhn?blogId=dark009&logNo=220004760464&parentCategoryNo=&categoryNo=58&viewDate=&isShowPopularPosts=true&from=search)          
오라클 listener 동작 방식 : [http://www.gurubee.net/lecture/2811](http://www.gurubee.net/lecture/2811)

<br>

## sql 파일 실행
```
# sql 파일 동작하기
# @ 뒤에 sql 파일의 경로 지정
@'C:\Users\heejaeahn\OneDrive\Desktop\insert\IF_2.sql';

# commit 명령어를 쳐야지 dbeaver에 동기화 되어 적용됨
SQL > commit;
```

tbsql sql 파일 명령어 : [https://ngio.co.kr/7968](https://ngio.co.kr/7968)

<br>

### Tibero Studio
만약 Tibero GUI 툴을 사용하고 싶으면 Tibero Studio가 있음             
Tibero admin의 최신 버전으로 공식 사이트에서 Tibero 다운로드 페이지에서 그냥 다운로드 하고 exe 파일을 실행시키면 된다.              

<br>

## 한글 깨짐 문제
다운로드한 tibero를 터미널을 통해 값을 insert할 때 한글 깨짐 현상이 생겼다.              
이유는 character encoding의 부조합 때문이다.             

현재 캐릭터 인코딩을 사용하는 주체는 총 3개다
- 서버
- 클라이언트
- 터미널

터미널은 기본적으로 949 ANSI/OEM Korean (Unified Hangul Code)를 사용하고 있어 생긴 문제였다. 이걸 Unicode (UTF-8)로 변경해주면 된다.

```
# Tibero
# Tibero 데이터베이스 인코딩 확인
SELECT * FROM sys."_VT_NLS_CHARACTER_SET" ;

# Tibero 클라이언트 전체 인코딩 overriding
# tbdsn.tbr
TB_NSL_LANG=UTF8

# terminal
# 터미널 인코딩 확인
chcp

# 인코딩 변경(UTF-8)
chcp 65001
```

tibero와 터미널 환경에서 한글 깨짐이 발생하는 이유 : [http://hell0-world.com/etc/2018/10/10/client-charset.html](http://hell0-world.com/etc/2018/10/10/client-charset.html)        
cmd에서 인코딩 바꾸기 : [https://jun7222.tistory.com/494](https://jun7222.tistory.com/494)               
유틸리티 가이드 : [https://www.tmaxdata.com/img/service/pdf/Tibero 5 Utility Guide_v2.1.4.pdf](https://www.tmaxdata.com/img/service/pdf/Tibero%205%20Utility%20Guide_v2.1.4.pdf)

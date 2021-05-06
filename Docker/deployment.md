# 도커 배포
## 필요
- 도커에 올릴 프로젝트/압축파일
- Dockerfile

프로젝트 안에 Dockerfile을 만든다면 최상위 루트에 둔다. 
압축 파일(jar, war)을 사용하면 해당 파일이 있는 폴더에 생성해주면 된다.

<br>

### 추가 설정
- maven의 경우 pom.xml에서 packaging이 jar로 되어 있는지 확인 후 사용
- Project Structure에 들어가서 artifact 옵션에 jar를 새로 추가한 다음 Build → Build Artifact로 jar 생성
- **Spring MVC는 main()이 없기 때문에 executable한 jar를 생성할 수 없다. 하지만 도커로는 executable jar가 필요 없기 때문에 괜찮음**

참고 : [https://www.leafcats.com/178](https://www.leafcats.com/178)

<br>

## Dockerfile
- Docker에 image를 빌드할 때 사용되는 blue print
- 컨테이너에서만 영향을 주는 명령어다.
- 여러 줄로 이어질 경우 역 슬래시(\) 사용하는 게 convention이다.

<br>

## Dockerfile 내 명령어
### FROM 
: 시작점
무조건 필요 (ARG 외엔 제일 먼저 와야 함)         
base가 되는 다른 이미지를 가져와서 사용. 결국 여러 개의 image layer가 사용됨              
base image를 install한다고 생각하면 됨              

**adoptopenjdk**               
미리 빌드된 OpenJDK를 사용할 수 있게 해주는 툴              
`adoptopenjdk:11-jre`로 이미지를 고정시켜주면 됨                     
[https://hub.docker.com/r/adoptopenjdk/openjdk11/](https://hub.docker.com/r/adoptopenjdk/openjdk11/)

<br>

### VOLUME 
: 호스트와 공유할 디렉터리 목록을 지정. 마운트 포인트를 생성한다고 함           
Dockerfile에 volume 명령어와 경로를 같이 입력하면 컨테이너 생성 시 host를 타고 자동으로 저장됨           
/var/lib/docker/volumes/{volume_name} ← 해당 경로로 저장됨             

대신 그냥 run을 해버리면 volume이 해시 함수를 거쳐서 기억하기 힘든 이름으로 저장된다.                
그래서 run을 할 때 host의 file system과 container의 file system을 매핑할 수 있도록 할 수 있다.             

v 옵션을 사용해 마운트를 하고, :을 사용해 앞에는 볼륨 이름, 뒤에는 컨테이너 내 경로로 지정
```
docker run -d -p 8080:8080 --name test1 -v test1-vol:/app test1
# volume을 test1-vol이라는 이름으로 /app 경로에 저장

docker volume ls // volume 리스트 확인
docker volume rm test1-vol // volume test1-vol 삭제

*** 연결된 docker 컨테이너를 지워도 따로 저장된 volume은 계속 있기 때문에 삭제해 줘야 함
```

<br>

### WORKDIR 
: RUN, CMD, ENTRYPOINT의 명령이 실행될 디렉터리를 설정

<br>

### ADD 
: 압축 파일을 지정한 루트로 복사

<br>

### COPY 
: RUN과 다르게 host 컴퓨터에 적용하는 명령어로 파일을 컨테이너 파일로 옮길 수 있음         
. /home/app → 앞에 . 은 현재 폴더 의미. /home/app은 복붙될 경로 표시          

<br>

### ENTRYPOINT 
: 시작 주소. 명시하지 않으면 기본적으로 `/bin/sh -c`가 정의됨

<br>

### CMD 
: 할당된 entrypoint에서 실행될 명령어

<br>

### EXPOSE 
: 내부 포트 번호

<br>

### RUN 
: 리눅스 명령어. 컨테이너 내에만 적용됨

<br>

### ENV 
: properties 또는 yml로 설정한 환경 설정(environmetal variable) 정의         
값이 변경될 경우 override 가능하기에 써주는 게 좋음        
Spring의 경우 @Value처럼 ${ spring.msyql.password } 형식으로 저장 가능          
→ 근데 ENC()한 경우에도 잘 들어가나? 확인해봐야 할 듯 ← docker에서 ENV 명령어로 확인         
무조건 필요하진 않음         

<br>

### ARG 
: Dockerfile 내에서 사용할 수 있는 상수(아규먼트) 설정          
예시 : JAR_FILE=medical-adm.jar           

<br>

### LABEL 
: 보통 maintainer="***@gmail.com" 같은 형식으로 담당자 이메일 적음

<br>

## 명령어 비교
### cmd vs entrypoint
**cmd**            
override가 쉬움           
전체적인 애플리케이션 명령어           

**entrypoint**              
override가 가능하나 —entrypoint 명령어 필요            
특정 executable에 사용하면 좋음            

shell과 exec 형태 명령어를 둘 다 쓸 수 있지만 exec 형태 추천

<br>

### add vs copy
add는 os가 인식하는 압축 파일이면 바로 해제해 디렉토리로 생성한다.       
copy는 압축 파일 그대로 저장 (without the tar and remote URL handling)          
압축 해제가 목적이 아니라면 copy를 쓰고 아니면 add를 사용         

```
FROM tomcat:8-jre8

WORKDIR /usr/local/tomcat

ADD medical-adm.war /usr/local/tomcat/webapps/ROOT.war
# 프로젝트 안에 있을 때는 /target/을 추가해야 했음
# 왜냐면 war 파일이 target 안에 있으니까... duh

EXPOSE 8080
```

<br>

## Docker에 이미지 및 컨테이너 생성
### build
이미지 생성        
- t 로 태그 명 추가
- 뒤에 . 붙이는 거 중요! (현재 경로에서 진행하겠다는 의미)
- 여기서 FROM에 해당하는 이미지가 없다면 Docker가 알아서 해당 image를 pull해 온다.
- : 뒤 태그명은 안 붙여도 되지만 추후 버전 관리에는 중요하니 쓰는 방향으로

```
docker build -t medical-adm:latest .
```

<br>

### run
컨테이너 생성         
- d : 데몬 → 데몬을 돌려주는 걸로 실행시켜야 터미널 명령창을 닫아도 계속 실행을 시켜준다

```
docker run -d -p 8080:8080 --name medical-adm medical-adm:latest
```

<br>

### start
생성한 컨테이너 시작 & 종료

```
docker start medical-adm

docker stop medical-adm
```
[https://www.hanumoka.net/2019/01/21/springBoot-20190121-springboot-deploy-docker/](https://www.hanumoka.net/2019/01/21/springBoot-20190121-springboot-deploy-docker/)

<br>

## jar vs war
jar : Java 프로젝트를 압축한 파일로 jre만 있다면 실행이 가능        
war : 웹 어플리케이션 압축 파일로 파일을 실행하려면 tomcat 같은 was 환경 필요             

<br>

### war 파일
문제 : /usr/local/tomcat/webapps/ 아래 ROOT 파일이 존재해서 톰캣 메인 페이지가 먼저 보임
cmd로 먼저 삭제한 다음 ROOT.war를 지정해줘야 한다

[https://antdev.tistory.com/75](https://antdev.tistory.com/75)
[terminal 명령어](https://www.notion.so/terminal-6aec872b21214d1ba879a6db063623cd)

설치 후 tomcat 파일 안에 들어가기

```
docker exec -it medical-adm bash

docker exec -it medical-adm /bin/sh // bash가 없는 컨테이너

rm -rf ROOT // webapps에 들어가서 ROOT 지우기

docker run -t medical-adm2 // 새로운 이미지를 만들어줘야 shutdown 명령어 기동 시
// 컨테이너가 닫히지 않음

bin/shutdown.sh

bin/startup.sh
```

서버에 프로젝트를 올리기 위해서 Tomcat에 war 파일을 배포한다.           
ROOT.war로 이름을 설정해 톰캣 폴더 내 webapps에 넣어 bin/startup.sh를 실행            
[https://its-easy.tistory.com/4](https://its-easy.tistory.com/4)

<br>

**리눅스 환경 톰캣 내부 구조**

/usr/local/tomcat ←  톰캣 기본 루트

/bin : 실행 파일(Linux - .sh | Windows - .bat)

/conf : 설정 파일(server.xml)

/work :

/webapps : 기본 주소 + ROOT

.war 파일을 넣어두면 압축 해제

ROOT 파일이 있기 때문에 `ROOT.war 파일이 압축 해제가 안 되면서` localhost:8080을 주소창에 치면 추가한 애플리케이션이 아닌 톰캣 메인 페이지가 뜬다.

또한 localhost:8080이 안 먹을 때는

- 다른 애플리케이션이 같은 포트 번호를 사용하고 있진 않은지 확인
- ROOT 파일을 rm -rf로 지워서 ROOT.war이 압축 해제 되도록 설정

[https://sang12.co.kr/134/Docker-tomcat(톰캣)-war파일-배포하기](https://sang12.co.kr/134/Docker-tomcat(%ED%86%B0%EC%BA%A3)-war%ED%8C%8C%EC%9D%BC-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)

<br>

### jar 파일

```
FROM adoptopenjdk:11-jre

EXPOSE 8080

VOLUME /tmp

ADD medical-api.jar /root/medical-api.jar

ENTRYPOINT ["java", "-Dspring.profiles.active=docker", "-jar", "/root/medical-api.jar"]
```

<br>

### jar와 war 파일 Dockerfile 생성하며 문제점

1. path
- ADD에서 jar 파일이 생성되는 위치와 ENTRYPOINT에서 java -jar 명령어라 실행될 위치가 일치해야 했음
- [x]  경로에서 /root를 빼고 다른 경로

→ 다른 경로를 줘도 ADD와 ENTRYPOINT 경로가 같다면 성공

- v C:\Temp:/tmp 유무?
- [x]  -v로 지정 안함

→ -v 와는 관계 없음.

1. profile 명
- profile local은 로컬 환경용 설정이라 안에 내용물(datasource, jpa 등)이 아무 것도 없었음
- [x]  다른 프로파일(win)

→ 다른 프로파일을 넣어도 잘 돌아감

- [x]  active profile 설정 없을 때

→ active profile이 없는 경우 **컨테이너가 시작하고 바로 꺼짐**

참고로 temp(Windows), tmp(Linux)는 윈도우에서 임시적으로 저장하는 용도의 폴더이다.

```
curl
curl [<http://localhost:8080>](<http://localhost:8080/>)
curl [<http://localhost:8080/swagger-ui.html>](<http://localhost:8080/swagger-ui.html>)

doskey /history > c:/Developer/cmd.txt
```

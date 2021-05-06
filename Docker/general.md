## 정의
컨테이너라는 표준화된 소프트웨어 유닛을 관리하는 소프트웨어 플랫폼     
AWS에 의하면 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어에 필요한 모든 기능을 컨테이너에 포함            

<br>

## 사용 이유
- 편리성 : 명령어로 바로 애플리케이션을 생성하거나 삭제 가능
- 확장성 : 필요한 만큼 컨테이너를 생성하면 됨
- 종속성 : 환경에 구애 받지 않고 표준화된 환경 
- 안전성 : 버전 관리와 동일한 환경으로 충돌 방지

<br>

## 용어
**이미지**              
`컨테이너를 정의하는 읽기 전용 템플릿`                
컨테이너를 만드는데 필요한 모든 종속성 및 정보, 런타임 실행 구성 등을 담음.           
하나의 패키지로 만들어 어느 환경이든 안정적으로 배포되고 재현 되도록 도와준다.          
다른 배포 환경에서 종속성(동일한 환경)을 보장하기 위해 사용됨.            

<br>

**컨테이너**                 
도커 이미지의 인스턴스        

<br>

**Dockerfile**              
도커 이미지를 만들기 위한 가이드라인을 포함한 파일.             
빌드 명령어는 `docker build` 이다.

<br>

## 가상 머신과 도커 컨테이너 차이
가상 머신 : 서버 `하드웨어` 가상화            
도커 : 서버 `운영 체제` 가상화                  

가상 머신 : 자체 커널을 포함한 완전한 운영체제 구조              
- 호스트 운영체제와 완벽한 격리
- 완전한 운영체제 구조라 많은 리소스를 요함
- 가상 머신 내 운영체제만 가능
- 관리가 어려움

도커 : 호스트 운영체제 커널을 공유하는 구조                   
- 경량 버전이라 리소스 코스트가 적음
- 호스트 운영체제만 가능
- 버전업 시 Dockerfile 수정해서 이미지를 다시 빌드하면 끝

<br>

### 특징
- 기존의 도커 컨테이너는 리소스를 적게 쓰는 대신 기본 os를 공유하기에 격리성은 떨어졌음
- 하지만 Hyper-V 컨테이너부터 각 컨테이너마다 별도의 특별 VM을 할당에 이런 문제점 해결
- 최근에는 Hyper-V 다음으로 WSL2가 소개 되었음
- WSL2는 윈도우 운영체제에서도 리눅스 환경을 사용 가능
- 그 외 동적 메모리 할당으로 메모리 사용 기능을 개선, 도커 daemon 성능 개선으로 시작 시간이 단축

<br>

## WSL2
WSL2 (Windows Subsystem for Linux 2)           
리눅스 커널 구성을 다운로드하고 명령어를 수행하면 됨               
참고로 WSL을 사용하려면 `Windows Terminal`을 사용하는 게 좋다고 추천함               

```jsx
wsl -l -v // wsl 버전 확인
wsl --set-default-version 2 # 모든 Linux 배포판을 version 2를 기본 설정
```

[https://www.lesstif.com/software-architect/wsl-2-windows-subsystem-for-linux-2-89555812.html](https://www.lesstif.com/software-architect/wsl-2-windows-subsystem-for-linux-2-89555812.html)

<br>

## 기본 명령어

```java
docker login

docker search oracle-xe-11g

docker pull jaspeen/oracle-xe-11g

docker run --name oracle11g -d -p 1521:1521 jaspeen/oracle-xe-11g

docker restart oracle11g

docker exec -it oracle11g sqlplus

system / oracle

docker ps -a
```

<br>

## 나중에 다시 보면 좋을 사이트
[https://aws.amazon.com/ko/docker/](https://aws.amazon.com/ko/docker/)                 
[https://docs.microsoft.com/ko-kr/dotnet/architecture/containerized-lifecycle/what-is-docker](https://docs.microsoft.com/ko-kr/dotnet/architecture/containerized-lifecycle/what-is-docker)         
[https://docs.docker.com/docker-for-windows/wsl/](https://docs.docker.com/docker-for-windows/wsl/)        


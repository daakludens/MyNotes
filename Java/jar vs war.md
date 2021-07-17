# jar 파일 vs. war 파일
## jar 파일
Java ARchive의 약자로 배포를 위해 자바 파일, 메타 데이터, 리소스를 하나로 모은 파일 형태다.
한 번의 요청으로 애플리케이션 전체를 다운로드할 수 있고. 배포나 도커 이미지로 만들 때 패키징해 실행시킬 수 있다.

```
mvn clean // target 폴더 내 하위 공간을 비움(이전 메이븐 빌드의 모든 산출물 삭제)
mvn package // (실행 가능한 jar 파일 생성)
java -jar target/{name-of-jar-file}.jar // jar 파일 실행
```

<br>

## war 파일
Web application ARchive의 약자로 jar 파일, 서블릿, xml, 웹페이지 등 웹 애플리케이션에 배포될 파일 모음이다.

jar 파일과 다르게 war file은 `WEB-INF 디렉토리 안에 web.xml`을 가지고 있다. 
web.xml이 웹 어플리케이션의 구조를 정의하고 있는 파일이다. 
만약 JSP 페이지만 있다면 쓸 일은 없지만 서블릿을 사용한다면 어느 URL로 라우팅이 될 건지에 대한 정보가 필요하기 때문에 web.xml을 사용해야 한다.

MVC 구조에 가장 잘 맞는 배포 파일 형태이다.

<br>

## 그럼 왜 Spring Boot는 jar 파일을 사용하는가?
내장 톰캣은 spring-boot-starter-web 라이브러리에 들어있으며, 만약 다른 내장 was를 사용하려면 spring-boot-web-starter를 exclusion으로 톰캣을 제외시키면 된다.

또한 내장 톰캣이 있다는 건 하나의 애플리케이션 당 하나의 톰캣이 붙어 운영된다는 거다. 그러면 톰캣 자원이 나눠지지 않기 때문에 더 빠른 애플리케이션 운영이 가능하다.

결과적으로 스프링 부트의 경우 내장 톰캣으로 jar 실행 가능(원래대로라면 war 압축 파일 실행을 위해 웹 서버를 따로 수동적으로 설치해서 같이 올려야 한다)

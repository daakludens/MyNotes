### shell script

shell script에서 `$`는 실행 혹은 변수 값 참조 때 사용           
`$( )`는 command substitution으로 사용됨         
→ 커멘드 표현식을 $( )에 넣어서 문자열에 지정하면, $문자열로 커멘드를 대체할 수 있다. 

`#!/bin/sh`은 sh로 해당 스크립트 파일을 실행시킨다는 의미다. 여기서 다른 쉘을 사용하려면 다른 쉘 명을 넣으면 된다. ex #!/bin/bash

```jsx
#!/bin/sh

SHORT_PROC_HOME_DIR=/home/weather/ws/weather/proc
cd $SHORT_PROC_HOME_DIR

dt=$(date +"%Y%m%d")
tm=$(date +"%H")

echo $dt $tm

time /usr/bin/python3 short.py $dt $tm'00'
```

<br>

![image](https://user-images.githubusercontent.com/71559880/118369846-feacbd00-b5df-11eb-9a67-ccf343f2a6fc.png)

<br>

다운 받은 애플리케이션은 `/usr/bin`에 다운 받아 관리

```jsx
# 애플리케이션 설치
sudo apt-get install [애플리케이션 이름][ex. openjdk-11-jre]

# 환경변수 설정
vim ~/.bashrc
export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))
export PATH=$PATH:$JAVA_HOME/bin

# alternative로 애플리케이션 관리
# alternative는 여러 버전의 패키지를 관리하기 위해 사용
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-11-openjdk-amd64/bin/java 1

# 위치 찾기
which java

# 버전 확인
java --version
```

<br>

ssh를 이용해 각 서버로 들어가서 터미널 명령어를 넣는 것도 가능하다.

[https://codechacha.com/ko/ubuntu-install-open-jdk11/](https://codechacha.com/ko/ubuntu-install-open-jdk11/)

[https://blog.gaerae.com/2015/01/bash-hello-world.html](https://blog.gaerae.com/2015/01/bash-hello-world.html)

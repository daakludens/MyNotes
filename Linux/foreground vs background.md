# Foreground and Background
## 정의
프로세스(쓰레드)를 실행하는 방식으로 2개가 있다.

하나는 foreground로 콘솔창에 실행하며, 실행 중에는 다른 프로세스를 실행하지 못하고 끝날 때까지 대기해야 한다.

background는 말 그대로 뒤에서 프로세스를 실행하며, 실행 중에도 콘솔창에 다른 업무를 실행할 수 있다.

<br>

## 작업 상태를 변경하기
& 를 붙이면 background에서 실행 된다

만약 foreground에서 진행 중인 작업을 background에서 작업하도록 변경하고 싶으면 작업을 멈춘 다음에 옮길 수 있다.

```
jobs // 전체 작업, 아이디와 상태 확인

ctrl + Z // 현재 작업을 정지시킴

bg %{jobs-id} // 작업 아이디를 입력해 해당 작업을 background에서 실행

fg %{jobs-id} // foreground에서 실행하도록 설정


// google에서 제시하는 젠킨스 백그라운드 실행 방식(Jenkins ProcessTreeKiller)
BUILD_ID=dontKillMe
nohup bash relaunch.sh &

BUILD_ID=dontKillMe nohup bash relaunch.sh &
```

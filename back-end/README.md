# 백앤드 관련 기본 지식 (레드햇 계열)

### 디렉토리

```bash
    # 디렉토리 생성
    mkdir 폴더이름

    # 디렉토리 이동
    cd 경로

    # 파일 목록 출력(숨긴 파일 + 자세히)
    ls -al

    # 현재 경로 표시
    pwd

    # 파일, 디렉토리 삭제
    rm -rf

    # 파일 복사
    cp folder folder-copy

    # 파일 생성
    touch file.txt

    # 이름 변경, 다른 경로로 이동
    mv file.txt file2.txt
    mv file1 file2 file3 file4 # file1,file2,file3를 /file4로 이동
    mv file.txt 경로

```

### 링크 파일 만들기

```bash

ln -s (소프트 링크) - 바로가기 만들기

```

### 별칭 등록

```bash

    alias 키값="내용"

    # example
    alias rm='rm -i'
    alias k='kubectl'

    # shell 설정
    echo "alias k='kubectl'" >> ~/.bashrc
    source ~/.bashrc

```

### 환경 변수

```bash
  export KEY="VALUE";

  이렇게 작성하면 재부팅 하면 사라진다
  .bashrc라는 파일에 기록하면 된다
  바로 적용이 안되므로 재부팅하거 source .bashrc
```

#### 쉘 스크립트내에서

- 다른 쉘 스크립트의 환경변수를 사용하고 싶으면 source ./쉘 스크립트파일.sh로 선언하고 사용하면

### whoami

- 로그인한 아이디 확인

### 프로세스

```bash

    # 프로세스 조회
    ps -ef

    # 조회 후 pid를 이용하여 종료
    kill -9 pid

```

### 파이프 라인 ( '|' )

- 찾은 리스트를 가공할 때 사용
- 예) yum list | grep [패키지 이름] (grep : 포함된 것만 찾기)

#### 프로세스 + 파이프라인 조합해서 pid 조회, 삭제

```bash

ps -ef | grep tomcat8 | grep -v grep | awk '{print $1}'

#ps -ef | grep tomcat8 이란것도 프로세스에 기록 된다 그래서 -v 옵션으로 grep으로 검색된 것을 제외

#awk는 공백을 기준으로 리스트를 만들어 준다. 배열은 1번부터 시작이기 때문에 2번이 pid이다

sudo kill -9 `ps -ef | grep tomcat8 | grep -v grep | awk '{print $1}'`

# 백틱(`)으로 감싸줘야 한다.

#!!!! OR
pgrep -f 타깃

```

### 스프링 부트 프로젝트 강제 종료 시키기 (pgrep 사용)

```bash
# spring-stop.sh

echo "Sprinboot Stop......"

SPRING_PID=$(pgrep -f *.jar)

echo $SPRING_PID

kill -9 $SPRING_PID

```

### 서비스 목록 확인 (+ : 실행중 , - : 실행 중지)

- service --status-all

### systemctl

- service로 실행 안되는 것이 있으니 systemctl을 사용하자!
- 사용법 예시
  - sudo systemctl list-unit-files | grep tomcat8
  - sudo systemctl start tomcat8
  - sudo systemctl status tomcat8

### 파일 찾기

- sudo find / -name tomcat8

### root에 비밀번호 추가하기

- sudo passwd root

### 사용자 전환

- su - user1 또는 su root

### 파일 소유권

| -         | r           | w           | -           | r         | -         | -         | r                 | -                 | -                 |     | 1    |
| --------- | ----------- | ----------- | ----------- | --------- | --------- | --------- | ----------------- | ----------------- | ----------------- | --- | ---- |
| 파일 유형 | 소유자 읽기 | 소유자 쓰기 | 소유자 실행 | 그룹 읽기 | 그룹 쓰기 | 그룹 실행 | 그 외 사용자 읽기 | 그 외 사용자 쓰기 | 그 외 사용자 실행 |     | 링크 |

- sudo chmod u(소유자)+x,g(그룹)+wx,o(아무나)+x 파일명
  - +는 추가, = 재정의
  - 띄어쓰기 금지
- sudo chmod 775 파일명 방식으로 권한을 준다.
- 파일에 소유자와 그룹명 부여
  - sudo chown root:ubuntu test1.txt (root가 소유자 ubuntu가 그룹)

### 패키지 다운로드 및 파일 다운로드

- yum install wget (파일 경로로 다운로드 받기위해 설치)

- wget url
  - -c : 끊긴 파일 다시 다운받기

### 아마존 리눅스 한국 시간 관련 설정

- 현재 시간으로 설정된 나라 확인
  - timedatectl

1. 방법1

- sudo rm /etc/localtime
  - sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime

2. 방법2

- timedatectl list-timezones | grep Seoul
  - sudo timedatectl set-timezone Asia/Seoul

### 리눅스에서 url 호출

- curl "url 주소"

### 파일 끝 줄 보기

- sudo tail -f catalina.out
  - 기본 10줄에 -f 옵션으로 인해 실시간으로 계속 감시 가능
  - 파일로 남기고 싶으면?
    - 파일 생성, 권한 변경 후
    - sudo tail -f catalina.out > file.log

### 소프트 웨어 패키지

- 업데이트
  - sudo yum update -y
- 찾기
  - sudo yum search "java"
- jdk 11 설치
  - 아마존 리눅스는 sudo yum install java-11-amazon-corretto-headless

### nohup

- 리눅스에서 프로세스를 실행한 터미널의 세션이 끊겨도 백 그라운드로 계속 실행할 수 있게 해주는 것
- 예시
  - nohup java -jar \*.jar & (&는 백그라운드 실행)
- nohup.out이라는 파일이 생긴다
- tail -f(계속 구독) nohup.out
- 일반 로그와 에러 로그 분리
  - nohup java -jar spring-0.0.1-SNAPSHOT.jar 1>log.out 2>err.out &

### netstat (네트워크 연결상태 확인용)

- 예시
  - netstat -nlpt

### crontab

- 주기적 실행 명령어 (몇 분마다, 몇 시간 마다, 몇 일마다, 몇 시간마다, 몇 요일마다 실행하겟다)
- (\* \* \* \* \*) : 분(0~60) 시(0~23) 일(1~31) 월(1~12) 요일(0~7)
- 예시
  - crontab -e 입력
  - \* \* \* \* \* ls -l 1>>cron.log 내용 저장

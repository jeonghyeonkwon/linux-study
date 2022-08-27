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

### 프로세스

```bash

    # 프로세스 조회
    ps -f

    # 조회 후 pid를 이용하여 종료
    kill -9 pid

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

### 파이프 라인 ( '|' )

- 찾은 리스트를 가공할 때 사용
- 예) yum list | grep [패키지 이름]

### whoami

- 로그인한 아이디 확인

### 서비스 목록 확인 (+ : 실행중 , - : 실행 중지)

- service --status-all

### 파일 소유권

| -         | r           | w           | -           | r         | -         | -         | r                 | -                 | -                 |     | 1    |
| --------- | ----------- | ----------- | ----------- | --------- | --------- | --------- | ----------------- | ----------------- | ----------------- | --- | ---- |
| 파일 유형 | 소유자 읽기 | 소유자 쓰기 | 소유자 실행 | 그룹 읽기 | 그룹 쓰기 | 그룹 실행 | 그 외 사용자 읽기 | 그 외 사용자 쓰기 | 그 외 사용자 실행 |     | 링크 |

- chmod +x 파일명, chmod 775 파일명 방식으로 권한을 준다.

### 패키지 다운로드 및 파일 다운로드

- yum install wget (파일 경로로 다운로드 받기위해 설치)

- wget url
  - -c : 끊긴 파일 다시 다운받기

### 아마존 리눅스 한국 시간 관련 설정

1. sudo rm /etc/localtime

2. sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime

### 리눅스에서 url 호출

- curl "url 주소"

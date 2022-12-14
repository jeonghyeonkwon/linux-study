# 셸 스크립트 프로그래밍 정리

## 환경 변수

```

echo $환경 변수

printenv, export를 이용하여 환경 변수명 검색 가능

```

| 환경 변수 | 설명                           |
| --------- | ------------------------------ |
| HOME      | 현재 사용자의 홈 디렉토리 경로 |
| LANG      | 기본 지원되는 언어             |
| TERM      | 로그인 터미널 타입             |
| USER      | 현재 사용자의 이름             |
| PWD       | 사용자의 현재 작업 디렉토리    |
| PS1       | 1차 명령 프롬프트 정보         |
| BASH      | bash 셸의 경로                 |
| HISTFILE  | 히스토리 파일의 경로           |
| HOSTNAME  | 호스트의 이름                  |
| MAIL      | 메일을 보관하는 경로           |
| PATH      | 실행 파일을 찾는 경로          |

---

## 셸 스크립트 시작하기

- 확장자가 .sh 파일

```bash
#! /bin/bash

exit 0
```

이 .sh파일을 /bin/bash로 실행 시키겠다는 것

chmod +x 파일명 또는 chmod 755 파일명 둘 중 하나로 실행 가능 속성 추가

---

## 변수

1. 문자열

```bash
#! /bin/bash

str1=string
# =사이는 공백이 있으면 안됨

str2="string string"
# 띄어쓰기 있는 문자열은 묶어야 한다

str3=7+5
# 이렇게 쓰면 "7+5"라는 문자열이 된다

str4='$str3'

str5=\$str3
# str4, str5는 이렇게 써야 $가 출력된다.

```

2. 숫자 계산

```bash
#! /bin/bash

num=100 + 200
# + 사이의 띄어쓰기 해야됨

num2=`expr $num + 200`
# 변수가 들어있는 값과 숫자를 계산하려면 expr 사용해야됨

num3=`expr \( $num2 + 200 \) / 10 \* 2`
# 수식에 괄호를 사용할려면 역슬래쉬 사용해야 되고 곱하기만 특별히 백슬래쉬로 사용해야됨

```

3. 파라미터 변수

```bash
#! /bin/bash

echo "첫 번째 파라미터 $0"
echo "두 번째 파라미터 $1"
echo "전체 파라미터 $*"


# 이후 ./파일명.sh parameter1 parameter2
```

4. if ~ else

```bash
if [ 조건 ] # 조건 양 옆에 띄어쓰기 해야됨
then

    # 참일때

else

    # 거짓일때

fi  # 끝 맺음
```

- 문자열 비교 연산자

| 문자열 비교 | 결과                                 |
| ----------- | ------------------------------------ |
| -n "문자열" | 문자열이 NULL(빈 문자열)이 아니면 참 |
| -z "문자열" | 문자열이 NULL(빈 문자열)이면 참      |

- 파일 관련 조건

| 파일         | 설명                       |
| ------------ | -------------------------- |
| -d 파일 이름 | 파일이 폴더 인가?          |
| -e 파일 이름 | 파일이 존재하는가?         |
| -f 파일 이름 | 파일인가?                  |
| -r 파일 이름 | 파일이 읽기 기능이 있는가? |
| -s 파일 이름 | 파일크기가 0이 아니면 참   |
| -w 파일 이름 | 파일이 쓰기 가능한가       |
| -x 파일 이름 | 파일이 실행 가능한가?      |

5. case ~ esac문

```bash

case "$1" in

    c1) # 파라미터가 c1일때
        echo "case1";;  #세미콜론 2개 붙여야 됨

    c2)
        echo "case2"    # 실행구문이 더있으면 ;;찍지 않는다
        echo "case22";;
    *)
        echo "다른 언어의 default";;
esac #case문 끝맺음
```

6. for ~ in

```bash
for 타겟 in value1 value2 value3 ...
do
    내용
done

```

7. while

```bash

# 무한 루프

while [ 1 ]
do
    실행문
done

# or

while [ : ]
do
    실행문

    echo "실행 됨"


done

# continue break exit return 조합하여 사용
```

8. 함수

```bash
func(){
    echo "$1 $2"
}

func string1 string2
```

9. eval

```bash
string = "mkdir folder"

eval string

# 명령문으로 실행

```

10. export

```bash

file1.sh

export string="노출 String"

```

```bash

file2.sh

echo $string

```

11. set, $(명령어)

```bash
string = $(date) # 오늘 날짜 받기

set $(date) # 파라미터로 받기
```

12. shift

- 파라미터 변수를 왼쪽으로 한 단계식 이동

```bash

param = "1 2 3 4 5 6"
shift

param = "2 3 4 5 6"

shift
param = "3 4 5 6"

shift
param = "4 5 6"

# 파라미터 순서에 값이 저렇게 된다는 뜻
# $1 = 1 => 2 => 3 => 4 로 된다는 뜻

```

13. `$변수, $(명령어) 차이`

- $변수 = 변수의 값 또는 실행
- $(명령어) = 명령어의 리턴 값

### 실습용 배포 코드

```bash
#! /bin/bash


# 1. env variable
source ./var.sh

echo "1. env variable setting complete"

# 2. clone delete
touch crontab_delete
crontab crontab_delete
rm crontab_delete
echo "2. cron delete complete"

# 3. server checking

if [ -n "${PROJECT_PID}" ]; then # pid가 존재한다면
	# re deploy
	kill -9 $PROJECT_PID
	echo "3. project kill complete"
else
	# first deploy

	# 3-1  update
	sudo yum -y update 1>/dev/null

	# 3-2 jdk install
	sudo yum install -y java-11-amazon-corretto-headless 1>/dev/null
	echo "3-2 jdk install complete"

	# 3-3 timezone
	sudo timedatectl set-timezone Asia/Seoul
	echo "3. project install and timezone setting complete"
fi

# 4. project folder delete
rm -rf ${HOME}/${PROJECT_NAME}
echo "4. project folder delete complete"

# 5. git clone
git clone https://github.com/${GITHUB_ID}/${PROJECT_NAME}.git
sleep 3s
echo "5. git clone complete"

# 6. gradlew +x
chmod u+x ${HOME}/${PROJECT_NAME}/gradlew
echo "6. gradlew u+x complete"

# 7. build
cd ${HOME}/${PROJECT_NAME}
./gradlew build


# [강좌 - 개발자를 위한 AWS DevOps 입문 [CI/CD 무중단 배포]](https://easyupclass.e-itwill.com/course/course_view.jsp?id=74&rtype=0&ch=course)

```

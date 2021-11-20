# getopt
* 셸의 쉬운 구문 분석을 위해 명령줄에서 옵션을 쉽게 분리하는데 사용함.
### 사용법
1) short 옵션은 `-o`로 사용.
  * 해당 옵션에 추가 인수가 필요하다면 `콜론(:)`을 추가해 사용.
  * `ex) "-o a:b:c"`: a, b 옵션은 추가 인수 필요.
2) long 옵션은 `-l`로 사용.
  * short 옵션과 달리 long 옵션을 구분하려면 `콤마(,)`로 구분.
  * 해당 옵션에 추가 인수가 필요하다면 `콜론(:)`을 추가해 사용.
  * `ex) "-l apple, banana, carret:"`: carret 옵션은 추가 인수 필요.
3) `"-n <이름>"` 옵션으로 getopt가 오류를 출력할 때 사용할 이름을 지정.
4) 마지막에 `-- "$@"`를 삽입
  * `"--"`는 뒤에 오는 인수를 해당 명령의 옵션으로 처리하지 말라는 키워드로 `"$@"` 값이 getopt의 옵션으로 소비되는것을 막기 위함.
  * `"$@"`는 스크립트를 실행할 때 입력한 모든 인수가 담겨있고 파싱할 대상으로 지정.

### 사용예제
```shell script
$ nano getopt.sh

#!/bin/bash

set -- $(getopt -o a:b:c -- "$@")

while [ -n "$1" ]
do
        case "$1" in
                -a)
                        arg="$2"
                        echo "-a option, with argument value $arg"
                        shift;;
                -b)
                        arg="$2"
                        echo "-b option, with argument value $arg"
                        shift;;
                -c)
                        echo "-c option";;
        esac
        shift
done

$ ./getopt.sh -a apple -b banana -c
```
![getopt](https://user-images.githubusercontent.com/68629440/142728993-7b8327d8-6f78-47a0-969f-825a1f3ba487.png)

# getopts


# sed
* 텍스트 데이터를 패턴 매칭하여 처리, 표준 입력이나 파일에서 텍스트를 입력받아 데이터를 처리함.
* vi에디터와 비슷함. 단, 파일을 열어 직접 수정하는 vi에디터와는 달리 커맨드 창, 스크립트에서 동작함.
### 사용법
`$ sed [옵션] [명령어] <파일명>`
|옵션|내용|
|---|---|
|-f|처리할 명령을 저장한 파일을 지정|
|-i|원본 파일에 덮어 씌워 저장|
|-n|특정 값이 들어간 줄만 출력(주로 p명령어와 함께 사용)|

|명령어|내용|
|---|---|
|a|해당 값이 있는 다음 줄에 입력|
|d|삭제|
|g|한 줄에 해당하는 값이 여러개 있을 경우 모두 변경|
|p|출력|
|s|치환|

### 사용예제
---
#### 파일 내용 변경
```shell script
$ echo -e "one\ntwo\nthree\nfour" > test_s.txt
$ cat test_s.txt
one
two
three
four

# e를 1로 변경(리눅스 명령어 tr과 유사)
$ sed 's/e/1/' test_s.txt
on1
two
thr1e
four

# g 명령어와 함께 사용하면?
$ sed 's/e/1/g' test_s.txt
on1
two
thr11
four

# 변경 후 저장
$ sed -i 's/e/1/g' test_s.txt
$ cat test_s.txt
on1
two
thr11
four
```
#### 파일 내용 삽입
```shell script
# 마지막 라인 삽입
$ sed '$a five' test_s.txt
on1
two
thr11
four
five
```
#### 파일 내용 삭제
```shell script
# 3번째 라인 삭제
$ sed '3d' test_s.txt
on1
two
four
```
#### 파일 내용 치환
```shell script
# four를 4로 치환
$ sed 's/four/4/' test_s.txt
on1
two
thr11
4
```
# awk
* 입력을 주어진 분리자(field seperator)로 분리하여 명령을 처리함.
### 사용법
` awk [패턴] {[액션(함수)]} <파일명> `
|옵션|내용|
|---|---|
|-F|문자열을 분리할 기준이 되는 분리문자 입력|
|-v|파라미터 전달|

|함수|내용|
|---|---|
|sub|지정한 문자열 치환|
|gsub|문자열 일괄 치환|
|index|주어진 문자열과 일치하는 문자의 인덱스를 반환|
|length|문자열의 길이를 반환|
|substr|시작위치에서 주어진 길이 만큼의 문자열 반환|
|split|문자열을 분리하여 배열로 반환|
|print|문자열 출력|
|printf|지정한 포맷에 따라 함수 출력|
|system|명령 실행|

### 사용예제
---
#### 합 구하기
```shell script
$ cat test_a.txt
a 1 kim
b 2 shin
c 3 go
c 4 baek
b 5 yoo

% $1 각 그룹의 합 구하기
$ awk '{arr[$1] += $2} END {for (i in arr) {print i, arr[i]}}' test_a.txt
a 1
b 7
c 7

% $2의 모든 합 구하기
$ awk '{sum += $2} END {print sum}' test_a.txt
15
```

#### 문자열 출력
```shell script
% kim 문자가 들어간 문자열 출력
$ awk '/kim/' test_a.txt
a 1 kim
```

#### 문자열 추가
```shell script
% 성 앞에 소속 대학 문자열 출력
$ awk '{print ($1 " " $2 " " "chosun univ. " $3)}' test_a.txt
a 1 chosun univ. kim
b 2 chosun univ. shin
c 3 chosun univ. go
c 4 chosun univ. baek
b 5 chosun univ. yoo
```

#### 문자열 자르기
```shell script
% 함수 substr을 사용해 문자열 자르기
$ echo "1234567890" | awk -F " " '{print substr($1, 4, 3)}'
456
```

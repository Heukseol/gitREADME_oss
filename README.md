# getopt


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
# four를 4로 
$ sed 's/four/4/' test_s.txt
on1
two
thr11
4
```
# awk
* 입력을 주어진 분리자(field seperator)로 분리하여 명령을 처리함.
### 사용법
`awk [옵션] [
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
a 1
b 2
c 3
c 2
b 3

$ cat test_a.txt | 

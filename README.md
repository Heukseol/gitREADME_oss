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
```
$ echo -e "one\ntwo\nthree\nfour" > test.txt
$ cat test.txt
one
two
three
four

# e를 1로 변경(리눅스 명령어 tr과 유사)
$ sed 's/e/1/' test.txt
on1
two
thr1e
four

# g 명령어와 함께 사용하면?
$ sed 's/e/1/g' test.txt
on1
two
thr11
four

# 변경 후 저장
$ sed -i 's/e/1/g' test.txt
$ cat test.txt
on1
two
thr11
four
```
#### 파일 내용 삽입
```
# 마지막 라인 삽입
$ sed '$a five' test.txt
on1
two
thr11
four
five
```
#### 파일 내용 삭제
```
# 3번째 라인 삭제
$ sed '3d' test.txt
on1
two
four
```
#### 파일 내용 치환
```
$ sed 's/four/4/' test.txt
on1
two
thr11
4
```
# awk

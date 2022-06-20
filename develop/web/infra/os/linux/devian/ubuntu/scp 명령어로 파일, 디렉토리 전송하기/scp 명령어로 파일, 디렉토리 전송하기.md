# 개요

- scp 명령어를 이용해 로컬과 원격지간에 파일 혹은 디렉토리를 전송할 수 있다.

# 명령어

> scp [option] [from] [to]

> Remote는 [계정@아이피:파일경로], Local은 [파일경로]로 작성

# 옵션

- `-P`: 포트번호
- `-i`: 인증키 이용한 접속시 인증키 경로
- `-r`: 디렉토리를 전송할시
- `-p`: 파일의 수정시간 및 권한 유지

# 예

## Remote -> Local

- `scp -P 22 user@a.b.c.d:/memo.txt ~/Download/memo.txt`

## Local -> Remote

- `scp -i "~/.ssh/id_rsa" -P 22 ~/Download/memo.txt user@a.b.c.d:/memo.txt`  

## Remote -> Remote

- `scp -P 22 -p from-user@a.b.c.d:/memo.txt to-user@a.b.c.d:~/Download/memo.txt`

## 여러파일 전송

> 여러파일 전송시에는 여러 파일 목록을 quote로 묶어준다

- L->R: `scp -i "key" "/a.txt /b.txt" to-user@a.b.c.d:"~/Download/a.txt ~/Download/b.txt"`
- R->L: `scp -P 22 from-user@a.b.c.d:"/a.txt /b.txt" "~/Download/a.txt ~/Download/b.txt"`
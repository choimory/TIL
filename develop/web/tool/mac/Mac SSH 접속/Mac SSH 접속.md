# 개요

- 맥은 리눅스기반이라 기본 터미널로 SSH 접속을 선호

# 명령어

```shell
ssh [아이디]@[아이피] -p [포트번호]
```

- 종료시 `exit`

# ssh key를 이용한 접속

## ssh key 생성

```shell
ssh-keygen [원하는 암호화 방식]
```

- 명령어 비활성화시 openssl, openssh 설치가 필요하다. (맥은 보통 기본 설치되어 있음)
- 이후 파일명과 passphrase(키파일 비밀번호) 입력
  - passphrase를 미입력 엔터시, 비밀번호 없는 인증키로 생성됨 
- 암호화방식 미입력시 기본 rsa 방식으로 진행됨
- 파일.pub(공개키)는 서버에 제출하고, 확장자없는 파일(프라이빗 키)는 본인이 서버에 제출하는 용도로 사용한다.

````shell
chmod 600 [프라이빗키파일]
````

- 생성된 키파일의 권한을 본인만 사용할 수 있도록 변경해주어야 한다. 400, 600, 440 정도.

## ssh key로 접속

```shell
ssh -i [키파일] [아이디]@[아이피] -p [포트번호]
```

- i 옵션에 키파일을 지정하면 해당 ssh 키파일로 접속을 시도한다
- pem 파일도 가능

## ssh config 기본 키 설정

> ssh 접속이 필요할때마다 키파일을 지정하는게 번거로울시 시스템 기본 키파일을 지정할 수 있다.

```shell
mv [프라이빗키파일] [~/.ssh/id_rsa]
mv [공개키파일] [~/.ssh/id_rsa.pub] 
```

- ~/.ssh의 id_rsa.pub, id_rsa가 시스템 기본 공개키, 프라이빗 키 파일이다.
- 해당 파일 세팅시 -i 옵션 없이도, 시스템 기본 키로 접속이 진행된다.

## ssh config 파일 생성 및 설정

> 서버별로 다른 키파일을 제공해야 할시, 혹은 ssh 명령어를 alias로 단축하여 편히 사용하고 싶을시 ssh config를 생성하여 사용할 수 있다.

```shell
# config 파일 생성
vi ~/.ssh/config

# config 파일 작성
Host alias명
  HostName 아이피
  Port 포트번호
  User 아이디
  IdentityFile 프라이빗키파일
  
Host alias명2
  HostName 아이피
  Port 포트번호
  User 아이디
  IdentityFile pem파일
  
...

# config 파일 권한 변경
chmod ~/.ssh/config 600 
```

1. ~/.ssh 내에 config 파일 생성
2. config 파일에 접속정보 작성
3. config 파일을 본인만 읽을 수 있도록 400, 600 등으로 변경

- 단 password를 이용한 접속에는 사용할 수 없고, ssh 키를 이용한 방식에만 사용할 수 있음.
  - .pem 파일, .key 파일로도 가능하다
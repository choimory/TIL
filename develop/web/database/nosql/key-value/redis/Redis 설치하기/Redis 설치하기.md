# docker image pull

```docker
# docker hub에서 이미지 검색
docker search redis

# 이미지 pull
# alpine은 redis의 lts 버전이다
docker pull redis:alpine
```

# docker network create

```docker
docker network create redis-network
```

- redis server와 redis cli간에 통신하려면 두 컨테이너를 별도의 네트워크에서 관리해야 된다
    - bridge에서 해도 될 줄 알았는데 안되더라...
    - 그리고 사실 bridge에 다 때려박는것보단 별도의 네트워크로 구분하여 관리하는게 훨씬 권장되는 방식이기도 하다

# docker run (server)

```docker
docker run -d -p 6379:6379 -v /Users/choimory/choimory_workspace/mount/redis/choimory/data:/data -v /Users/choimory/choimory_workspace/mount/redis/choimory:/usr/local/etc/redis/redis.cnf --restart unless-stopped --network redis-network --name redis_choimory redis:alpine redis-server --appendonly yes --requirepass "asdqwe123"
```

- - p : 레디스의 기본포트
- -v 저장할경로:/data : redis의 데이터를 외부에 저장하고 싶을시
- -v 저장할경로:/usr/local/etc/redis/redis.cnf : redis의 설정을 외부에 저장하고 싶을시
- —restart unless-stopped (always) : 도커 컨테이너 재시작여부. 도커의 옵션 명령어
- —name [생성할 컨테이너명] [실행할 이미지명]: 컨테이너명을 어떤것으로 할지, 어떤 이미지를 실행할지
- redis-server : redis는 redis-cli와 redis-server가 존재하는데 그 중 server를 실행한다는 이야기
    - server는 db, cli는 쿼리 날리는 용도로 쓰임
- —appendonly yes : redis 데이터 저장을 AOF 방식으로 저장하겠다는 설정. 도커 명령어가 아닌 redis의 환경변수로 넘겨주는 옵션이므로 맨 뒤에 작성해준다.
    - AOF 관련 참고문서 - [http://www.redisgate.com/redis/configuration/persistence.php](http://www.redisgate.com/redis/configuration/persistence.php)
- —requirepass [비밀번호] : redis 비밀번호 설정. 도커 명령어가 아닌 redis의 환경변수로 넘겨주는 옵션이므로 맨 뒤에 작성해준다

# docker run (cli)

- 이제 cli를 실행하여 redis server가 잘 올라갔는지 확인해본다

```docker
docker run -it --rm --network redis-network redis:alpine redis-cli -h redis_choimory
```

- 먼저 docker run으로 redis-cli를 실행한다
    - —rm : 이미 컨테이너가 존재할 시 지우고 실행
    - -h : redis server 컨테이너명 지정

```docker
# redis-server에 비밀번호가 존재할 시 먼저 비밀번호를 입력해주어야 함
AUTH [비밀번호]

# 간단한 명령어 실행
keys *
```

# 참고

- [https://velog.io/@stella6767/Redis-설치M1-Docker](https://velog.io/@stella6767/Redis-%EC%84%A4%EC%B9%98M1-Docker) - 기본적인 설치방법
- [https://gofnrk.tistory.com/84](https://gofnrk.tistory.com/84) - redis 비밀번호 설정법
- [https://github.com/docker-library/redis/issues/176](https://github.com/docker-library/redis/issues/176) - appendonly, requirepass는 redis의 args 이므로 도커 명령어 뒤에 넣어야 한다
- [https://minholee93.tistory.com/entry/ERROR-NOAUTH-Authentication-required](https://minholee93.tistory.com/entry/ERROR-NOAUTH-Authentication-required) - redis cli 비밀번호 입력법
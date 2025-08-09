# 개요

- Docker로 Mariadb 이미지를 Pull하여, 구동하는 법을 알아보자.

# Docker pull

```shell
# 이미지 검색
docker search mariadb

# 이미지 Pull
docker pull mariadb:10.6
```

# Docker run

```shell
# 먼저 마운트에 진행할 mariadb 설정 파일을 마련하기 위해, 임시 mariadb를 구동 시킨 후, 설정 파일을 꺼낸다.

docker run -d -p 3307:3306 -e "MYSQL_ROOT_PASSWORD=asdfasdf" --name tmp-mariadb mariadb:10.6
docker cp tmp-mariadb:/etc/mysql my-mariadb/config
```  

```shell
# 실제 진행

docker run \
-d \
-p [host-port]:[container-port] \
-e "MYSQL_ROOT_PASSWORD=your_password" \
-e "TZ=Asia/Seoul" \
-v "/etc/localtime:/etc/localtime:ro" \
-v "[host-directory]:/var/lib/mysql:rw" \
-v "[host-directory]:/etc/mysql:rw" \
-v "[host-directory]:/var/log/mysql:rw" \
--ip [specific-ip] \
--restart="always" \
--network [your-docker-network] \
--name [container-name] [image-name:version]
```

- `d`: 데몬으로 실행
- `p`: Host의 요청포트:컨테이너의 요청포트 설정
- `e`: 해당 컨테이너 어플리케이션에 넘길 환경변수
    - 해당 예시에선 Mariadb root 비밀번호, DB 타임존 설정.
- `v`: 도커 마운트 (`호스트경로`:`컨테이너경로`)
    - 첫번째 마운트: DB 컨테이너의 시간을 Host 시스템의 시간으로 적용시키도록 마운트.
        - `:ro`: read-only. 읽기전용.
    - 두번째 마운트: 컨테이너의 Mariadb 데이터를 호스트쪽과 마운트
        - 해당 내용을 적용안할시 데이터가 컨테이너 내에만 적재되고, 컨테이너를 삭제할 시 데이터도 함께 간다.
        - `:rw`: read write. 읽기쓰기.
    - 세번째 마운트: 컨테이너의 Mariadb 설정을 호스트쪽과 마운트
        - 위와 동일한 이유도 설정파일도 호스트쪽으로 꺼낸다.
        - 가장 중요한 부분인데, mariadb 설정 파일은 호스트쪽에 기존 설정 파일이 없을시 마운트가 되지 않는것 같다.
            - 때문에 먼저 임의의 mariadb를 간단하게 하나 띄운 다음에, 기존 설정 디렉토리(/etc/mysql)를 docker cp로 호스트쪽으로 꺼내와서 설정 파일을 구축해놓고 마운트를 걸어주는 식으로 진행했다.
                ```shell
                docker run -d -p 3307:3306 -e "MYSQL_ROOT_PASSWORD=asdfasdf" --name tmp-mariadb mariadb:10.6
                docker cp tmp-mariadb:/etc/mysql my-mariadb/config
                ```  
    - 네번째 마운트: 컨테이너의 Mariadb 로그를 호스트쪽과 마운트
        - 위와 동일한 이유로 로그파일도 호스트쪽으로 꺼낸다.
- `--ip`: 고정 IP 할당을 원할시 사용
    - 사용하려면 도커 네트워크의 subnet을 확인하자.
    - docker network 생성시 gateway와 subnet ip도 고정할당이 가능하다.
        - ex. `docker network create --subnet 172.18.0.0/16 --gateway 172.18.0.1`, 이후 컨테이너 구동시 `--ip 172.18.0.100`
- `--restart`: 호스트 시스템이나 도커 시스템이 재시작될때 도커 컨테이너를 자동으로 재시작한다.
    - `--restart unless-stopped`으로 대체하여 사용 가능.
- `--network`: 해당 컨테이너를 편입시킬 도커 네트워크망을 지정한다.
- `--name`: 컨테이너 네임 지정.

# DB 튜닝

````shell
# [mysqld] 또는 [mariadbd] 밑에 옵션을 넣어줘야 제대로 동작

# Buffer Setting
innodb_buffer_pool_size         = 8G
innodb_write_io_threads         = 8
innodb_read_io_threads          = 8**
# Query Cache
query_cache_limit               = 16M
query_cache_size                = 512M
query_cache_type                = 1
````

- buffer pool size는 시스템 램의 80% 정도로 부여
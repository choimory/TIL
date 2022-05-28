# 개요

- 도커 컨테이너를 이용한 DB 활용시 데이터가 자꾸 깨지면서 컨테이너가 죽는 문제가 생겨서, 당분간 Homebrew로 운영체제 내에 직접 설치하여 사용하기로 했다

# 설치

```shell
# 설치
brew install mariadb

# 특정버전을 설치하고 싶다면
brew install mariadb@10.6
```

# 링크

```shell

brew link mariadb@10.6

# 충돌 오류시
brew link --overwrite mariadb@10.6

# 충돌파일까지 제거하고 싶을 시
brew link --overwrite --dry-run mariadb@10.6 

```

- 특정 버전의 DB를 설치했을시 심볼링 링크를 걸어주어야 한다

# 서비스 가동

```shell

brew services start mariadb

```

- brew services의 start를 사용시 매번 자동으로 시작하게 해준다

# cnf 위치

```shell

# x86 mac
vi /usr/local/etc/my.cnf

# M1 mac
vi /opt/homebrew/etc/my.cnf

```

- 인텔 맥, M1 맥인지에 따라 홈브루의 설정파일 로케이션이 달라지므로 확인하여 my.cnf 진입하여 수정한다

# Fine tuning

```shell

# [mysqld] 또는 [mariadbd] 밑에 옵션을 넣어줘야 제대로 동작

# Buffer Setting
innodb_buffer_pool_size         = 8G
innodb_write_io_threads         = 8
innodb_read_io_threads          = 8**
# Query Cache
query_cache_limit               = 16M
query_cache_size                = 512M
query_cache_type                = 1

```

- 더 많은 튜닝 참고: https://happist.com/577204/db-%ED%8A%9C%EB%8B%9D%EC%9C%BC%EB%A1%9C-mysql-%EC%B5%9C%EC%A0%81%ED%99%94
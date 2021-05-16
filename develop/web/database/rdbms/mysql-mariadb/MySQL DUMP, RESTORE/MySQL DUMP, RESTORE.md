# DUMP

## 전체 DUMP

- mysql 혹은 mysqldump -u[계정명] -p [DB명] > [파일명].sql
    - mysql -uroot -p targetdb > filename.sql

## DDL만 DUMP

- 명령어에 `-d` 혹은 `--no-data` 옵션 추가

## DML만 DUMP

- 명령어에 `-t` 혹은 `--no-create-info` 옵션 추가

## 특정 값만 DUMP

## 버퍼 사용없이 바로 표준출력으로 dump

- 명령어에 `-q` 혹은 `--quick` 옵션 추가
- DB 사이즈가 큰 경우 사용

## 특정 호스트의 MySQL에서 덤프

- 명령어에 `-h` 혹은 `--host=host` 옵션 추가

# RESTORE

- mysql 혹은 mysqldump -u[계정명] -p [DB명] < [파일명].sql
    - mysql -uroot -p targetdb < dumpfile.sql
    - [DB명]이 이미 존재시 기존 DB를 제거하고 다시 생성하므로 기존 데이터 유실됨에 유의.

# 참고

- [https://www.lesstif.com/dbms/mysqldump-db-backup-load-17105804.html](https://www.lesstif.com/dbms/mysqldump-db-backup-load-17105804.html)
- [https://bluebreeze.co.kr/462](https://bluebreeze.co.kr/462)
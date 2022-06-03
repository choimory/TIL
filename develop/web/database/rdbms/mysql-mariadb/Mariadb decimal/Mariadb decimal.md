# 개요

- MariaDB decimal 타입에 대해 작성

# 타입

- decimal(전체 자리수(M), 소수점자리수(D))
  - 전체 자리수(M)은 최대 65, D는 최대 30
  - 이때 전체 자리수는 소수점 포함한 자리수라는것에 유의하여야 함
    - e.g: decimal(5,2) = MMM.DD
- decimal에 자리수를 설정하지 않고 기본 decimal로만 설정할시 decimal(8,0)으로 설정됨

# zerofill

- zerofill 옵션 부여시 빈자리를 0으로 채워줌 (정수 포함)
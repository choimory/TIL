# 개요

- 파일경로를 작성할때 윈도우는 백슬래시, 리눅스는 슬래시를 사용한다.
    - 윈도우는 이스케이프도 필요해서 백슬래시 두번을 사용함 (`E:\\first\\second\\...`)
    - 리눅스 :  (`/first/second/...`)

# 사용법

- `File.separator` 를 사용한다

## 예

```java
String path = firstPath + File.separator + secondPath + File.separator...
```
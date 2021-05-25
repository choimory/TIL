# 빌드 오류 대처

## Java 관련 오류시

- build path → libraries → jdk 버전 확인
- build path → libraries → tomcat 버전 확인
- build path → libraires → 빠진 libraries 없는지 확인 (web app libraries..)
- build path → source → 패키지 구조 확인

## 라이브러리 관련 오류시

- Maven dependencies에 해당 라이브러리 다운로드 여부 확인
- buildpath → libraries에서 라이브러리 오류 여부 확인
- 프로젝트내에 별도 폴더로 라이브러리 관리 여부 확인
- C:\Users\사용자\.m2 내에 워크스페이스에서 참조중인 maven repository로 들어가 라이브러리 존재 여부 확인
    - 사용중인 워크스페이스에서 참조중인 maven repository 확인 및 변경여부는 window→preferences→maven→user settings 에서 가능
    - C:\Users\사용자\.m2\settings.xml에서도 확인 및 변경 가능
- pom.xml에 라이브러리 의존도가 작성되어 있는데 maven repository에 라이브러리가 존재하지 않을시, 해당 라이브러리가 시간이 지나면서 maven으로 받는게 불가능해진 상황. 라이브러리 파일을 따로 구해 maven repository에 물리적으로 내려놓던가 buildpath에서 라이브러리를 지정하여 등록해주어야함

## 스트럿츠 체크아웃시 대처

- 해당 프로젝트의 project facets를 인수자에게 직접 확인하여 project facets 설정

# 구동 실패 대처

## DB 연결 확인

## 톰캣 설정 확인
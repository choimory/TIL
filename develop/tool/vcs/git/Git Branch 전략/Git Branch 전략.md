# 주요 브랜치

## master (main)

- 운영 브랜치
- 배포될 코드만 merge하는 브랜치

## develop

- 개발 브랜치
- 개발된 코드들을 하나로 merge하는 브랜치

## feature/*name*

- 기능별 개발 브랜치
- 네이밍은 feature/기능명 (feature/login, feature/board...)
- develop에서 브랜치를 따서 작업
- 브랜치를 기능 단위로 생성함
- 기능 구현 완료 후 develop 브랜치에 Merge 후 삭제

## release-*version*

- 배포 준비를 위한 브랜치
- 네이밍은 release-버전명 (release-1.13...)
- develop → master로 바로 배포할 수 도 있지만, develop→release→master로 배포할 수 도 있음
    - 이때 release는 master와 develop에 모두 merge함

## hotfix-*version*

- 배포 버그 긴급수정 브랜치
- 네이밍은 hotfix-버전명 (hotfix-1.13...)
- master에 배포하였는데 문제가 생겼을시, master에서 hotfix 브랜치를 따서 작업
- 완료 후 master에 바로 merge한다
- 모든 브랜치 중, hotfix 브랜치만 master에서 계속계속 분기할 수 있다.

# 참고

[https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html)
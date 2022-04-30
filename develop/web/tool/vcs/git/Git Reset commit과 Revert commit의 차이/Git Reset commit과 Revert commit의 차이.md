# 개요

- Git Reset commit과 Revert commit의 차이를 알아봄

# Reset commit

- 로컬 브랜치에만 커밋하고 원격 브랜치에 푸쉬하지 않은 경우에는, Reset commit으로 커밋을 되 돌릴 수 있다.
- 이때 커밋을 되돌리면서 커밋 내역 코드롤 남겨 놓을지 여부를 soft, hart 등으로 정할 수 있다.

# Revert commit

- 로컬 브랜치 뿐만 아니라 원격 브랜치까지 커밋했을 경우에는 Reset commit으로 돌려놓는것이 불가능하다.
- 이때는 Revert commit을 사용한다. Revert commit은 취소하고 싶은 커밋의 이전 소스코드를 다시 푸시하여 코드를 원래 상태로 돌려놓는것.
- 예를 들어 `int a = 0;` 을 `int a = 1;`로 푸시하였는데 이를 되돌리고 싶은 경우, `int a = 0;`의 코드를 재차 푸시하여 원복하는것이다.
  - 해당 커밋의 revert commit을 선택시 이때 푸시되어 바뀐 모든 코드들을 푸시 전 내역으로 돌려놓음. 
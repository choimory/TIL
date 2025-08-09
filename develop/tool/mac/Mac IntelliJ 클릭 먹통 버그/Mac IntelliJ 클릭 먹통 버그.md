# 요약

- Mac의 악센트 관련 기능이 발생할시 생기는 버그
- Caps Lock으로 입력언어를 변환시 해결되지만 계속 이럴순 없으니 악센트 기능 해제 진행

# 작업

> terminal 켜고 해당 명령어 입력

- OS 전체 적용
    - `defaults write -g ApplePressAndHoldEnabled -bool false`
- IntelliJ 만 적용
    - `defaults write com.jetbrains.intellij ApplePressAndHoldEnabled -bool false`

# 출처

[https://androphil.tistory.com/759?category=423961](https://androphil.tistory.com/759?category=423961)
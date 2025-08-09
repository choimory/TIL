# nvm이란?

- node version manager
- node를 관리해주는 매니저
- 원하는 버전의 노드를 설치하고 변경해가며 사용할 수 있다

# node를 직접 설치하지 말자

- brew install node@version 할시 많은 노드들을 모두 설치해야 하고, brew로 무엇을 사용할시 계속 바꿔주어야 한다
- nvm을 사용할시 nvm을 설치하는것으로 사용할 node의 설치, 변경등을 자유롭게 진행할 수 있다

# nvm 설치

```shell
brew install nvm
```

- 우선 homebrew로 nvm을 설치해준다

```shell
mkdir ~/.nvm
```

- 홈에 .nvm 디렉토리를 추가해준 뒤

```shell
#zsh
vi ~/.zshrc 

#bash
vi ~/.bash_profile 
```

- 로 터미널 설정파일을 열고

```shell
# zsh
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh

# bash
export NVM_DIR="$HOME/.nvm"
source $(brew --prefix nvm)/nvm.sh
```

- (디렉토리는 M1 기준)
- 위의 내용을 환경변수로 추가로 등록해준다

```shell
#zsh
source ~/.zshrc 

#bash
source ~/.bash_profile
```

- 로 터미널에 반영해준다

# nvm 설치 확인

````shell
nvm -v
````

# nvm을 이용한 node 관련 명령어

- `nvm install {version}`: 해당 버전의 노드 설치
- `nvm use {version}`: 해당 버전의 노드로 버전 변경
- `nvm current`: nvm으로 사용중인 현재 노드 버전
- `nvm ls node`: nvm로 설치되어 관리되는 노드 목록들
- `nvm alias default {version}`: 해당 노드를 글로벌로 적용

# 참고

- https://lynmp.com/ko/article/tb585d114096490055
- https://hohoya33.tistory.com/212
- https://velog.io/@syj9760/TIL-%ED%84%B0%EB%AF%B8%EB%84%90iTerm%EC%9C%BC%EB%A1%9C-node.js-%EC%84%A4%EC%B9%98
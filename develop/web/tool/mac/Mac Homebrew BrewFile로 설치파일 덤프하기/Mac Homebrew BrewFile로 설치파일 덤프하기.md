# dump

> 해당 명령어를 실행하면 BrewFile이 만들어진다

```
brew bundle dump
```

# restore

> BrewFile이 있는 곳에서 해당 명령어 실행

```
brew bundle
```

# BrewFile 구조

```
tap "homebrew/bundle"
tap "homebrew/cask"
tap "homebrew/core"
tap "homebrew/services"
brew "curl"
brew "gradle"
brew "htop"
brew "nvm"
brew "openjdk@11"
brew "openssl@3"
brew "yarn"
cask "datagrip"
cask "discord"
cask "docker"
cask "drawio"
cask "fig"
cask "figma"
cask "google-chrome"
cask "intellij-idea"
cask "iterm2"
cask "jandi"
cask "keka"
cask "mark-text"
cask "mysqlworkbench"
cask "naver-whale"
cask "notion"
cask "obsidian"
cask "postman"
cask "slack"
cask "spectacle"
cask "warp"
```

- homebrew로 설치한 내역들이 적히게 되고, `brew bundle`로 실행시 해당 내역들을 모두 install하게 된다
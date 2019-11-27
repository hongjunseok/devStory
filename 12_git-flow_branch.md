# Git-flow
> git-flow 브런치 전략을 사용함으로써, 코드의 품질 향상 및 가독성을 향상 시킬 수 있다.

### 설치
```
# homebrew(설치관리자) 설치
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
 
# git flow 설치
$ brew install git-flow
```

### 기본 개념
* master 를 통해 develop 브런치가 생성이 되며
* 새 기능 개발은 feature 브런치를 생성하여 개발 하되
* feature 브런치는 Local Repository 에만 존재하며, 짧은 생명주기를 가지고 있어야 한다.
* 새 기능이 완료되면, develop 브런치에 커밋이 이뤄지며,
* 아무런 문제가 없을 경우 release 브런치를 생성하여, 제품을 출시한다.
* 출시한 제품에 문제가 생기면 master 브런치를 통해 hotfix 브런치를 생성하여, 긴급 버그를 처리하고,
* 긴급한 버그 및 이슈가 아닌 이상 feature 를 통해 개발하고 테스트 하여 버전을 출시 하는 것을 원칙으로 한다.

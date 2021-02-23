---
title: 'Dev Tips \| 개인적인 개발 세팅 메뉴얼'
date: 2021-02-23
last_modified_at: 2021-02-24T01:47:21

category:
  - Dev Tip

tags:
  - setting
---

본 글은 제가 새로운 기기로 개발 환경을 세팅할 때 이전에 사용해오던 설정을 기억하기 위한 기록입니다.

최근 임시로 기기를 대여받아 1~2달 정도 사용하고 교체하는 일이 반복적으로 있었습니다.

매번 개발 툴을 설치하고 설정하다가 똑같은 부분에서 막히고 똑같은 내용을 다시 검색하는 제 모습을 보며 개발 세팅 메뉴얼의 필요성을 느꼈습니다.

그래서 아래 내용은 다른 사람에게 알려주고 추천하기 위함이 아닌, 어디까지나 제가 개인적으로 사용하는 방법임을 분명하게 밝힙니다.

조금 부정확하거나 엉성한 내용이 많다는 뜻입니다^^

작업은 macOS Catalina 10.15.5 에서 진행했습니다.


## Homebrew
[homebrew 공식 사이트](https://brew.sh/index_ko)에 접속하여 `Homebrew 설치하기`에 적힌 스크립트를 터미널에 복사해 설치합니다.

또한 gui 프로그램도 homebrew를 통해 설치할 수 있도록 해주는 `cask` 패키지도 함께 설치합니다.

```sh
$ brew install cask
```

> [macOS 용 패키지 관리자 — Homebrew](https://brew.sh/index_ko)
>
> [Homebrew(홈브류) 설치 및 사용법, MacOS에서 프로그램을 쉽게 다운로드 및 삭제할 수 있는 패키지 관리자](https://whitepaek.tistory.com/3)



## Vim
vim은 왠만하면 기본적으로 제공되기 때문에 굳이 새로 설치할 필요는 없습니다.

게다가 저는 커밋 메세지 적을 때랑 터미널 설정 만질 때 빼고는 거의 사용하질 않아서 최신 버전을 설치하지 않아도 무방하지만, 그래도 포스팅 하는 김에 vim도 업데이트 해보겠습니다.

```sh
$ brew install vim
```

그리고나서 `.vimrc` 파일을 생성해 vim의 세부 설정을 진행합니다.

```sh
$ vim ~/.vimrc
```

다음은 제가 주로 사용하는 설정입니다.
`.vimrc` 파일에 아래 설정들을 추가해줍니다.

```
set number
set ruler
set nowrap

set ts=2
set sts=2
set shiftwidth=2

set showmatch
set smarttab
set smartindent

set hlsearch

set history=1000

set mouse=a

set backspace=indent,eol,start
```



## 터미널 설정
vim 작업을 한 김에 터미널 설정도 해줍니다.

터미널은 Catalina 기준 기본 터미널인 zsh를 사용하겠습니다.

우선 설정 파일을 생성합니다.

```sh
$ vim ~/.zshrc
```

저는 자주 쓰는 오타는 아예 alias로 지정하여 사용하는 편입니다.
대표적으로 `exit`를 `eixt`로 자주 적는데, 오타를 치더라도 원래 의도한 `exit` 명령어를 수행하도록 했습니다.

```
## set exit alias
alias eixt='exit'
```



## Chrome
위에서 설치한 `homebrew cask`를 이용하여 cli 상에서 설치할 수도 있습니다.

```sh
$ brew cask install google-chrome
```



## Visual Studio Code
[vscode 공식 사이트](https://code.visualstudio.com/)에서 다운로드합니다.

다운이 완료되면 지정한 경로에 `.zip` 파일과 압축이 해제된 vscode 어플리케이션 파일 `Visual Studio Code.app`이 생성될 것입니다. 
Mac의 `Finder`에서 다운 받은 `Visual Studio Code.app`을 `응용 프로그램`에 드래그 앤 드랍해서 넣어줍니다.

그러면 `Launchpad`에서 vscode를 확인할 수 있는데, 추가로 vscode를 터미널에서 `code` 명령어로 실행할 수 있도록 설정하겠습니다.

vscode를 실행한 후에, `CMD + shift + p`를 눌러 command palette를 실행합니다.

command palette에서 `shell command`로 검색하면 `code` 커맨드를 PATH에 설치하는 작업을 찾을 수 있습니다.

해당 작업을 수행하여 터미널에서 `code` 명령어를 사용을 설정합니다.

> [Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)
>
> [김군의 삽질메모장 :: MAC OS X에 Visual Studio Code 설치하기 및 쉘, 터미널에서 Code 실행하기](https://devkimgoon.tistory.com/6)



## Git
우선 터미널에서 현재 git이 설치되어 있는지 확인합니다.

```sh
$ git --version
git version 2.x.x (Apple Git-xx)
```

보통 기본적으로 git이 설치되어 있지만 최신 버전이 아닐 때가 많습니다.

이를 homebrew를 통해 최신버전으로 업데이트 하겠습니다.

```sh
$ brew install git
```

git을 설치한 뒤에 터미널을 재시작하면 최신 버전의 git이 적용됩니다.

이제 git을 설치했으니 후속 설정도 해주겠습니다.

먼저 유저와 기본 에디터를 설정합니다.

```sh
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
$ git config --global core.editor vim
```

그리고 git도 마찬가지로 alias로 추가합니다.

```sh
$ git config --global alias.chekcout checkout
$ git config --global alias.tree "log --all --decorate --oneline --graph"
```

자주 실수하는 오타를 위한 alias를 설정했고, 깔끔하게 log를 확인하는 옵션을 단축키로 만들었습니다.

다음으로 터미널에 git branch를 표시하는 설정입니다.
아래 라인을 `~/.zshrc` 등의 터미널 설정 파일에 추가합니다.

```
## show git branch on shell
autoload -Uz vcs_info
precmd_vcs_info() { vcs_info }
precmd_functions+=( precmd_vcs_info )
setopt prompt_subst
RPROMPT=\$vcs_info_msg_0_
# PROMPT=\$vcs_info_msg_0_'%# '
zstyle ':vcs_info:git:*' formats '%b'
```

> [Git - Git Aliases](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)
> [Git - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)
> [Git - Git in Zsh](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Zsh)



## NVM
brew를 통해 NVM을 설치합니다.

```sh
$ brew install nvm
```

```sh
Please note that upstream has asked us to make explicit managing
nvm via Homebrew is unsupported by them and you should check any
problems against the standard nvm install method prior to reporting.

You should create NVM's working directory if it doesn't exist:

  mkdir ~/.nvm

Add the following to ~/.zshrc or your desired shell
configuration file:

  export NVM_DIR="$HOME/.nvm"
  [ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && . "/usr/local/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

You can set $NVM_DIR to any location, but leaving it unchanged from
/usr/local/opt/nvm will destroy any nvm-installed Node installations
upon upgrade/reinstall.

Type `nvm help` for further information.
```

설치가 완료되면 위와 같은 메세지가 나옵니다.

메세지에서 안내하는대로 `.nvm` 디렉토리가 없을 시에 생성해주고, PATH 설정 관련 스크립트를 터미널 설정 파일<sub>(`.zshrc`)</sub>에 복사합니다.



## Node.JS
앞서 설치한 NVM을 이용하여 Node.JS를 설처합니다.

```sh
$ nvm install lts
```



## yarn
```sh
$ brew install yarn
```

brew를 통해 yarn을 설치하면 dependency인 icu4c와 node가 함께 설치됩니다.

미리 node를 설치했다면

```sh
$ brew install yarn --ignore-dependencies
## 혹은
$ brew install yarn --without-node
```

으로 node의 추가적인 설치를 막는 방법도 있습니다.

공식 문서에서는 node가 이미 설치돼 있다면 `brew install yarn`을 하더라도 node가 중복으로 설치되지 않는다고 했지만 실제로는 icu4c와 node 둘 다 설치됐습니다.

그래도 일단은 yarn이 큰 문제없이 작동하는 것 같은데 사용해보고 문제가 생기면 이후에 내용을 추가하겠습니다.

> [Installation \| Yarn](https://classic.yarnpkg.com/en/docs/install/#mac-stable)



## 블로그
이번에 블로그 세팅하는데 많이 애를 먹었습니다...

제가 React, Node.JS에 익숙하다보니 ruby 관련 문제는 더 어렵게 다가왔습니다.

아무래도 얼른 블로그를 React 기반의 정적 페이지로 마이그래이션 해야할 것 같습니다.

아무튼, 여느 경우와 마찬가지로 homebrew를 이용해서 ruby를 설치합니다.

```sh
$ brew install ruby
```

ruby도 기본적으로 깔려있긴 합니다.
애초에 vim을 구동하는데 ruby가 필요한 것 같더라구요.

그리고 새로 설치한 ruby의 경로를 `$PATH`에 추가해줍니다.

```sh
$ echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc
```

이제 블로그 디렉토리에 이동해서 `bundler`와 `jekyll`을 설치하겠습니다.

```sh
$ gem install bundler jekyll
```

여기서 만약 ruby 경로를 설정해주지 않으면 `Gem::FilePermissionError`가 발생합니다.

기본으로 제공되는 ruby가 애초에 write permission이 부여되지 않아서 그런지 어떤지는 정확히는 모르겠습니다.

`$PATH`를 설정하지 않더라도 `rbenv`를 설치하여 사용할 ruby의 버전을 지정하는 방법도 있습니다.
하지만 저는 ruby를 블로그에서만 사용하고 평소 개발할 땐 전혀 다루지 않기 때문에 그렇게까지 설정해야할 필요를 못 느꼈습니다.

이제 dependencies를 설치하고 블로그를 로컬에서 호스팅합니다.

```sh
$ bundle install
$ bundle exec jekyll serve
```

만약 `'require': cannot load such file -- webrick (LoadError)` 에러가 발생한다면 `webrick`을 추가하여 해결합니다.

```sh
$ bundle add webrick
```

추가로, 블로그 레포는 제 개인 깃헙 계정 소유이지만, 기기의 글로벌 깃헙 유저는 회사 계정으로 설정해놨기 때문에 블로그 레포에서만 적용할 로컬 유저, 이메일을 설정합니다.

```sh
$ git config --local user.name "John Doe"
$ git config --local user.email johndoe@example.com
``` 

> (맥OS 에 Jekyll 설치 \| Jekyll • 심플한, 블로그 지향적, 정적 사이트)[https://jekyllrb-ko.github.io/docs/installation/macos/]
>
> (jekyll을 설치하고 로컬에 블로그를 뛰우려고 합니다. : frhyme.code)[https://frhyme.github.io/blog/install_jekyll_again/]



## 자주 까먹는 미세 팁
* 환경변수 경로 `$PATH`는 `echo $PATH`로 확인할 수 있습니다.
* 오랜만에 homebrew를 쓸 땐 `brew update && brew upgrade`를 잊지 말고 해줍니다.



## Checklist
이번 글에서 다룬 내용을 정리한 체크리스트입니다.
추가로 제가 애용하는 어플리케이션도 포함했습니다.

- [ ] Homebrew
- [ ] Vim
- [ ] 터미널 설정
- [ ] Chrome
- [ ] Visual Studio Code
- [ ] Git
- [ ] NVM
- [ ] Node.js
- [ ] yarn
- [ ] 블로그

- [ ] 카카오톡
- [ ] slack
- [ ] zoom
- [ ] Microsoft To Do
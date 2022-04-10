---
title: 'Dev Tips \| 깃 브랜치 관리에 유용한 명령어 모음'
excerpt: 'git checkout --track, rebase --onto, cherry-pick'
date: 2022-04-10
last_modified_at: 2022-04-11T00:35:43

category:
  - Dev Tip

tags:
  - git
---

Git은 개발에 있어 빼놓을 수 없는 필수 시스템입니다. 
깃 덕분에 협업 과정에서 동시다발적으로 일어나는 코드 수정에 효율적으로 대응할 수 있기 때문입니다.

깃을 이용한 체계적인 브랜치 전략 또한 쉽게 접할 수 있는데요, 이 글에선 그런 브랜치 전략을 이행하다보면 마주치게 되는 몇몇 상황을 대처하기에 유용한 명령어들을 정리해봤습니다.

## checkout --track
리모트 브랜치를 로컬 브랜치로 가져오는 명령어 입니다.

좀 더 정확히는 로컬에 리모트와 동일한 이름의 브랜치를 생성하고, 로컬 브랜치에 리모트 브랜치를 merge 합니다.
이때 `--track` 옵션으로 생성된 브랜치는 **track branch** 라고 부르고, track branch가 tracking 하는 리모트 브랜치는 **upstream branch** 라고 부릅니다.
또한 `git branch -vv` 명령어를 통해 track branch를 확인할 수 있으며, `git pull` 등을 수행할 때 track branch는 자동적으로 upstream branch가 merge 됩니다.

`git checkout --track` 하기 전엔 `git fetch`를 통해 브랜치를 로컬로 fetch 해야 생성할 수 있습니다.

사실 track branch를 생성하는 동작은 워낙 자주 사용하기 때문에 `--track` 없이 단순 체크아웃 만으로도 만들어주기는 합니다.

```sh
$ git fetch origin <branch_name>
$ git checkout --track <branch_name>
## or
$ git checkout <branch_name>
```

참고로 `checkout` 대신 `switch`를 사용해서 track branch를 생성하고 이동하는 일련의 과정을 동일하게 수행할 수 있습니다.
`switch`는 `restore`와 함께 2.23 버전부터 너무 방대해진 `checkout`의 기능을 분리하기 위해 추가된 명령어 입니다.



## rebase --onto
`rebase`는 커밋을 다시 적용하는 것과 관련한 동작을 수행하는 명령어 입니다.

우선 아무런 옵션 없이 `git rebase <new_base>`를 하면 현재 브랜치의 커밋을 `<new_base>` 끝에 붙여줍니다.

만약 rebase 할 때 `<new_base>` 뒤에 브랜치를 추가로 써주면 입력된 브랜치로 checkout 한 뒤에 rebase 하는 것과 같은 작업을 수행합니다.
rebase할 브랜치를 지정한다고 보면 될 것 같습니다.

```sh
## git rebase <new_base>
$ git checkout feature
$ git rebase main

$ git rebase main feature   ## checkout + rebase
```
```
## before
          A---B---C feature (HEAD)
         /
    D---E---F---G main

## after
                  A'--B'--C' feature (HEAD)
                 /
    D---E---F---G main
```

이와 같이 브랜치의 base를 부모 브랜치의 최신 커밋으로 변경하고 싶을 때 유용하게 사용할 수 있습니다.

그런데 만약 `<new_base>`와 현재 base가 다를 경우에 rebase를 사용하면 어떻게 될까요?

```sh
$ git rebase main
```
```
## before
                            H---I---J featureB (HEAD)
                           /
                  E---F---G  featureA
                 /
    A---B---C---D  main

## after
                 E'--F'--G'--H'--I'--J'  featureB (HEAD)
                /
                | E---F---G  featureA
                |/
    A---B---C---D  main
```

보시는 것처럼 `main`과 `HEAD` 사이의 모든 커밋이 `main` 이후로 rebase 됩니다.

`featureB` 브랜치에만 존재하는 커밋 기록을 그대로 유지하면서 base만 변경하고 싶다면 위 결과는 의도와는 부합하지 않습니다.
이럴 때 사용하는 것이 `--onto` 옵션 입니다.

```sh
## git rebase --onto <new_base> <old_base>
$ git rebase --onto main featureA
$ git rebase --onto main featureA featureB    ## base of featureB: featureA -> main
```
```
## before
                            H---I---J featureB (HEAD)
                           /
                  E---F---G  featureA
                 /
    A---B---C---D  main

## after
                 H'--I'--J'  featureB (HEAD)
                /
                | E---F---G  featureA
                |/
    A---B---C---D  main
```

`rebase --onto`도 그냥 `rebase`와 마찬가지로 세 번째로 입력되는 브랜치가 있다면 그곳으로 먼저 checkout한 뒤 `rebase --onto`를 수행합니다.
즉 base를 변경할 브랜치를 지정하는 것과 같습니다.

`rebase --onto`를 활용하면 연속된 커밋들을 제거할 수도 있습니다.

```sh
$ git rebase --onto feature~5 feature~3 feature
```
```
## before
    E---F---G---H---I---J  feature

## after
    E---H'---I'---J'  feature
```



## cherry-pick
`cherry-pick`은 특정 커밋을 `HEAD`로 옮기는 명령어입니다.
특정 커밋 만을 쏙 골라서 가져오는 것이 채리를 따는 것을 연상시켜 이런 명령어를 사용하는 것 같습니다.

cherry pick의 사용법은 간단합니다.
가져올 commit id를 입력해주기만 하면 됩니다.

```sh
## git cherry-pick commitSha
$ git cherry-pick F
```
```
## before
          E---F---G feature
         /
    A---B---C---D main (HEAD)

## after
          E---F---G feature
         /
    A---B---C---D---F' main (HEAD)
```

연속적인 다수의 커밋을 체리픽하는 것도 가능합니다.

```sh
$ git cherry-pick ..feature
```
```
## before
          E---F---G feature
         /
    A---B---C---D main (HEAD)

## after
          E---F---G feature
         /
    A---B---C---D---E'---F'---G' main (HEAD)
```

위와 같은 방식으로 공통 부모까지의 커밋을 모두 채리픽할 수 있습니다.

또한 시작점을 지정하여 가져올 커밋의 범위를 설정할 수도 있습니다.

```sh
## git cherry-pick <start_commit>..<end_commit>
$ git cherry-pick feature~2..feature
```
```
## before
          E---F---G feature
         /
    A---B---C---D main (HEAD)

## after
          E---F---G feature
         /
    A---B---C---D---F'---G' main (HEAD)
```

보시는 것처럼 `<start_commit>`은 **미포함하고 다음 커밋부터** `<end_commit>`까지를 가져왔습니다.

여기서 한 가지 주위할 점이 있습니다.
`..` 없이 단순 두 가지 커밋을 나열하면 두 커밋 사이의 모든 커밋을 가져오지 않고, 입력한 두 커밋만을 가져옵니다.

```sh
$ git cherry-pick feature~2 feature
```
```
## before
          E---F---G feature
         /
    A---B---C---D main (HEAD)

## after
          E---F---G feature
         /
    A---B---C---D---E'---G' main (HEAD)
```

이외에 cherry-pick의 대표적인 옵션 두 가지만 소개하겠습니다.
* `-e`, `--edit`: 채리픽하며 커밋 메시지를 수정합니다.
* `-n`, `--no-commit`: 채리픽한 변경사항을 커밋하지 않고 staged 상태로 둡니다.

cherry-pick은 커밋을 다른 브랜치로 옮길 수 있다는 점에서 앞서 소개했던 rebase와 기능이 일정 부분 겹칩니다.

하지만 각자 유용하게 사용되는 상황이 조금은 다릅니다.
base branch를 잘못 설정했을 때는 똑같이 커밋을 옮기더라도 브랜치 전체를 이동하는 rebase가 훨씬 편합니다.
cherry-pick 한다면, 커밋들을 옮긴 후에 원래 커밋이 있던 브랜치로 돌아가 채리픽된 커밋을 삭제해줘야 하기 때문입니다.
반대로 rebase론 해결하지 못하는 경우도 있습니다.

```
## AS-IS
                  E---F---G develop
                 /
    A---B---C---D develop (origin)

## TO-BE
                  E---F---G feature
                 /
    A---B---C---D develop (origin)
```

개발을 하다보면 위와 같이 브랜치를 생성하여 변경사항을 분리해야 하는 상황에서 실수로 base branch에 커밋하는 상황을 심심치 않게 보게 됩니다.
이럴 때 다음과 같이 cherry-pick을 사용하여 커밋 `E`, `F`, `G` 를 `feature` 브랜치로 옮겨 보겠습니다.

```sh
$ git checkout origin/develop
$ git checkout -b feature
$ git cherry-pick ..develop
$ git checkout develop
$ git reset --hard origin/develop
```

각 명령어 마다 커밋 로그가 어떻게 변하는지 하나하나 살펴보겠습니다.

1. 채리픽할 시작점으로 이동
```sh
$ git checkout origin/develop
```
```
                  E---F---G develop
                 /
    A---B---C---D origin/develop (HEAD)
```

1. 채리픽할 변경사항을 커밋할 브랜치 생성
```sh
$ git checkout -b feature
```
```
                  E---F---G develop
                 /
    A---B---C---D origin/develop feature (HEAD)
```

1. 가져올 커밋 채리픽
```sh
$ git cherry-pick ..develop
```
```
                 E---F---G develop 
                /
                | E'---F'---G' feature (HEAD)
                |/
    A---B---C---D  origin/develop
```

1. 잘못 커밋된 변경사항을 되돌리기 위해 이전 브랜치로 이동
```sh
$ git checkout develop
```
```
                 E---F---G develop (HEAD)
                /
                | E'---F'---G' feature
                |/
    A---B---C---D  origin/develop
```

1. 잘못된 커밋 삭제
```sh
$ git reset --hard origin/develop
```
```
                  E'---F'---G' feature
                 /
    A---B---C---D origin/develop feature (HEAD)
```

이 외에도 `cherry-pick`만의 다양한 용례는 [여기](https://www.atlassian.com/git/tutorials/cherry-pick)에서 참고할 수 있습니다.



## References
> [Git - Remote Branches](https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches#_tracking_branches)
>
> [Git - git-switch Documentation](https://git-scm.com/docs/git-switch)
>
> [Git - git-rebase Documentation](https://git-scm.com/docs/git-rebase)
>
> [git - Change branch base - Stack Overflow](https://stackoverflow.com/questions/10853935/change-branch-base)
>
> [Git Cherry Pick \| Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/cherry-pick)
>
> [Git - git-cherry-pick Documentation](https://git-scm.com/docs/git-cherry-pick)
---
title: 'Dev Tip \| 가끔 사용하는 깃 명령어 모음'
excerpt: 'git clone --mirror, reset, revert'
date: 2020-10-04
last_modified_at: 2022-04-11T00:40:30

category:
  - Dev Tip

tags:
  - git
---

1일 1커밋을 지향하기에 git은 거의 매일 사용하고 있습니다.
하지만 사용하는 명령어 몇 개만 계속 쓰다보니 익숙한 명령어는 매크로처럼 손가락이 자동적으로 움직이는 반면, 있는 줄도 모르는 옵션, 명령어는 모르는 채로 계속 내버려 두게 되더라구요.
물론 필요한 상황이 되면 알아서 찾아서 사용하겠지만, 드물게 사용하는 명령어들은 필요할 때마다 찾고, 또 찾고, 봤던 글 보고, 또 보고 ...

그래서 이번 기회에 가끔씩 사용하는 깃 명령어들을 정리해 필요할 때마다 참고할려고 합니다.


## git clone --mirror
```sh
$ git clone --mirror
```
`git clone`을 할 때 `--mirror` 옵션을 준다면 브랜치와 커밋 기록 등 레포지토리 자체를 복사합니다.

`--mirror`보다 조금 더 얕게 복사하는 `--bare` 옵션도 있는데 솔직히 관련 글을 아무리 읽어봐도 `--mirror`와 `--bare`의 차이점이 이해가 안되서... [하단](#git-clone-mirror)에 레퍼런스를 첨부했으니 필요하신 분들은 직접 확인해보시면 좋을 것 같습니다.

아무튼 레포지토리 자체를 옮긴다거나 할 때 `--mirror` 옵션은 유용합니다.
저 같은 경우는 팀에서 private으로 진행한 프로젝트 레포를 개인 계정의 public 레포로 옮길 때 사용했습니다.

`git clone`을 하면 레포지토리 이름의 디렉토리가 생성되고 그 안에 `.git` 파일이 들어가지만 `git clone --mirror`을 하면 `.git` 자체가 `$GIT_DIR`이 되어 `[repo name].git`의 형태로 다운됩니다.

추가로, 위키는 `git clone --mirror` 하더라도 복사되지 않습니다.
**위키까지 옮기고 싶다면** `git clone [Repository Name].wiki.git`을 입력하는 것으로 가능하며 url은 위키 탭에서도 복사할 수 있습니다.


## git remote update
```sh
$ git remote update
```
`git clone --mirror`를 통해 클론한 레포는 `git pull` 명령어로 remote의 변경 사항을 가져올 수 없습니다.
대신 `git remote update` 명령어로 로컬 레포의 모든 refs들이 덮어씌워 업데이트 합니다.

## git push --mirror
```sh
$ git push --mirror
```

`git clone --mirror`한 로컬 레포를 다른 리모트 저장소로 푸시할 때 사용하는 명령어 입니다.

아래 명령어로 `git remote`의 푸시할 url을 변경하여 다른 곳으로 옮길 수도 있습니다.

```sh
$ git remote set-url --push origin https://github.com/exampleuser/mirrored
```



## git reset
`git reset`은 커밋을 되돌리는 명령어이며 대략적인 사용법은 아래와 같습니다.

```sh
$ git reset head~
## or
$ git reset c6cdc2f
```

`head`를 사용해서 되돌아갈 커밋을 상대적으로 지정할 수도 있고, 커밋 id를 직접 입력하여 절대적으로 지정할 수도 있습니다.

`head`는 현재 `head`가 가리키는 커밋을 의미하며, `head~n`은 현재 `head`로부터 `n` 번째 전 커밋을, `head^`은 바로 직전 커밋을 의미합니다.

- `head`는 `head~0`와 같습니다.
- `head~1`은 바로 이전 커밋(부모 커밋)과 같으며 `head~`, 혹은 `head^` 라고도 쓸 수 있습니다.
- 예를 들어 최근 3개의 커밋을 하기 전 상태로 돌아가고 싶다면 되돌아갈 곳에 `head~3`이라고 입력하면 됩니다.

`git reset`은 모드에 따라 커밋을 취소하는 방법이 조금씩 다릅니다.
여기서는 대표적인 모드 `hard`, `mixed`, `soft`에 대해 알아보겠습니다.


### `git reset --hard`
```sh
$ git log --oneline
403b8c1 (HEAD -> master) d
8b58c2e c
c6cdc2f b
75816bb a
$ git reset --hard c6cdc2f
$ git log --oneline
c6cdc2f (HEAD -> master)  b
75816bb a
```

`git reset --hard <commit>`은 `head`를 `<commit>`으로 돌리고, `<commit>` 이후의 변경사항들은 모두 없애버립니다.


### `git reset --mixed`
```sh
$ git log --oneline
403b8c1 (HEAD -> master) d
8b58c2e c
c6cdc2f b
75816bb a
$ git reset --mixed c6cdc2f
$ git log --oneline
c6cdc2f (HEAD -> master)  b
75816bb a
$ git status
현재 브랜치 master
추적하지 않는 파일:
  (커밋할 사항에 포함하려면 "git add <파일>..."을 사용하십시오)
  c
  d

커밋할 사항을 추가하지 않았지만 추적하지 않는 파일이 있습니다 (추적하려면 "git add"를 사용하십시오)
```

`git reset --mixed <commit>`은 `head`를 `<commit>`으로 돌리고, `<commit>` 이후의 변경사항들은 `Modified` 상태로 변경합니다.

`--mixed` 모드는 `reset`의 기본값이기도 합니다.
그래서 `git reset`이라고만 쳐도 `mixed`와 동일하게 동작합니다.


### `git reset --soft`
```sh
$ git log --oneline
403b8c1 (HEAD -> master) d
8b58c2e c
c6cdc2f b
75816bb a
$ git reset --soft c6cdc2f
$ git log --oneline
c6cdc2f (HEAD -> master)  b
75816bb a
$ git status
현재 브랜치 master
커밋할 변경 사항:
  (use "git restore --staged <file>..." to unstage)
  새 파일:       c
  새 파일:       d
```

`git reset --soft <commit>`은 `head`를 `<commit>`으로 돌리고, `<commit>` 이후의 변경사항들은 `Staged` 상태로 변경합니다.

정리하자면, `soft` -> `mixed` -> `hard` 로 갈수록 변경된 사항을 보존하기 어렵다고 볼 수 있겠습니다.

추가로, `git reset`의 모드를 이용하여 파일의 상태를 `Staged`에서 `Modified`로 변경할 수도 있습니다.
`git restore --staged <file>`을 사용한 것처럼 말입니다.

```sh
$ git status
현재 브랜치 master
커밋할 변경 사항:
  (use "git restore --staged <file>..." to unstage)
  새 파일:       c
  새 파일:       d
$ git reset head c
$ git status
커밋할 변경 사항:
  (use "git restore --staged <file>..." to unstage)
  새 파일:       d
추적하지 않는 파일:
  (커밋할 사항에 포함하려면 "git add <파일>..."을 사용하십시오)
  c
```

이런 식으로 커밋을 되돌리고 `git push` 하면 리모트 레포의 커밋을 되돌릴 수도 있습니다.

단, 그냥 `git push`를 한다면 현재 커밋이 리모트 레포의 커밋보다 뒤에 있기 때문에 `push`가 실패합니다.
이를 해결하기 위해 `-f` 또는 `--force` 옵션으로 `push` 해줍니다.
물론, 강제로 push를 하는 것은 변경 이력 자체도 삭제하는 것이기 때문에 프로덕션 코드에선 revert를 하는 편이 안전하다고 볼 수 있겠습니다.

```sh
$ git push -f origin master
```



## git revert
`git revert` 또한 `git reset`과 마찬가지로 커밋을 되돌립니다.

하지만 `git reset`과는 다르게 커밋을 되돌렸다는 과정 자체를 새로 커밋하는 방식입니다.
덕분에 되돌리는 작업의 기록이 가능합니다.

```sh
$ git log --oneline
403b8c1 (HEAD -> master) d
8b58c2e c
c6cdc2f b
75816bb a
$ git revert head
$ git lot --oneline
dbe18e3 (HEAD -> master) Revert "d"
403b8c1 d
8b58c2e c
c6cdc2f b
75816bb a
```

`git reset`의 경우 **지정하는 커밋으로 돌아갔지만**, `git revert`는 **지정하는 커밋을 되돌립니다.**

그러니깐, `git reset head~`는 현재 `head`를 하나 전 커밋으로 돌리기 때문에 가장 최근의 커밋이 삭제됩니다.
반면에 `git revert head~`는 현재 `head`로부터 하나 전 커밋을 되돌리기 때문에 하나 전의 커밋이 되돌려지고, 가장 최근의 커밋을 되돌리고 싶다면 `head~`가 아닌 `head`로 revert해줘야 합니다.

`git revert`로 동시에 여러 커밋을 되돌릴 수도 있습니다. `<commit>..<commit>`와 같이 되돌릴 커밋의 범위를 설정하면 됩니다.

```sh
## 1
$ git revert head~3..

## 2
$ git revert head~3..head~1

## 3
$ git revert -n head~3..
$ git commit
```

`1`은 가장 최근부터 3개 전 커밋까지를 되돌립니다.

`2`는 1개 전부터 3개 전 커밋을 되돌립니다.

단, `1`과 `2`는 되돌리는 커밋 개수만큼 새로운 커밋을 해줘야 합니다.
이런 불편함은 `-n` 혹은 `--no-commit` 옵션으로 해결할 수 있습니다.

`3`은 **커밋 없이** 가장 최근부터 3개 전 커밋까지를 `Staged` 상태로 되돌리며, 이후 `git commit`을 통해 커밋을 되돌렸다는 작업을 모아서 커밋할 수도 있습니다.



## References
### git clone --mirror
> [https://git-scm.com/docs/git-clone#Documentation/git-clone.txt---mirror](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt---mirror)
>
> [https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/duplicating-a-repository](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/duplicating-a-repository)
>
> [https://stackoverflow.com/questions/3959924/whats-the-difference-between-git-clone-mirror-and-git-clone-bare](https://stackoverflow.com/questions/3959924/whats-the-difference-between-git-clone-mirror-and-git-clone-bare)

### git reset
> [Git - Reset Demystified](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified)

### git revert
> [Git - git-revert Documentation](https://git-scm.com/docs/git-revert)
---
title: 'Dev Tip \| 가끔 사용하는 깃 명령어 모음'
excerpt:
date: 2020-10-04
last_modified_at: 2020-10-04T20:28:48

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
**위키까지 옮기고 싶다면** `git clone [Repository Name].wiki.git.wiki.git`을 입력하는 것으로 가능하며 url은 위키 탭에서도 복사할 수 있습니다.


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



## References
### git clone --mirror
> [https://git-scm.com/docs/git-clone#Documentation/git-clone.txt---mirror](https://git-scm.com/docs/git-clone#Documentation/git-clone.txt---mirror)
>
> [https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/duplicating-a-repository](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/duplicating-a-repository)
>
> [https://stackoverflow.com/questions/3959924/whats-the-difference-between-git-clone-mirror-and-git-clone-bare](https://stackoverflow.com/questions/3959924/whats-the-difference-between-git-clone-mirror-and-git-clone-bare)

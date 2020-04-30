---
title: 'Ubuntu \| Surface 전용 커널 설치'
excerpt: 'Ubuntu에 Surface 전용 커널을 설치해보겠습니다.'
date: 2020-04-30
last_modified_at: 2020-04-30T22:22:04

category:
 - Ubuntu

tags:
 - Ubuntu
 - Linux
 - Surface pro 4
 - kernel
 - git
---

> 본 글은 Surface Pro 4 / 128GB / 4GB 모델을 바탕으로 작성되었습니다.

## 세 줄 요약
[jakeday/linux-surface: Linux Kernel for Surface Devices](https://github.com/jakeday/linux-surface) 를 설치한 이후 

`/etc/default/grub`파일에

```bash
GRUB_SAVEDEFAULT=true
GRUB_DEFAULT=saved
```

를 추가하고 (`sudo`권한 필요) 터미널에 `sudo update-grub`을 입력해줍니다.



## 들어가기 전에...
드디어 제 서피스에 우분투를 설치했습니다.

설치 방법은 아래를 참고하여 진행했습니다.

[Create a bootable USB stick on Windows \| Ubuntu](https://ubuntu.com/tutorials/tutorial-create-a-usb-stick-on-windows#6-write-the-iso)

[Install Ubuntu desktop \| Ubuntu](https://ubuntu.com/tutorials/tutorial-install-ubuntu-desktop#8-select-your-location)

진행하다보면 파티션을 나눠서 SWAP 영역과 우분투를 설치할 공간을 설정하는 창이 나오는데

충분한 RAM 공간이 있다면 굳이 SWAP 영역을 설정할 필요는 없어 보입니다. 

그렇지 않다면 보통 RAM의 1.5~2배 정도의 공간을 SWAP을 위해 할당하는 것 같습니다.

물론 저는 RAM과 SSD 둘 다 용량이 부족해서 RAM과 동일한 4GB의 공간을 할당했습니다.ㅠㅠ

그리고 2020년 4월 23일 기준으로 20.04 LTS 버전이 배포 되었지만 개발도구는 최신버전을 쓰는 것이 아니라고 배웠기 때문에... 18.04 LTS 버전을 설치하였습니다.

> [FocalFossa/ReleaseNotes - Ubuntu Wiki](https://wiki.ubuntu.com/FocalFossa/ReleaseNotes)



## Linux Kernel for Surface
아무튼 우분투를 설치하고 나면 18.04 버전 기준으로 `Linux 5.3.0-46-generic` 커널이 설치될 것입니다.

해당 커널에선 터치 스크린이 작동하지 않고, 저만의 문제였는지는 모르겠지만 블루투스도 자주 끊기는 문제가 있었습니다.

USB포트가 부족한 서피스를 사용하다보니 자연스레 주변 악세서리는 모두 블루투스 기기였고 그런 상황에서 터치와 블루투스가 안되니깐 PC에 어떠한 입력도 할 수 없게 돼 정말 답답했습니다.

또 저는 서피스 전용 키보드인 typecover를 사용하진 않아서 정확히는 모르지만 typecover도 인식이 안된다고 하더라고요.

그래서 서피스 전용 커널을 찾아 설치하였습니다.

[jakeday/linux-surface: Linux Kernel for Surface Devices](https://github.com/jakeday/linux-surface)

서피스 전용 커널로 이 외에도 한 두어 가지가 더 있었지만 기능은 거의 동일하면서 위 repo가 가장 설치하기 쉽고 인지도도 가장 높았습니다.

설치방법은 `README.md`를 참고하시면 큰 어려움 없이 설치할 수 있을 것입니다.

단, 커널을 설치해도 SP4 이후 모델부터는 내장 웹캠을 인식 못하는 것 같습니다.

저도 그래서 웹캠을 따로 구해서 사용 중이고요...



## 부팅 기본값 설정
서피스 커널을 설치한다고 해서 바로 사용할 수 있는 건 아니고 부팅 시 `grub`에서 서피스 커널을 선택해야만 사용 가능합니다.

`grub` 창에서 `Advanced options for Ubuntu` > `Ubuntu, with Linux 5.1.15-surface-linux-surface`를 선택합니다.

하지만 기본값은 여전히 `Linux 5.3.0-46-generic` 커널로 설정됐기 때문에 부팅 시 잠시 한눈이라도 팔면 재부팅을 해야하는 귀찮음이 남아 있습니다.

그래서 `grub`이 이전 선택을 기억할 수 있는 설정을 추가해서 이전에 서피스 커널로 Ubuntu를 사용했다면 다음에도 서피스 커널이 자동으로 선택되도록 해보겠습니다.

먼저, `/etc/default/grub` 파일 열고,

```bash
GRUB_SAVEDEFAULT=true
GRUB_DEFAULT=saved
```

설정을 추가해줍니다.

참고로 `/etc/default/grub` 파일을 수정하려면 `sudo` 권한이 필요하므로 주의하시기 바랍니다.

그리고 터미널에 

```bash
sudo update-grub
```

을 입력해줘서 변경사항이 적용되도록 해줍니다.
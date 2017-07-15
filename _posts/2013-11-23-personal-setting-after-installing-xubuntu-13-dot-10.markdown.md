---
layout: post
title: "[Ubuntu] (X)Ubuntu 13.10 설치 후 개인적인 설정"
date: 2013-11-23 22:03:10
comments: true
categories: etc
tags: ['linux','ubuntu']
---

**XUbuntu 13.10** 새로 설치하면서 기본적인 **추가적인 설정 내용**을 정리.<br>
예전에 [Ubuntu 12.04](http://tkhwang.kr/blog/2013/03/03/a-few-personal-setting-after-installing-linux-12-dot-04/) 때도 살짝 적어두니깐 나중에 참고하기 좋아서 다시 정리함. 

<!--more-->

### [#1] 배포판 결정 : Xubuntu

그동안 linux 사용할 경우에 크게 고려없이 ubuntu 를 사용해왔는데, 생각보다 많은 linux 배포판들이 있어서 이들 중에서 마음에 드는 것을 한 번
골라서 선택해보기 위해서 여러가지 배포판을 깔아서 조금씩 사용해보았다. 

* Linux 는 주로 **android 개발 환경**으로 사용 예정.
* **Android 개발 환경에 최적화**되고 패키지 설치 시에 **호환성** 문제 없어야 함.
* 버벅거림 없이 가벼우면 좋겠음.
* 이왕이면 theme 이쁘면 좋겠지만 앞으로는 이런 부분은 신경 많이 안 쓰고 싶음.
* ...

<!--more-->

최종 결정은 [Xubuntu 주분투](http://xubuntu.org/).

패키지 때문에 deb 을 쓰는 ubuntu 계열 중에서 최근 unity 가 많이 좋아지긴 했는데 필요없이 기능이 너무 많고 무거운 느낌이라서 
최종 결정은 [xfce](http://www.xfce.org/) 사용하는 [ubuntu](http://www.ubuntu.com/) 인 [xunbutu](http://xubuntu.org/) 로 결정함. 

#### Partition
* / : ext4
* /home : ext4
* swap

Linux partition 을 / 이외에 분리를 하지 않아서 system upgrade 등에 매번 기존 data 를 날려서 /home partition 을 분리하여 system upgrade 시에 문제 없도록 분리함.

#### 외장 하드 중의 일부 파티션을 ext4 format 및 linux 에 mount 하여 사용

외장 하드 중에 몇 개 파티션은 linux 용으로 format 하고, mount 하여 source build 등에 주로 사용할 예정임.

```bash
# tkhwang : extHDD
/dev/sdd1               /home/tkhwang/mnt/exthdd          ext4   defaults         0     2 
```

### [#2] 정리된 article 참고 

* [10 Things To Do After Installing Ubuntu 13.10](http://www.omgubuntu.co.uk/2013/10/10-things-installing-ubuntu-13-10)
* [8 THINGS TO DO AFTER INSTALLING UBUNTU 13.10 (SAUCY SALAMANDER)](http://www.webupd8.org/2013/10/8-things-to-do-after-installing-ubuntu.html)
* [Things To Do After Installing Ubuntu 13.10](http://itsfoss.com/things-to-do-after-installing-ubuntu-13-10/)


### [#3] 추가 pacakge 설치

* **Google chrome**
* **git** : can't image w/o it.
* [Terminator](https://apps.ubuntu.com/cat/applications/precise/terminator/) : 가로/세로 화면 분활 가능한 terminal 대용.
* [Sublime Text3](http://www.sublimetext.com/3) : 주로 read 에 사용하는 editor.
* **nabi** : 한글 입출력 IME. Shift+space 때문인지 항상 이를 쓴다.
* **Dropbox**
* **Evernote** Ubuntu Client : [Everpad](https://github.com/nvbn/everpad)

```bash
sudo add-apt-repository ppa:nvbn-rm/ppa
sudo apt-get update
sudo apt-get install everpad
```

* [Emacs24](http://linuxg.net/how-to-install-emacs-24-3-on-ubuntu-13-10-13-04-12-10-12-04-linux-mint-15-14-13-and-elementary-os-0-2-luna/)

```bash
sudo add-apt-repository ppa:cassou/emacs
sudo apt-get update
sudo apt-get install emacs24
```

#### .emacs 설정을 위한 [prelude](https://github.com/bbatsov/prelude#installing-emacs-24)

```
curl -L https://github.com/bbatsov/prelude/raw/master/utils/installer.sh | sh
```

* [Octopress](http://octopress.org/docs/setup/) : blogging framework (on gitHub) 설치.

#### Markdown editor : [ReText](http://sourceforge.net/projects/retext/)

* ~~[Uberwrite](http://uberwriter.wolfvollprecht.de/) ~~<br>
~~Octopress blog 가 markdown 으로 작성을 하기 때문에 markdown editor  설치. 위의 link 는 유료 이나 아래 ppa 로부터 package install 되네요.~~
* [In search of an open-source standalone markdown GUI editor](http://stackoverflow.com/questions/3255484/in-search-of-an-open-source-standalone-markdown-gui-editor)
* 처음에는 [Uberwrite](http://uberwriter.wolfvollprecht.de/) 를 사용하였는데, [ReText](http://sourceforge.net/projects/retext/) 가 two pane 으로 **live preview** 가 지원되서 더 편하군요.

### [#4] Powerline 설치 : statusline

* [(Ubuntu) Powerline : Shell, Vim 등의 Statusline 을 예쁘게...](http://tkhwang.kr/blog/2013/11/26/ubuntu-powerline-beautify-the-stateline/)

![img](https://powerline.readthedocs.org/en/latest/_images/pl-mode-normal.png)


### [#5] 폰트 설정 관련 

Font 와 theme 깔맞춤은 항상 가장 먼저 해야 함. <br>
최근에는 주로 `UbuntuMono`, `Monaco`, `NanumGothic`, `NanumGothicCoding` font 를 주로 사용한다.

* [monaco font](https://sites.google.com/site/jorrel/Monaco_Linux.ttf?attredirects=0). <br>
* Ubuntu 13.10 에는 나눔 font 류는 기본 포함이 되어 있네요.

#### ttf font 설치 

```bash
sudo mkdir /usr/share/fonts/truetype/custom/
sudo cp *.ttf /usr/share/fonts/truetype/custom/
sudo fc-cache -f -v
```

### [#6] Color theme 설정 관련 

* [Solarized theme](http://ethanschoonover.com/solarized) : 모든 툴의 color theme 은 solarized 로 대동단결.
![img](http://ethanschoonover.com/solarized/img/solarized-vim.png)
* [Terminator](https://github.com/ghuntley/terminator-solarized)
* [Gnome Terminal](https://github.com/sigurdga/gnome-terminal-colors-solarized)
* [Gnome Editor](https://github.com/mattcan/solarized-gedit)
* Sublime : font and theme (solarzied dark) configuration

```
* Preferences > Color scheme > Solarized (Dark)
* Preferences > File Settings - User

{
	"color_scheme": "Packages/Color Scheme - Default/Solarized (Dark).tmTheme",
	"font_face": "Ubuntu mono",
	"font_size": 12
}
```

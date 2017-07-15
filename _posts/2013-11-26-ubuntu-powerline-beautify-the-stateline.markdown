---
layout: post
title: "[Ubuntu] powerline : shell, vim 등의 statusline 을 예쁘게..."
date: 2013-11-26 22:43
comments: true
categories: etc
tags: ['android','utility']
---

요즘 여러가지 tool 을 살펴보면 몇가지 공통된 특징이 보인다.

<!--more-->

* **뛰어난 성능** : 이건 뭐 기본.
* Plugin 을 통한 **손쉬운 기능 확장**<br>
gitHub 등을 통한 open source 가 활발하게 되어 협력이 손쉽게 된 영향도 있겠지만 plugin 을 통하여 기능 확장이 잘 되어야 하는 것 같다. 더욱이 이러한 plugin 을 marketplace 같은 곳을 통하여 손쉽게 확인, 검색, 설치가 가능하도록 지원해주고 있는 듯 하다. <br>
-Emacs 의 [melpa](http://elpa.gnu.org/)<br>
-Sublime 의 package<br>
-Vim 의 bundle<br>
* **여러 다른 툴 사이의 공통된 경험 제공**<br>
툴의 개발자가 다르기 때문이겠지만 기존에는 툴이 다르면 서로 다른 Look & feel 을 나타내기 마련이었다. 하지만 최근에는 서로 다른 툴 사이에도 서로 공통된 사용자 경험을 줄 수 있도록 환경을 맞추어 주는 노력이 많이 이루어지고 있는 듯 하다. <br>
-[Solarzied](http://ethanschoonover.com/solarized) : color theme <br>
-[Powerline](https://powerline.readthedocs.org/en/latest/index.html) : Statusline<br>

그동안 몇 번 사용해봐야지 하다가 컴도 새로 설치한 기념으로 powerline 설치해봤다.
<!--more-->
![img](https://powerline.readthedocs.org/en/latest/_images/pl-mode-normal.png)
![img](https://powerline.readthedocs.org/en/latest/_images/pl-mode-insert.png)
![img](https://powerline.readthedocs.org/en/latest/_images/pl-mode-visual.png)
![img](https://powerline.readthedocs.org/en/latest/_images/pl-mode-replace.png)

* [Powerline gitHub](https://github.com/Lokaltog/powerline)
* [Powerline Doc](https://powerline.readthedocs.org/en/latest/index.html)
* [Installation on Linux](https://powerline.readthedocs.org/en/latest/installation/linux.html#installation-linux)
* [How can I install and use powerline plugin?](http://askubuntu.com/questions/283908/how-can-i-install-and-use-powerline-plugin)


### [#0] 기본 tool 설치 : python pip, git

* python 2.7 혹은 3.3 이 필요하다고 하네요.
* 혹시나 pip 와 git 설치.

```bash
$ sudo apt-get install python-pip git
```

### [#1] Powerline plugin installation (system-wide)
user 에게만 설치할 수도 있고, system wide 하게 설치할 수도 있는데... 나는 system wide 설치시도.
처음에 user 설치와 sytem 설치를 혼동했더니 powerline path 가 안 잡히고, `~/.local` 아래에 root 권한으로 파일이 생성되어 실행안 되고 하는 문제가 있었습니다. [How can I install and use powerline plugin?](http://askubuntu.com/questions/283908/how-can-i-install-and-use-powerline-plugin) 글 참고 시에는 user/system 구분 잘 하셔야 합니다.

```bash
$ sudo pip install git+git://github.com/Lokaltog/powerline
```

처음에 설정 시에 user/system 설치를 혼동하여 powerline executable path 가 잡히지 않아서 문제가 생긴 적이 있어서 설치 이후에 powerline 가 `/usr/local/bin` 에 잘 설치되어 있는지 확인.

```bash
$ which powerline
/usr/local/bin/powerline
```

### [#2] Font Installation (system-wide)

Powerline 과 함께 설치되는 font 는 **programming 에 유명한 monospace font** 는 다 들어 있네요. 이들 좋은 font 가 모두 모아서 powerline tool 에서 제공해준다는 것이 이들 개발자도 **font 를 중요하게 생각**하고 있는 것 같아서 동질감(?)을 느끼게 되어 좋군요. :)

* monospace
* Droid  Sans Mono
* DejaVu Sans Mono
* Envy Code R
* Inconsolata
* Lucida Console
* Monaco
* Pragmata
* PragmataPro
* Menlo
* Source Code Pro
* Consolas
* Anonymous pro
* Bitstream Vera Sans Mono
* Liberation MonoUbuntu Mono

```bash
wget https://github.com/Lokaltog/powerline/raw/develop/font/PowerlineSymbols.otf https://github.com/Lokaltog/powerline/raw/develop/font/10-powerline-symbols.conf
sudo mv PowerlineSymbols.otf /usr/share/fonts/
sudo fc-cache -vf
sudo mv 10-powerline-symbols.conf /etc/fonts/conf.d/
```

### [#3] Bash config
Add the following line to your `~/.bashrc` or `/etc/bash.bashrc`:

```bash
if [ -f /usr/local/lib/python2.7/dist-packages/powerline/bindings/bash/powerline.sh ]; then
    source /usr/local/lib/python2.7/dist-packages/powerline/bindings/bash/powerline.sh
fi
```

### [#4] Vim config
Add following to `/etc/vim/vimrc`: 

```bash
set rtp+=/usr/local/lib/python2.7/dist-packages/powerline/bindings/vim/

" Always show statusline
set laststatus=2

" Use 256 colours (Use this setting only if your terminal supports 256 colours)
set t_Co=256
```

### [#5] Tmux config
Add the following line to your ~/.tmux.conf:

```bash
source /usr/local/lib/python2.7/dist-packages/powerline/bindings/tmux/powerline.conf
set-option -g default-terminal "screen-256color"
```

### [#6] Hard core config

세부 configuration 좀더 글을 읽어본 이후에... 

* [Configuration @https://powerline.readthedocs.org/](https://powerline.readthedocs.org/en/latest/configuration.html#)


이를 반영하고 나면 다음과 같이 **statusline 이 예쁘게** 변경된 것을 볼 수 있다.

![img](https://raw.github.com/tkhwang/tkhwang-etc/master/images/powerline.png)


만약 마음에 들지 않아서 uninstall 을 하고 싶다면...

### [#Z] Uninstall

```bash
$ sudo pip uninstall powerline
```






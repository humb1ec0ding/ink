---
layout: post
title: "A few personal setting after installing Linux 12.04"
date: 2013-03-03 15:14
comments: true
categories: etc
tags: ['linux','config']
---

A few personal setting after installing Linux 12.04 **just** for my later preferences.

<!--more-->

### Tool

* [Emacs24](https://launchpad.net/~cassou/+archive/emacs)
* [Sublime Text 2](http://www.sublimetext.com/2) : Text editor mostly for browsing the code.
* [Terminator](https://apps.ubuntu.com/cat/applications/precise/terminator/) : Multiple terminals in one window
* [playonlinux](http://freshtutorial.com/install-itunes-ubuntu-linux/) : Windows app emulation in Linux. For iTunes usage.

### System configuration
* [Classic menu indicator](http://www.liberiangeek.net/2012/05/install-classic-menu-indicator-in-ubuntu-12-04-precise-pangolin/) : Legacy Gnome like menu bar.
* [Nabi](http://hwanud.egloos.com/2875145) : Korean input IME
* Put Korean input IME in the try : [Ubuntu 한글 입력기 nabi 트레이에 넣기](http://whatwant.tistory.com/374)
* [Change the cursor and its size.](http://askubuntu.com/questions/126491/how-do-i-change-the-cursor-and-its-size)

### Font setting
* Download [monaco font](http://jorrel.googlepages.com/Monaco_Linux.ttf), [Naver Nanum font](http://hangeul.naver.com/font)
* Install

```bash
sudo mkdir /usr/share/fonts/truetype/custom/
sudo cp *.ttf /usr/share/fonts/truetype/custom/
sudo fc-cache -f -v
```

### Color theme : Solarized Dark

* [Solarized](http://ethanschoonover.com/solarized) : I like Solarized Dark color theme.

![img](http://ethanschoonover.com/solarized/img/solarized-vim.png)

* Emacs : [prelude](https://github.com/bbatsov/prelude)
* Terminator : [ghuntley / terminator-solarized @gitHub](https://github.com/ghuntley/terminator-solarized)
* Sublime : font and theme (solarzied dark) configuration

```bash
Preferences > Color scheme > Solarized (Dark)
Preferences > File Settings - User

{
  "color_scheme": "Packages/Color Scheme - Default/Solarized (Dark).tmTheme",
  "font_face": "Monaco",
  "font_size": 10
}
```

* [Gnome terminal](https://github.com/sigurdga/gnome-terminal-colors-solarized)

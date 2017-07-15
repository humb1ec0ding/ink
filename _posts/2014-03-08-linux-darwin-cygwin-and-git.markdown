---
layout: post
title: "linux darwin cygwin 그리고 git"
date: 2014-03-08 16:27
comments: true
categories: etc
tags: ['config']
---

요즘 많이들 그렇지만 어찌하다보니 컴 환경이 Ubuntu (`Linux`), MAC OS X (`Darwin`), Windows (`Cygwin`) 이 되어 모두 linux 을 이용할 수 있는 환경이 되었다.

아직 내가 하는 programming 이라해봐야 너무나도 `humbleCoding` 한 정도라서 Android 의 `java` app programming 과 `python`,`bash` shell programming 정도니깐 이 세 환경에서 자유롭게 작성하고, 실행시키고 싶었다.

Java 의 경우에는 원래 JVM 위에서 동작을 하니 platform 의존도가 없고, 개발환경도 Java 랑 IDE (eclipse 나 android studio) 만 설치되어 돌아가니 별 문제 없고, python 과 bash 기본적인 linux 환경인 Ubuntu, Mac OS X, Cygwin 에서는 돌아가니 큰 문제없이 동작할 것 같았다.

<!--more-->

그런데, 문제는 `adb` 부터 시작해서 bash 에서 실행하는 실행 파일이 platform 의존하는 경우가 많아서 간단한 script 이지만 이를 `ubuntu`, `mac os x`, `cygwin` 모두 돌게 하려고 하니 귀찮은 점들이 많이 있었다. 

간단한 script 이긴 하지만 platform dependent 한 부분을 abstract 해서 common 한 interface 로 동작하도록 정리를 했는데, 나름 편하다~~~

```python
class Executor():
        
    os        = os.name.lower()
    platform  = Platform.system().lower().split('_')[0]
            
    if self.platform   == 'linux':
    ...
    elif self.platform == 'darwin':
    ...
    elif self.platform == 'cygwin':
```


```bash
PLATFORM="$OSTYPE"
if [ $PLATFORM = 'linux-gnu' ]; then
elif [ $PLATFORM = 'darwin13' ]; then
elif [ $PLATFORM = 'cygwin' ]; then
fi
```

또한 `git` 에 조금씩 익숙해지고 있어서 personal project code 들을 GitHub 이나 bitbucket 에 관리를 하니 `ubuntu`, `mac os x`, `cygwin` 어디에서든 이용, 실행, 수정하면서 변경 시에는 git 통하여 밀어 넣고, 다른 편에서 이를 받아서 업데이트하고 받아서 이어서 작업을 할 수 있다. 

`git` 아직은 branch, rebase 도 잘 사용 못하지만 그래도... 조금씩 알아감에 따라서 점점 더 편하고, 좋아진다. 

```bash
git clone

git add
git commit -m 
git push

git lg     
git pull
```

* [git log](http://fredkschott.com/post/2014/02/git-log-is-so-2005/)
* [tig : Text-mode interface for Git](https://github.com/jonas/tig)

[이 글](https://www.atlassian.com/git/workflows#!workflow-centralized) 보니 아직 [centralized workflow](https://www.atlassian.com/git/workflows#!workflow-centralized) 형식으로 혼자서 사용하고 있는 것 같은데, 빨리 [feature branch workflow](https://www.atlassian.com/git/workflows#!workflow-feature-branch) 적용해서 feature 는 branch 따서 작업하고 merge/rebase 해서 반영하도록 해야겠다.

좋다... :)

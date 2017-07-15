---
layout: post
title: "[book] 폴리글랏 프로그래밍 (임백준 저)"
date: 2014-03-23 22:36
comments: true
categories: book
tags: book
---

* 폴리글랏 프로그래밍 Polyglot programming : 새로운 자바 언어를 기다리는 히치하이커를 위한 안내서
* 임백준 지음 
* My rating 7/10 = ★★★★★ ★★☆☆☆

<!--more-->

![폴리글랏 프로그래밍](https://github.com/tkhwang/tkhwang-etc/blob/master/images/2014/DSC_0235.jpg?raw=true)

사실 그동안 나에게 programming language 라고 하면 kernel, drvier 와 같이 h/w dependency 가 있는 경우에는 `c`, CPU coprocessor 를 access 하거나 bootloader 에서는 `assembler`, Application 은 `C++` 이후에 `java`, `C#`, ...
Stript 언어류인 `python`, `perl`, `javascript`, `Ruby`, ...

그러나 이렇게 많은 프로그래밍 언어들의 특징이 무엇인지 ? 왜 있는지도 잘 모른다. 사실 이렇게 다양한 언어가 왜 존재하고, 각 언어도 revision 을 하면서 발전을 할텐데 어떻게 발전을 하고 있는지 ? 무엇을 위하여 발전을 하는지 ? 에 대해서 제대로 생각을 해보지 못한 것 같다. 아니 그런 고민 할 필요없이 각 분야에 맞는 언어가 애초에 있어고, 그 중의 하나를 사용하면 되는 것이라고 생각해온 것이 사실일 것 같다.

그러다가 `python` 을 사용을 해보면서 정말 편리하게 programming 할 수 있다는 것을 알게되고, 그 편리함이 `c` 로 구현하기 위해서는 어렵게 많이 구현을 해야할 사항들이`python` 에서는 단 몇 줄만으로 기능 구현이 되는 것을 보고 **programming language 에서의 abstraction 을 높이고, 이에 따른 결과로서 개발자가 참 쉽게 구현을 하게 되는 것** 같다는 것을 조금씩 알아가고 있는 상황이다. 

이름만으로 신뢰를 주는 이름이 있다. 
소프트웨어에는 **임백준**, **박재호** 씨가 그렇다. 이분들이 낸 책이라면 거의 대부분 주저하지 않고 읽어보려고 하고 있고, 그렇게 최근 **임백준**씨가 낸 폴리글랏 프로그래밍이라는 이상한 이름의 책을 읽어 되었다.

 책은 코볼언어가 어떻게 역사속으로 사라졌는지를 이야기하면서 시작하고 있다. 이후 자바 언어적인 이야기를 하면서 Generic, 람다, 클로저, 연산자 오버로딩과 같은 항목에 대해서 언어적 특성을 이야기하면서 이후 이를 `C#`, `Scala` 까지 진행하면서 이야기한다.

	모든 언어의 '발전'은 추상수준을 상승시켜서 프로그래머가 작성해야하는 행사코드의 분량을 줄이는 방향으로 움직인다는 것. 다른 것은 몰라도 이 명제만큼은 잘 기억하기 바란다. 최근 관심을 끌고 있는 스칼라나 F# 같은 함수형 프로그래밍 언어, 웹 개발 과정에서 널리 사용되는 루비나 파이썬 같은 동적 언어, 새로 발표된 C++ 언어의 표준이 모두 마찬가지다. 그들은 모두 추상수준을 한 단계 상승시켜서 전보다 압축적인 표현이 가능하도록 만드는 방향으로 질주하며 경쟁을 펼치고 있다.

 표현의 간결성, 프로그래머의 의도가 명확히 단순 명료하게 표현할 수 있는 형식, 이를 위하여 꼭 필요한 행사 코드는 가능하면 줄이도록 하는 것이 현재 프로그래밍 언어의 발전 방향이라는 것이다. 내가 느낀 `python` 이 왠지 편한다는 것이 바로 이런 이유가 아니었을까 ? 아는 만큼 보인다고... 항상 대단하다 생각하는 [+Jake Wharton](https://plus.google.com/+JakeWharton/posts) 의 최근을 보니깐 **Java 8** 의 **람다** 기능에 대해서 많은 관심을 가지고 있는 듯 하는군요.

* [+Jake Wharton's G+ posting](https://plus.google.com/108284392618554783657/posts/81XiXEzEBLN)

```java
ImmutableSet<String> items = IntStream.rangeClosed(1, 10)
    .mapToObj(x -> "Item " + x)
    .collect(GuavaCollectors.immutableSet());
```

* [Programming Languages Influence Network](http://exploringdata.github.io/vis/programming-languages-influence-network/)

![Programming Languages Influence Network](https://github.com/tkhwang/tkhwang-etc/blob/master/images/2014/prgramming_language_network.jpg?raw=true)

Generic 이라는 표현식. 람다라는 표현식이 언어적으로 왜 중요한지... `java` 와 `c++`, `c#` 이 어떻게 서로 다른지.. 언어는 어떻게 발전을 해가고 있는지  일깨워준 책이다. 그동안 나에게 **블랙박스로서 고정된 Programming langugage가 계속 발전해나가고 있는 살아있는 생물로 느끼도록 나의 사고를 넓혀준 책**이다. :)

<div class="ttbReview"><table><tbody><tr><td><a href="http://www.aladin.co.kr/shop/wproduct.aspx?ISBN=8968480869&amp;ttbkey=ttbtkhwang2131007&amp;COPYPaper=1" target="_blank"><img src="http://image.aladin.co.kr/product/3720/43/cover/8968480869_1.jpg" alt="" border="0"/></a></td><td align="left"  style="vertical-align:top;"><a href="http://www.aladin.co.kr/shop/wproduct.aspx?ISBN=8968480869&amp;ttbkey=ttbtkhwang2131007&amp;COPYPaper=1" target="_blank" class="aladdin_title">폴리글랏 프로그래밍</a> - <img src="http://image.aladin.co.kr/img/common/star_s10.gif" border="0" alt="10점" /><br/>임백준 지음/한빛미디어(한빛아카데미)</td></tr></tbody></table></div>
---
layout: post
title: "[Android][rooted] ProxyDroid 와 Burpsuite 이용한 android 단말의 네트워크 분석"
date: 2014-01-13 22:30:47 +0900
comments: true
categories: etc
tags: ['android','hacking']
---

단말에서 app assessment 를 하다보면 항상 필요한 **network analysis**. 

매번 proxy configuration 때문에 힘들어 하곤 했는데 interpidus group 에 [burpsuite](http://www.portswigger.net/index.html)와 [proxyDroid](https://play.google.com/store/apps/details?id=org.proxydroid&hl=ko) ([src](https://github.com/madeye/proxydroid)) 앱을 이용하여 특정 앱의 패킷만을 분석하는 방법까지 잘 정리된 글이 있어서 따라하면서 정리해본다. **SSL** 까지 MiTM 할 수 있다니... 좋다. :)

* [Network Analysis With ProxyDroid, BurpSuite, and Hipster Dog @Interpidus Group, July 1, 2012](https://intrepidusgroup.com/insight/2012/07/network-analysis-with-proxydroid-burpsuite-and-hipster-dog/)

<!--more-->

## ProxyDroid 

ProxyDroid 매우 좋음... 단, 이를 이용하기 위해서는 **root access** 가 필요함. 즉, 단말 루팅을 해야함.  <br>
좋은 점은...

* 단말 전체가 아니라 **특정 앱** 만을 타켓할 수 있음.
* 설정 및 실행을 매우 빨리 할 수 있음.

글쓴이의 환경은 컴퓨터 IP 는 `192.168.1.146` 이고, 여기에 WiFi 가 연결되어 있으며, 단말을 WiFi 을 통하여 여기에 연결하였음. 

#### @ProxyDroid : PROXY SETTINGS > Host 설정 

아직 설정은 안 했으니깐 ProxySwitch 는 꺼짐으로 disable proxy 인 상태에서 **PROXY SETTINGS** Host 설정 
Host 에서 Burp Suite 를 실행하고, Proxy > Options > Proxy Listeners 에서 host IP 설정하기. 

![img](http://intrepidusgroup.com/insight/wp-content/uploads/2012/06/Screenshot_2012-06-26-18-09-27-e1340750384933-300x158.png)

#### @ProxyDroid : PROXY SETTINGS > Port 설정 

![img](http://intrepidusgroup.com/insight/wp-content/uploads/2012/06/Screenshot_2012-06-26-18-10-49-e1340750793555-300x248.png)


#### @ProxyDroid : FEATURE SETTINGS > Global Proxy

Device 의 모든 packet 을 MiTM

#### @ProxyDroid : FEATURE SETTINGS > Individual Proxy

특정 앱만 MiTM 할 경우 선택.  이 옵션은 단지 선택한 앱의 `UID` 을 기초로하여 **iptables** 을 사용한다고 함. <br>
(network 이나 iptable 에 대해서 잘 몰라서 얼마나 cool 한 것인지 잘 모르겠지만 특정 앱만을 MiTM 할 수 있다면 편하긴 할 듯.)

![img](http://intrepidusgroup.com/insight/wp-content/uploads/2012/06/Screenshot_2012-06-26-18-11-43-e1340751006249-300x227.png)

#### @ProxyDroid : Proxy Switch > Enable Proxy

설정값을 Profile 로 저장해두면 나중에는 동일한 설정값을 profile 로 불러와서 Proxy Switch 버튼만 눌러서 사용할 수 있다.  Enable 시키면 아래와 같이 메시지 표시되면서 동작한다. 

![img](http://intrepidusgroup.com/insight/wp-content/uploads/2012/06/Screenshot_2012-06-26-18-14-10-e1340751415600-300x235.png)


## BurpSuite 

이제는 **BurpSuite** 설정. <br>
만약 지금까지 burpSuite 설정 해보지 않았다면... 이에 대해서 자세하게 설명한 글 매우 많으니깐... 다시 읽고 오고...

여기서는  **BurpSuite** 를 proxy 로 사용해본 것으로 가정한다. 
한 가지 차이점은 안드로이드는 공식적으로 ProxyDroid 와 같은 글로벌 프록시들을 지원하지 않기 때문에 **SSL conntection** 의 경우에는 문제가 될 수 있다. 

SSL 잘 아는 옆에 있는 Sid 놈이 좀더 설명해준다. 


	그 이유는 말이야...  

	보통 HTTP Proxies 와 연관이 되어 있는 HTTP CONNECT 명렁어는 instagram.com 과 같은 특정 hostname을 포함하는 형태에서 IP 형태로 변경이 되지. BurpSuite 에서 traffic 을 직접보면 보면 말이지... target 으로서 오직 IP 만 찍히는 거랑 같은 이유야. 

일단 여기까지 하고, 잠시 후에 다시 한 번 보자.

그래서 가장 먼저 해야하는 일은 **Burp 의 CA 인증서**를 단말에 설치해야 하는 거. <br>
ICS 부터는 매우 쉽고, Gingerbread 이전에는 이전 방식으로 해야 하는데, ICS 보다는 좀 귀찮은 듯.

BurpSuite 인증서를 뺴내는... 내가 아는 가장 쉬운 방법은 브라우저를 사용해서 **export** 하는 거다.<br>
만약 잘 모르겠다면 [이거](http://www.portswigger.net/burp/help/proxy_options.html#listeners_cert) 읽어봐라. 

![img](http://intrepidusgroup.com/insight/wp-content/uploads/2012/06/portswigger_ca-e1340807365362-132x300.png)

#### @HostPC : Burp CA  인증서 설치 & 인증서 export

(tkHWANG) SSL 통신 사이에 BurpSuite 이용하는 경우에 proxy 로 중간에서 packet 을 바꿔치기 하니깐 단말과 proxy 사이에 원래 서버 인증서가 아니라
burpSuite 인증서로 패킷이 날라올텐데 이 경우에 SSL 에러가 나지 않도록 처리하기 위해서 BurpSuite 인증서를 믿을 수 있는 인증서로 추가해야하는 듯 하다. 
그래서 먼저 브라우저에서 Burp CA 인증서를 설치되고 하고 나서, 이를 export 해서 단말에 심는 것 같다. 

[Installing Burp's CA Certificate: IE](http://www.portswigger.net/burp/help/proxy_options_installingCAcert.html#IE)

* (IE는 admin 권한으로 실행 후 )Proxy 를 burpSuite 가 되도록 proxy 설정 : 127.0.0.1:8080
* 아무거나 (https://www.google.co.kr/) https 연결하면 **SSL 인증서** error 가 발생.
* 이때 무시하고 연결하고 나서 인증서 확인해보면 Portswigger 인증서 확인할 수 있다. 
* 인증서 확인 후에 export 해서 저장해둔다.
* 이 Portswigger 인증서를 브라우저 인증서로 import 하여 설치.


#### @단말 : Settings > 일반 > 보안 > 자격증명 저장소 > 신뢰할 수 있는 인증서 

이후에는 BurpSuite 의 인증서 CA 를 단말에 **추가** 시켜야 한다. <br>
다시 말하면 앞에서 브라우저에서 export 한 Portswigger file 을 단말에 넣고, Security Setting 메뉴를 이용해서 **import** 시킨다. 이것도 잘 모르겠다면 [여기](https://intrepidusgroup.com/insight/2011/12/mit/) 를 참고한다. 

(tkHWANG) 이것도 처음에는 헷깔렸다.

* 위에서 브라우저에서 export 해서 저장해둔 Portsigger 인증서를 단말을 내장메모리 root 에 저장.
* Settings > 일반 > 보안 > 자격증명 저장소 > 신뢰할 수 있는 인증서 선택하면 인증서 실치된다. 


#### @BurpSuite : Proxy > Options > Proxy Listeners > Edit > Binding

* Bind to address : **Loopback only** 로 설정하면 절대 안 됨. 자신의 host IP 로 변경해야 한다. 
* Host IP : 예제는 192.168.1.146
* Host Port : 8080 

![img](http://intrepidusgroup.com/insight/wp-content/uploads/2012/06/burp1-e1340752087930-300x145.png)


#### @BurpSuite : Proxy > Options > Proxy Listeners > Edit > Ceritificate

여기가 많이들 실수하는 부분이란다. <br>
기본 option 인 두번째 옵션인 **Generate CA-signed-per-host certificate** 을 사용해서는 안 된다. 

앞에서 이야기한 것과 같이 연결하려고 하는 **hostname 을 명시적으로 사용**해야한다. 만약 SSL 을 통하여 연결하고자 하는 호스트를 모른다면 `packet capture tool`을 이용해서 connection 을 **passively** sniff 하거나 
app 을 reverse 해서 코드 안의 호스트 이름을 찾아야 한다. 

만약 서로 다른 SSL 서버를 연결하려고 하는 앱이 있다고 한다면 listener 를 독립된 Lienster 를 여러개 설정해서 사용해야 한다. <br>
**Generate a CA_signed ceritificate with a specific hostname** 선택하여 `instagram.com` 을 입력해서 MiTM 해보자. 

![img](http://intrepidusgroup.com/insight/wp-content/uploads/2012/06/burp5-e1340752597427-300x199.png)


#### Result

일단 위의 가이드에 따라서 설정한 후 기본적인 동작은 되는 듯 하다. 특정 앱의 packet 만 잡는 경우에는 BurpSuite 의 certificate 옵션 에서 
`Generate a CA-signed certificate with a specific hostname` 에서 정확한 hostname 을 작성해야 하는 듯 하다. 

서버 주소가 일반적인 앱과 다른 경우가 많아서 이 경우에는 아래 hint 가 도움이 될 듯 하다. 

	If you don’t know the host that it’s connecting to over SSL, you’re going to have to either sniff the connection passively using a packet capture tool (Hint: check out DNS requests), or reverse the app and look for the host names inside the code.  If you have an app that is making connections to different SSL hosts, you’re going to have to setup separate listeners.

어찌되었든 단말 앱의 packet capture 할 수 있게 되었다. 좀더 차분히 해보자... :)

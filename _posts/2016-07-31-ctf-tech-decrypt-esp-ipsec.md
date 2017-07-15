---
layout: post
title: "[CTF skill] Decrypt ESP, IPSec"
date: 2016-07-31 22:40:00
category: ctf
tags:  [crypto, wireshark]
---

[Trend Micro CTF 2016](https://ctf.trendmicro.co.jp/)의 IoT/Network 100점 문제로 나왔던 문제다. 

```
src 1.1.1.11 dst 1.1.1.10
	proto esp spi 0xfab21777 reqid 16389 mode tunnel
	replay-window 32 flag 20
	auth hmac(sha1) 0x11cf27c5b3357a5fd5d26d253fffd5339a99b4d1
	enc cbc(aes) 0xfa19ff5565b1666d3dd16fcfda62820da44b2b51672a85fed155521bedb243ee
src 1.1.1.10 dst 1.1.1.11
	proto esp spi 0xbfd6dc1c reqid 16389 mode tunnel
	replay-window 32 flag 20
	auth hmac(sha1) 0x829b457814bd8856e51cce1d745619507ca1b257
	enc cbc(aes) 0x2a340c090abec9186c841017714a233fba6144b3cb20c898db4a30f02b0a003d
src 1.1.1.10 dst 1.1.1.11
	proto esp spi 0xeea1503c reqid 16389 mode tunnel
	replay-window 32 flag 20
	auth hmac(sha1) 0x951d2d93498d2e7479c28c1bcc203ace34d7fcde
	enc cbc(aes) 0x6ec6072dd25a6bcb7b9b3b516529acb641a1b356999f791eb971e57cc934a5eb
src 1.1.1.11 dst 1.1.1.10
	proto esp spi 0xd4d2074d reqid 16389 mode tunnel
	replay-window 32 flag 20
	auth hmac(sha1) 0x100a0b23fc006c867455506843cc96ad26026ec0
	enc cbc(aes) 0xdcfbc7d33d3c606de488c6efac4624ed50b550c88be0d62befb049992972cca6
```

<!--more--> 

- ESP packet decrypt 방법 설명 : [How can I decrypt IKEv1 and/or ESP packets ? - Wireshark Q&A](https://ask.wireshark.org/questions/12019/how-can-i-decrypt-ikev1-andor-esp-packets)
- TrendMicro CTF 2015 IoT/Misc 100pt write-up: [[Trend Micro CTF 2016 Online Quals] – Misc 100pts – crypto and something else](https://quandqn.wordpress.com/2016/07/31/trend-micro-ctf-2016-online-quals-misc-100pts/)

위의 msg 는 `sudo ip xfrm state`을 통하여 IPsec에 대한 configuration 값인데 이를 wiresharek 에 잘 설정을 해주면 이를 이용하여 암호화된 payload 를 wireshark 에서 자동으로 decrypt 해준다.

![Decrypt ESP/IPSec in wireshark](https://raw.githubusercontent.com/humb1ec0ding/humb1ec0ding-etc/master/2016/07/decrypt-ipsec-esp.png)

```
src 1.1.1.11 dst 1.1.1.10
	proto esp spi 0xfab21777 reqid 16389 mode tunnel
	replay-window 32 flag 20
	auth hmac(sha1) 0x11cf27c5b3357a5fd5d26d253fffd5339a99b4d1
	enc cbc(aes) 0xfa19ff5565b1666d3dd16fcfda62820da44b2b51672a85fed155521bedb243ee
```

-  Protocol: `IPv4`
-  Src IP: `1.1.1.11`
-  Dest IP: `1.1.1.10`
-  SPI:  `0xfab21777`
-  Encryption: `AES-CBC`
-  Encryption Key: `0xfa19ff5565b1666d3dd16fcfda62820da44b2b51672a85fed155521bedb243ee`
-  Authentication: `HMAC-SHA-1`
-  Authentication Key: `0x11cf27c5b3357a5fd5d26d253fffd5339a99b4d1`

Preference > Protocol > Esp > Attempt to detect /decode encrypted ESP payloads 

선택하고 나면 위에 설정에 따라서 decrypt 해준다.
그러면 기존 암호화되어 보이지 않던 packet 이 보이게 되므로 이후 복호화된 packet 을 좀더 분석을 하면 된다.







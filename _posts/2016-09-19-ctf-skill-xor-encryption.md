---
layout: post
title: "[CTF skill] XOR Encryption"
date: 2016-09-19 23:20:00법
description: "CTF Tool : pwntools XOR encryption"
category: ctf
tags:  [xor]
---

이번 CSAW 2016 에 나왔던 Crypto 50짜리 문제이다.

```
Only true hackers can see the image in this magic PNG....

nc crypto.chal.csaw.io 8000

Author: Sophia D'Antoine
```

- [[CSAW 2016] Sleeping Guard Writeup – Megabeets](http://www.megabeets.net/csaw-2016-sleeping-guard-writeup/)
- [CTF/CSAW/2016 at master · grocid/CTF](https://github.com/grocid/CTF/tree/master/CSAW/2016#sleeping-guard-50-p)
- [ctf-notes/crypto-50-sleeping_guard.md at master · 73696e65/ctf-notes](https://github.com/73696e65/ctf-notes/blob/master/2016-ctf.csaw.io/crypto-50-sleeping_guard.md)

<!--more--> 

## Server code

서버 코드에서는 PNG 파일을 `base64.encode(image)` 한 것만 나와 있다.
`length_encryption_key()` 를 통해서 12 byte key length 를 알려 주고 있지만 encrpytion 관련된 코드는 없다.

```python
import base64
from twisted.internet import reactor, protocol
import os

PORT = 9013

import struct
def get_bytes_from_file(filename):  
    return open(filename, "rb").read()  
    
KEY = "[CENSORED]"

def length_encryption_key():
    return len(KEY)

def get_magic_png():
    image = get_bytes_from_file("./sleeping.png")
    encoded_string = base64.b64encode(image)
    key_len = length_encryption_key()
    print 'Sending magic....'
    if key_len != 12:
        return ''
    return encoded_string 
    

class MyServer(protocol.Protocol):
    def connectionMade(self):
        resp = get_magic_png()
        self.transport.write(resp)

class MyServerFactory(protocol.Factory):
    protocol = MyServer

factory = MyServerFactory()
reactor.listenTCP(PORT, factory)
reactor.run()
```

## PNG file signature

```bash
$ hexdump -C ./test.png |head -1
00000000  89 50 4e 47 0d 0a 1a 0a  00 00 00 0d 49 48 44 52  |.PNG........IHDR|
```


## Base64 decode

서버로부터 data 를 받아서 base64 decode 를 한 결과는 역시나 PNG 파일 형태가 아니다.

```bash
$ hexdump -C ./b64d-1.bin |head -1
00000000  de 3f 0f 2f 52 4b 45 41  65 79 21 32 1e 27 05 3a  |.?./RKEAey!2.'.:|
```

## Which encryption ?

처음에는 key size 가 12 라서 AES encryption 일까해서 brute-force 를 할까 했는데 12자는 너무 길다.
다들 어찌 알았는지 XOR encryption 이라 생각을 하고 가장 쉬운데서 출발하는 듯 하다.

## XOR Encryption

```
out = input XOR key
key = input XOR out
```

## `pwn.xor()`

```
pwnlib.util.fiddling.xor(*args, cut = 'max') → str[source]
Flattens its arguments using pwnlib.util.packing.flat() and then xors them together. If the end of a string is reached, it wraps around in the string.

Parameters:	
args – The arguments to be xor’ed together.
cut – How long a string should be returned. Can be either ‘min’/’max’/’left’/’right’ or a number.
Returns:	
The string of the arguments xor’ed together.
```

pwntools 의 xor 을 써보자.

```python
In [1]: with open('./b64d-1.bin','r') as f_in:
   ...:     input = f_in.read(16)

In [2]: input
Out[2]: "\xde?\x0f/RKEAey!2\x1e'\x05:"

In [3]: with open('./test.png','r') as f_out:
   ...:     out = f_out.read(16)

In [4]: out
Out[4]: '\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR'

In [5]: import pwn

In [6]: pwn.xor(input, out)
Out[6]: 'WoAh_A_Key!?WoAh'
```

키 사이즈는 12라고 했으니깐 key를 추출해서...

```python
In [10]: key = pwn.xor(input, out)[:12]


In [11]: key
Out[11]: 'WoAh_A_Key!?'
```

`pwn.xor()` 로 decrypt 해보면...

```python
In [15]: with open('./b64d-1.bin','r') as f_in:
    ...:     with open('./decrypted.png','w') as f_out:
    ...:         input = f_in.read()
    ...:         data = pwn.xor(input, key)
    ...:         f_out.write(data)
```    

![img](https://raw.githubusercontent.com/humb1ec0ding/humb1ec0ding-etc/master/2016/09/csaw-xor-encryption.png)

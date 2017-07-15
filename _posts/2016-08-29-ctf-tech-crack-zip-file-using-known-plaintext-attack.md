---
layout: post
title: "[CTF skill] Cracking ZIP Files (Known-Plaintext Attack)"
date: 2016-08-29 22:40:00
categories: ctf
tags:  [crypto, zip]
---

CTF(x) 2016 에 나온 zip file crack 하는 문제이다.

[ctf-notes/Forensics-100-password.txt at master · 73696e65/ctf-notes · GitHub](https://github.com/73696e65/ctf-notes/blob/master/2016-ctfx/Forensics-100-password.txt)

<!--more--> 

Zip file 하나는 john the ripper 를 이용해서 알려진 password wordlist 를 이용해서 쉽게 crack 이 되었는데, 두 번째 zip file 은 crack 되지 못하여 문제를 풀지 못하였다가 끝난 이후에 write-up 보고 **known-plaintext attack** 이라는 방법을 알게 되어 write-up 보면서 해보자.

```bash
$ ./pkcrack 
Usage: ./pkcrack -c <crypted_file> -p <plaintext_file> [other_options],
where [other_options] may be one or more of
 -o <offset>    for an offset of the plaintext into the ciphertext,
                        (may be negative)
 -C <c-ZIP>     where c-ZIP is a ZIP-archive containing <crypted_file>
 -P <p-ZIP>     where p-ZIP is a ZIP-archive containing <plaintext_file>
 -d <d-file>    where d-file is the name of the decrypted archive which
                will be created by this program if the correct keys are found
                (can only be used in conjunction with the -C option)
 -i     switch off case-insensitive filename matching in ZIP-archives
 -a     abort keys searching after first success
 -n     no progress indicator
```



Zip file 안의 내용 중에서 `plaintextname` 과 `ciphertextname` 을 모두 아는 경우에는 이 내용을 기반으로 `encrypted-zip` 을 `plaintext-zip` 을 만들 수 있는 tool 인 [pkcrack](https://www.unix-ag.uni-kl.de/~conrad/krypto/pkcrack.html) 이용한다.

```
$ pkcrack            \
	-C encrypted-ZIP  \
	-c ciphertextname \
	-P plaintext-ZIP  \
	-p plaintextname  \
	-d decrypted_file -a
```

`Evelyn Davis.zip` 파일 안의 내용에 `Evelyn Davis.vcf` 로부터 `Ryan King.zip` 파일 안의 `Ryan King.vcf`의 plaintext 값을 유추하여 이를 통하여 known-plaintext attack을 시도한다.

#### `Evelyn Davis.vcf` 내용으로부터 `Ryan King.vcf` 유추 생성

`Evelyn Davis.zip` 안의 `Evelyn Davis.vcf`  내용을 참고로 하여 동일한 형식으로 이름만 변경하여 `Ryan King.vcf` 을 생성.

#### `Ryan King.vcf` 크기 비교

`Encrypted-zip` 에서 확인한 `ciphertextname` 인 `Ryan King.vcf` 의 크기는 `146` bytes.

```bash
$ unzip -l "Ryan King.zip"
Archive:  Ryan King.zip
  Length     Date   Time    Name
 --------    ----   ----    ----
      146  07-24-16 19:19   Ryan King.vcf
   100990  07-27-16 22:27   signature.png
 --------                   -------
   101136                   2 files
```

앞에서 유추하여 생성한 `Ryan King.vcf` 파일 사이즈 역시 동일하게 `146` bytes.

```bash
$ ls -al "Ryan King.vcf"
-rw-r--r--@ 1 tkhwang  staff  146 Aug 29 22:50 Ryan King.vcf
```

 #### `plaintextname` 으로부터 `ciphertextname` 생성

```bash
$ zip archive.zip 'Ryan King.vcf'
  adding: Ryan King.vcf (deflated 16%)
```

## Putting all together : 공격

우아... 진짜 풀린다... :)

```bash
$ ./pkcrack -C Ryan\ King.zip -c Ryan\ King.vcf -P archive.zip -p Ryan\ King.vcf -d decrypted.zip -a
Files read. Starting stage 1 on Mon Aug 29 23:27:42 2016
Generating 1st generation of possible key2_133 values...done.
Found 4194304 possible key2-values.
Now we're trying to reduce these...
Done. Left with 64012 possible Values. bestOffset is 24.
Stage 1 completed. Starting stage 2 on Mon Aug 29 23:27:49 2016
Strange... had a false hit.
Ta-daaaaa! key0=86cdf919, key1=bd44c60c, key2=60dbe8f7
Probabilistic test succeeded for 114 bytes.
Strange... had a false hit.
Strange... had a false hit.
Strange... had a false hit.
Strange... had a false hit.
Strange... had a false hit.
Stage 2 completed. Starting zipdecrypt on Tue Aug 30 00:09:24 2016
Decrypting Ryan King.vcf (be2570e236508bf4c50b6b92)... OK!
Decrypting signature.png (0d296646595805d826ba79ab)... OK!
Finished on Tue Aug 30 00:09:24 2016
```

Flag 는 `signature.png` 파일에 존재...

![flag](https://raw.githubusercontent.com/humb1ec0ding/humb1ec0ding-etc/master/2016/08/plaintext-zip-attack.png)


---
layout: page
title: libc.so.6 version `GLIBC_2.34' not found
description: libc.so.6 version `GLIBC_2.34' not found
keywords: desktop, linux, libc.so.6 version `GLIBC_2.34' not found
permalink: /desktop/linux/ubuntu/libs/
---

# libc.so.6: version `GLIBC_2.34' not found

<br/>

Делаю!  
2025.03.10

<br/>

```
$ cat /etc/os-release
NAME="Linux Mint"
VERSION="20.3 (Una)"
ID=linuxmint
ID_LIKE=ubuntu
PRETTY_NAME="Linux Mint 20.3"
VERSION_ID="20.3"
HOME_URL="https://www.linuxmint.com/"
SUPPORT_URL="https://forums.linuxmint.com/"
BUG_REPORT_URL="http://linuxmint-troubleshooting-guide.readthedocs.io/en/latest/"
PRIVACY_POLICY_URL="https://www.linuxmint.com/"
VERSION_CODENAME=una
UBUNTU_CODENAME=focal
```

<br/>

```
$ ./postman
./postman: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.34' not found (required by ./postman)
```

<br/>

```
$ ldd -v ./postman
./postman: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.34' not found (required by ./postman)
	linux-vdso.so.1 (0x00007ffcbd7a1000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f87d6c30000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f87d6e3c000)

	Version information:
	./postman:
		libc.so.6 (GLIBC_2.3) => /lib/x86_64-linux-gnu/libc.so.6
		libc.so.6 (GLIBC_2.2.5) => /lib/x86_64-linux-gnu/libc.so.6
		libc.so.6 (GLIBC_2.4) => /lib/x86_64-linux-gnu/libc.so.6
		libc.so.6 (GLIBC_2.34) => not found
	/lib/x86_64-linux-gnu/libc.so.6:
		ld-linux-x86-64.so.2 (GLIBC_2.3) => /lib64/ld-linux-x86-64.so.2
		ld-linux-x86-64.so.2 (GLIBC_PRIVATE) => /lib64/ld-linux-x86-64.so.2
```

<br/>

```
$ apt policy libc6
libc6:
  Installed: 2.31-0ubuntu9.9
  Candidate: 2.31-0ubuntu9.9
```

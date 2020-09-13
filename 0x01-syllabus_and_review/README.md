## [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/)

### Level 0

```bash
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
# initial ssh password: bandit0

$ ls
readme
$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

### Level 1

```bash
$ ssh bandit1@bandit.labs.overthewire.org -p 2220

$ ls
-
$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

参考：[How to open a f “-” dashed filename using terminal?](https://stackoverflow.com/questions/42187323/how-to-open-a-f-dashed-filename-using-terminal)

### Level 2

```bash
$ ssh bandit2@bandit.labs.overthewire.org -p 2220

$ ls
spaces in this filename
$ cat "spaces in this filename"
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

### Level 3

```bash
$ ssh bandit3@bandit.labs.overthewire.org -p 2220

$ ls -l
total 4
drwxr-xr-x 2 root root 4096 May  7 20:14 inhere

$ cd inhere/
$ ls -a
.  ..  .hidden

$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

### Level 4

```bash
$ ssh bandit4@bandit.labs.overthewire.org -p 2220

# To find the only human-readable file
$ file $(find inhere/ -name "-file0*")
inhere/-file01: data
inhere/-file00: data
inhere/-file06: data
inhere/-file03: data
inhere/-file05: data
inhere/-file08: data
inhere/-file04: data
inhere/-file07: ASCII text
inhere/-file02: data
inhere/-file09: data

$ cat inhere/-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```

### Level 5

```bash
$ ssh bandit5@bandit.labs.overthewire.org -p 2220

# 直接查找大小为 1033 bytes 的文件即可
$ du -ab inhere/ | grep 1033
1033    inhere/maybehere07/.file2

$ cat inhere/maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

### Level 6

```bash
$ ssh bandit6@bandit.labs.overthewire.org -p 2220

# owned by user bandit7, owned by group bandit6
$ find / -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password

$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

### Level 7

```bash
$ ssh bandit7@bandit.labs.overthewire.org -p 2220

# The password is stored in the file data.txt next to the word millionth
$ cat data.txt | grep millionth
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

### Level 8

```bash
$ ssh bandit8@bandit.labs.overthewire.org -p 2220

# The password is stored in the file data.txt and is the only line of text that occurs only once
$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

### Level 9

```bash
$ ssh bandit9@bandit.labs.overthewire.org -p 2220

# The password is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
$ strings data.txt | grep ==
========== the*2i"4
========== password
Z)========== is
&========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

### Level 10

```bash
$ ssh bandit10@bandit.labs.overthewire.org -p 2220

# The password is stored in the file data.txt, which contains base64 encoded data
$ base64 -d data.txt
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```

### Level 11

```bash
$ ssh bandit11@bandit.labs.overthewire.org -p 2220

# The password is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```

参考：[ROT13 - Wikipedia](https://en.wikipedia.org/wiki/ROT13)

### Level 12

```bash
$ ssh bandit12@bandit.labs.overthewire.org -p 2220

# The password is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed
$ mkdir /tmp/chicken
$ cp data.txt /tmp/chicken
$ cd /tmp/chicken

$ xxd -r data.txt > data

# 判断类型并选择对应工具解压
$ file data
data: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
# gzip 识别后缀，因此需要修改后缀名
$ mv data data.gz
$ gzip -d data.gz
$ ls
data  data.txt
$ file data
data: bzip2 compressed data, block size = 900k
$ bzip2 -d data
bzip2: Can’t guess original name for data -- using data.out
$ file data.out
data.out: gzip compressed data, was "data4.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
$ mv data.out data.gz
$ gzip -d data.gz
$ file data
data: POSIX tar archive (GNU)
$ tar -xf data
$ ls
data  data5.bin  data.txt
$ file data5.bin
data5.bin: POSIX tar archive (GNU)
$ tar -xf data5.bin
$ ls
data  data5.bin  data6.bin  data.txt
$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
$ bzip2 -d data6.bin
bzip2: Can’t guess original name for data6.bin -- using data6.bin.out
$ file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
$ tar -xf data6.bin.out
$ ls
data  data5.bin  data6.bin.out  data8.bin  data.txt
$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
$ mv data8.bin data8.gz
$ gzip -d data8.gz
$ file data8
data8: ASCII text
$ cat data8
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

$ cd && rm -r /tmp/chicken
```

### Level 13

The password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level.

**Note: localhost** is a hostname that refers to the machine you are working on

```bash
$ ssh bandit13@bandit.labs.overthewire.org -p 2220
$ ls
sshkey.private
$ ssh -i sshkey.private bandit14@localhost
$ cat /etc/bandit_pass/bandit14
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

参考：[Log in with an SSH private key on Linux and macOS - Rackspace Developer Center](https://docs.rackspace.com/support/how-to/logging-in-with-an-ssh-private-key-on-linuxmac/)

### Level 14

```bash
# 可以使用密码登录或 SSH 免密登录

# The password can be retrieved by submitting the password of the current level to port 30000 on localhost.
$ telnet localhost 30000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr

Connection closed by foreign host.
```

### Level 15

The password can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL encryption.

```bash
$ ssh bandit15@bandit.labs.overthewire.org -p 2220

$ echo BfMYroe26WYalil77FoDi9qh59eK5xNr | openssl s_client -connect localhost:30001 -quiet
# -quiet implicitly turns on -ign_eof as well
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd
```

如果没有使用`-ign_eof`参数，则无法通过管道传递并获得密码。`-ign_eof`防止当输入文件到达文件尾时断开连接<br>
![输出为 DONE](img/ign-eof.jpg)

参考：[s_client(1) - Linux man page](https://linux.die.net/man/1/s_client)

### Level 16

The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

```bash
$ ssh bandit16@bandit.labs.overthewire.org -p 2220

# -sV 给出开放端口的服务或版本信息
$ nmap localhost -p 31000-32000 -sV

Starting Nmap 7.40 ( https://nmap.org ) at 2020-09-13 04:38 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00037s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown     # 除了 31790 端口其他都是 echo
31960/tcp open  echo
1 service unrecognized despite returning data.
...
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 88.37 seconds

$ echo cluFn7wTiGryunymYOu4RcffSxQluehd | openssl s_client -connect localhost:31790 -quiet
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

$ mkdir /tmp/chicken-sshkey
$ cd /tmp/chicken-sshkey
$ vi sshkey.private     # 写入上述步骤得到的私钥
# 修改文件的权限位
# It is required that your private key files are NOT accessible by others.
$ chmod 0600 sshkey.private
$ ssh -i sshkey.private bandit17@localhost
$ cat /etc/bandit_pass/bandit17
xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn

$ cd && rm -r /tmp/chicken-sshkey
```

参考：[nmap(1) - Linux man page](https://linux.die.net/man/1/nmap)

### Level 17

There are 2 files in the homedirectory: **passwords.old** and **passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

```bash
# 可以使用密码登录或 SSH 免密登录

$ diff passwords.old passwords.new
42c42   # 表示两个文件在第 42 行不同
< w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii
---
> kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```

### Level 18

The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

```bash
# SSH 远程执行命令
$ ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org’s password:
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

### Level 19

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

```bash
$ ssh bandit19@bandit.labs.overthewire.org -p 2220

$ ls -l
total 8
-rwsr-x--- 1 bandit20 bandit19 7296 May  7 20:14 bandit20-do

$ cat /etc/bandit_pass/bandit20
cat: /etc/bandit_pass/bandit20: Permission denied
$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id

$ id
uid=11019(bandit19) gid=11019(bandit19) groups=11019(bandit19)
$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
# euid：有效用户 ID

$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

参考：[setuid - Wikipedia](https://en.wikipedia.org/wiki/Setuid)

### Level 20

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

```bash
$ ssh bandit20@bandit.labs.overthewire.org -p 2220

$ ls -l
total 12
-rwsr-x--- 1 bandit21 bandit20 12088 May  7 20:14 suconnect
$ ./suconnect
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.

$ echo GbKksEFF4yrVs6il55v6gwY5aVje5f0j | nc -l -p 23333 &
$ ./suconnect 23333
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
[1]+  Done                    echo GbKksEFF4yrVs6il55v6gwY5aVje5f0j | nc -l -p 23333
```

参考：[nc(1) [linux man page]](https://www.unix.com/man-page/linux/1/nc/)
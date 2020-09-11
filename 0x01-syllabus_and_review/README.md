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
```
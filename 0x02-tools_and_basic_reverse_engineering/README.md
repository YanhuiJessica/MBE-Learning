## Intro Crackmes

[Download - challenges.zip](http://security.cs.rpi.edu/courses/binexp-spring2015/lectures/2/challenges.zip)

### Environment

- Kali Linux
- GNU bash，版本 5.0.3(1)-release (x86_64-pc-linux-gnu)

### crackme0x00a

```bash
$ file crackme0x00a
crackme0x00a: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-, for GNU/Linux 2.6.15, BuildID[sha1]=a01d6d16a59c7f0d7ec00ab5454eed2eb22bd20d, not stripped
$ ./crackme0x00a
Enter password: 1234
Wrong!
Enter password: ^C
$ strings crackme0x00a
```
![使用 strings 查看可打印字符](img/0x00a-strings.jpg)
```bash
$ xxd crackme0x00a
```
![使用 xxd 查看](img/0x00a-xxd.jpg)
```bash
$ ./crackme0x00a
Enter password: g00dJ0B!
Congrats!
```

### crackme0x00b

```bash
$ file crackme0x00b
crackme0x00b: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.15, BuildID[sha1]=bd4052c7de7891abb85e0087656d212119aadee0, not stripped
$ ./crackme0x00b
Enter password: 1234
Wrong!
Enter password: ^C

# 提示使用 -e 选项
# -e --encoding={s,S,b,l,B,L} Select character size and endianness:
# s = 7-bit, S = 8-bit, {b,l} = 16-bit, {B,L} = 32-bit

# 枚举后，可使用 {B,L} 获取密码
$ strings -e B crackme0x00b
w0wgreat
$ strings -e L crackme0x00b
w0wgreat

$ ./crackme0x00b
Enter password: w0wgreat
Congrats!
```

### crackme0x01

```bash
$ file crackme0x01
crackme0x01: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.9, not stripped
$ ./crackme0x01
IOLI Crackme Level 0x01
Password: 1234
Invalid Password!

$ objdump -d crackme0x01 | grep -A 30 '<main>'
080483e4 <main>:
 80483e4:	55                   	push   %ebp
 80483e5:	89 e5                	mov    %esp,%ebp
 80483e7:	83 ec 18             	sub    $0x18,%esp
 80483ea:	83 e4 f0             	and    $0xfffffff0,%esp
 80483ed:	b8 00 00 00 00       	mov    $0x0,%eax
 80483f2:	83 c0 0f             	add    $0xf,%eax
 80483f5:	83 c0 0f             	add    $0xf,%eax
 80483f8:	c1 e8 04             	shr    $0x4,%eax
 80483fb:	c1 e0 04             	shl    $0x4,%eax
 80483fe:	29 c4                	sub    %eax,%esp
 8048400:	c7 04 24 28 85 04 08 	movl   $0x8048528,(%esp)
 8048407:	e8 10 ff ff ff       	call   804831c <printf@plt>
 804840c:	c7 04 24 41 85 04 08 	movl   $0x8048541,(%esp)
 8048413:	e8 04 ff ff ff       	call   804831c <printf@plt>
 8048418:	8d 45 fc             	lea    -0x4(%ebp),%eax
 804841b:	89 44 24 04          	mov    %eax,0x4(%esp)
 804841f:	c7 04 24 4c 85 04 08 	movl   $0x804854c,(%esp)
 8048426:	e8 e1 fe ff ff       	call   804830c <scanf@plt>
 804842b:	81 7d fc 9a 14 00 00 	cmpl   $0x149a,-0x4(%ebp)
 8048432:	74 0e                	je     8048442 <main+0x5e>  # 与 0x149a 比较，如果相等跳转
 8048434:	c7 04 24 4f 85 04 08 	movl   $0x804854f,(%esp)
 804843b:	e8 dc fe ff ff       	call   804831c <printf@plt>
 8048440:	eb 0c                	jmp    804844e <main+0x6a>
 8048442:	c7 04 24 62 85 04 08 	movl   $0x8048562,(%esp)
 8048449:	e8 ce fe ff ff       	call   804831c <printf@plt>
 804844e:	b8 00 00 00 00       	mov    $0x0,%eax
 8048453:	c9                   	leave
 8048454:	c3                   	ret
 8048455:	90                   	nop
 8048456:	90                   	nop
$ echo $((0x149a))
5274
$ ./crackme0x01
IOLI Crackme Level 0x01
Password: 5274
Password OK :)
```

### crackme0x02

```bash
$ file crackme0x02
crackme0x02: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.9, not stripped
$ ./crackme0x02
IOLI Crackme Level 0x02
Password: 1234
Invalid Password!
```
使用 IDA Pro 将汇编代码还原成伪代码查看<br>
![获得密码 338724](img/0x02.jpg)

```bash
$ ./crackme0x02
IOLI Crackme Level 0x02
Password: 338724
Password OK :)
```

### crackme0x03

```bash
$ file crackme0x03
crackme0x03: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.9, not stripped
$ ./crackme0x03
IOLI Crackme Level 0x03
Password: 1234
Invalid Password!
```

使用 IDA Pro 将汇编代码还原成伪代码查看，发现调用了函数`test`<br>
![test(v5, 338724)](img/0x03-test.jpg)

```c
int __cdecl test(int a1, int a2)
{
  int result; // eax

  if ( a1 == a2 )
    result = shift("Sdvvzrug#RN$$$#=,");
  else
    result = shift("Lqydolg#Sdvvzrug$");
  return result;
}

int __cdecl shift(char *s)
{
  size_t i; // [esp+1Ch] [ebp-7Ch]
  char v3[120]; // [esp+20h] [ebp-78h]

  for ( i = 0; i < strlen(s); ++i )
    v3[i] = s[i] - 3;
  v3[i] = 0;
  return printf("%s\n", v3);
}
```

使用工具 [ROT Cipher - Rotation - Rot Decoder, Encoder, Solver, Translator](https://www.dcode.fr/rot-cipher) 得到实际的字符串

```
Sdvvzrug#RN$$$#=, => Password~OK!!!~:)
Lqydolg#Sdvvzrug$ => Invalid~Password!
```

判断输入是否与 *338724* 相等，*338724* 即是正确答案

```bash
$ ./crackme0x03
IOLI Crackme Level 0x03
Password: 338724
Password OK!!! :)
```
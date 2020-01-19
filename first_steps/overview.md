## 框架

Radare2项目是一组小型命令行实用程序，可以一起使用或独立使用。

本章将使您快速了解它们，但是您可以在本书结尾处查看每种工具的专用部分。

### radare2

整个框架的主要工具。它使用十六进制编辑器和调试器的核心。Radar2允许您打开许多输入/输出源，就像它们是简单的普通文件一样，包括磁盘，网络连接，内核驱动程序，调试中的进程等。

它实现了高级命令行界面，用于在文件中移动，分析数据，拆卸，二进制修补，数据比较，搜索，替换和可视化。可以使用多种语言编写脚本，包括Python，Ruby，JavaScript，Lua和Perl。

### rabin2

从可执行二进制文件（例如ELF，PE，Java CLASS，Mach-O）中提取信息的程序，以及r2插件支持的任何格式。内核使用rabin2来获取数据，例如导出的符号，导入，文件信息，交叉引用（xref），库依赖项和节。

### rasm2

适用于多种体系结构（包括Intel x86和x86-64，MIPS，ARM，PowerPC，Java以及其他众多架构）的命令行汇编程序和反汇编程序。

#### 例子
```
$ rasm2 -a java 'nop'
00
```
```
$ rasm2 -a x86 -d '90'
nop
```
```
$ rasm2 -a x86 -b 32 'mov eax, 33'
b821000000
```
```
$ echo 'push eax;nop;nop' | rasm2 -f -
509090
```

### rahash2

基于块的哈希工具的实现。从小文本字符串到大磁盘，rahash2支持多种算法，包括MD4，MD5，CRC16，CRC32，SHA1，SHA256等。rahash2可用于检查完整性或跟踪大文件，内存转储或磁盘的更改。

### 例子
```
$ rahash2 file
file: 0x00000000-0x00000007 sha256: 887cfbd0d44aaff69f7bdbedebd282ec96191cce9d7fa7336298a18efc3c7a5a
```
```
$ rahash2 -a md5 file
file: 0x00000000-0x00000007 md5: d1833805515fc34b46c2b9de553f599d
```
### radiff2

一个实现多种算法的二进制差异实用程序。它支持二进制文件的字节级或增量差分，以及代码分析差分，以查找从radare代码分析获得的基本代码块中的更改。

### rafind2

在文件中查找字节模式的程序。

### ragg2

r_egg的前端。ragg2将用简单的高级语言编写的程序编译为x86，x86-64和ARM的微型二进制文件。

#### 例子

```
$ cat hi.r
/* hello world in r_egg */
write@syscall(4); //x64 write@syscall(1);
exit@syscall(1); //x64 exit@syscall(60);

main@global(128) {
 .var0 = "hi!\n";
 write(1,.var0, 4);
 exit(0);
}
$ ragg2 -O -F hi.r
$ ./hi
hi!

$ cat hi.c
main@global(0,6) {
 write(1, "Hello0", 6);
 exit(0);
}
$ ragg2 hi.c
$ ./hi.c.bin
Hello
```

### rarun2

启动程序，用于在不同的环境中运行具有不同参数，权限，目录和覆盖的默认文件描述符的程序。rarun2可用于：

* 解决 crackmes
* Fuzzing
* 测试套件

#### Sample rarun2 script
```
$ cat foo.rr2
#!/usr/bin/rarun2
program=./pp400
arg0=10
stdin=foo.txt
chdir=/tmp
#chroot=.
./foo.rr2
```

#### 用套接字连接程序
```
$ nc -l 9999
$ rarun2 program=/bin/ls connect=localhost:9999
```

#### 调试程序以将stdio重定向到另一个终端

1 - open a new terminal and type 'tty' to get a terminal name:

```
$ tty ; clear ; sleep 999999
/dev/ttyS010
```

2 - Create a new file containing the following rarun2 profile named foo.rr2:
```
#!/usr/bin/rarun2
program=/bin/ls
stdio=/dev/ttys010
```

3 - Launch the following radare2 command:
```
r2 -r foo.rr2 -d /bin/ls
```

### rax2

A minimalistic mathematical expression evaluator for the shell that is useful for making base conversions between floating point values, hexadecimal representations, hexpair strings to ASCII, octal to integer, and more. It also supports endianness settings and can be used as an interactive shell if no arguments are given.

简约的数学表达式评估器，用于在浮点值，十六进制表示形式，十六进制对字符串到ASCII，八进制到整数等之间进行基本转换。它还支持字节序设置，并且如果未提供任何参数，则可以用作交互式外壳。

#### Examples

```
$ rax2 1337
0x539

$ rax2 0x400000
4194304

$ rax2 -b 01111001
y

$ rax2 -S radare2
72616461726532

$ rax2 -s 617765736f6d65
awesome
```

# 调用约定

Radare2 uses calling conventions to help in identifying function formal arguments and return types. It is used also as a guide for basic function prototype and type propagation.

```text
[0x00000000]> afc?
|Usage: afc[agl?]
| afc convention  Manually set calling convention for current function
| afc             Show Calling convention for the Current function
| afcr[j]         Show register usage for the current function
| afca            Analyse function for finding the current calling convention
| afcf name       Prints return type function(arg1, arg2...)
| afcl            List all available calling conventions
| afco path       Open Calling Convention sdb profile from given path
[0x00000000]>
```

* To list all available calling conventions for current architecture using `afcl` command

```text
[0x00000000]> afcl
amd64
ms
```

* To display function prototype of standard library functions you have `afcf` command 

```text
[0x00000000]> afcf printf
int printf(const char *format)
[0x00000000]> afcf fgets
char *fgets(char *s, int size, FILE *stream)
```

All this information is loaded via sdb under `/libr/anal/d/cc-[arch]-[bits].sdb`

```text
default.cc=amd64

ms=cc
cc.ms.name=ms
cc.ms.arg1=rcx
cc.ms.arg2=rdx
cc.ms.arg3=r8
cc.ms.arg3=r9
cc.ms.argn=stack
cc.ms.ret=rax
```

`cc.x.argi=rax` is used to set the ith argument of this calling convention to register name `rax`

`cc.x.argn=stack` means that all the arguments \(or the rest of them in case there was argi for any i as counting number\) will be stored in stack from left to right

`cc.x.argn=stack_rev` same as `cc.x.argn=stack` except for it means argument are passed right to left


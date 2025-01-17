# 远程WinDbg

The WinDBG support for r2 allows you to attach to VM running Windows using a named socket file \(will support more IOs in the future\) to debug a windows box using the KD interface over serial port.

Bear in mind that WinDBG support is still work-in-progress, and this is just an initial implementation which will get better in time.

It is also possible to use the remote GDB interface to connect and debug Windows kernels without depending on Windows capabilities.

Enable WinDBG support on Windows Vista and higher like this:

```text
bcdedit /debug on
bcdedit /dbgsettings serial debugport:1 baudrate:115200
```

Starting from Windows 8 there is no way to enforce debugging for every boot, but it is possible to always show the advanced boot options, which allows to enable kernel debugging:

```text
bcedit /set {globalsettings} advancedoptions true
```

Or like this for Windows XP: Open boot.ini and add /debug /debugport=COM1 /baudrate=115200:

```text
[boot loader]
timeout=30
default=multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
[operating systems]
multi(0)disk(0)rdisk(0)partition(1)\WINDOWS="Debugging with Cable" /fastdetect /debug /debugport=COM1 /baudrate=57600
```

In case of VMWare

```text
    Virtual Machine Settings -> Add -> Serial Port
    Device Status:
    [v] Connect at power on
    Connection:
    [v] Use socket (named pipe)
    [_/tmp/windbg.pipe________]
    From: Server To: Virtual Machine
```

Configure the VirtualBox Machine like this:

```text
    Preferences -> Serial Ports -> Port 1

    [v] Enable Serial Port
    Port Number: [_COM1_______[v]]
    Port Mode:   [_Host_Pipe__[v]]
                 [v] Create Pipe
    Port/File Path: [_/tmp/windbg.pipe____]
```

Or just spawn the VM with qemu like this:

```text
$ qemu-system-x86_64 -chardev socket,id=serial0,\
     path=/tmp/windbg.pipe,nowait,server \
     -serial chardev:serial0 -hda Windows7-VM.vdi
```

Radare2 will use the 'windbg' io plugin to connect to a socket file created by virtualbox or qemu. Also, the 'windbg' debugger plugin and we should specify the x86-32 too. \(32 and 64 bit debugging is supported\)

```text
$ r2 -a x86 -b 32 -D windbg windbg:///tmp/windbg.pipe
```

On Windows you should run the following line:

```text
$ radare2 -D windbg windbg://\\.\pipe\com_1
```

At this point, we will get stuck here:

```text
[0x828997b8]> pd 20
    ;-- eip:
    0x828997b8    cc           int3
    0x828997b9    c20400       ret 4
    0x828997bc    cc           int3
    0x828997bd    90           nop
    0x828997be    c3           ret
    0x828997bf    90           nop
```

In order to skip that trap we will need to change eip and run 'dc' twice:

```text
dr eip=eip+1
dc
dr eip=eip+1
dc
```

Now the Windows VM will be interactive again. We will need to kill r2 and attach again to get back to control the kernel.

In addition, the `dp` command can be used to list all processes, and `dpa` or `dp=` to attach to the process. This will display the base address of the process in the physical memory layout.


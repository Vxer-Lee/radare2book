# 下载 radare2

您可以从网站, [http://radare.org](http://radare.org)或GitHub获取: [https://github.com/radare/radare2](https://github.com/radare/radare2)

二进制程序包可用于许多操作系统（Ubuntu，Maemo，Gentoo，Windows，iPhone等）。但是，强烈建议您获取源代码并自己进行编译，以更好地理解依赖关系，使示例更易于访问，并且当然拥有最新版本。

通常每个月都会发布一个新的稳定版本。有时可以在 [http://bin.rada.re/](http://bin.rada.re/) 获得每晚tarball .

radare开发仓库通常比“稳定”版本稳定。要获取最新版本:

```text
$ git clone https://github.com/radare/radare2.git
```

这可能会花费一些时间，因此请稍事休息并继续阅读本书。

要更新仓库的本地副本，请git pull在radare2源代码树中的任何位置使用：

```text
$ git pull
```

如果您对源代码进行了本地修改，则可以使用以下命令还原它们（并丢失它们！）。

```text
$ git reset --hard HEAD
```

或向我们发送补丁：

```text
$ git diff > radare-foo.patch
```

在整个系统范围内更新和安装r2的最常见方法是使用：

```text
$ sys/install.sh
```

## Build with meson + ninja

There is also a work-in-progress support for Meson.

Using clang and ld.gold makes the build faster:

```bash
CC=clang LDFLAGS=-fuse-ld=gold meson . release --buildtype=release --prefix ~/.local/stow/radare2/release
ninja -C release
# ninja -C release install
```

## 辅助脚本

看一下 `sys/*` 脚本，这些脚本用于自动化与同步，构建和安装r2及其绑定有关的内容。

最重要的是 `sys/install.sh`。它将拉动，清理，构建和安装r2系统。

Symstalling是使用符号链接而不是复制文件来安装所有程序，库，文档和数据文件的过程。

默认情况下，它将安装在/ usr中，但是您可以定义一个新的前缀作为参数。

这对开发人员很有用，因为它允许开发人员仅运行“ make”并尝试更改，而不必再次运行make install。

## 打扫干净

清理源代码树对于避免诸如链接到旧对象文件或在ABI更改后不更新对象之类的问题很重要。

以下命令可以帮助您更新git克隆：

```text
$ git clean -xdf
$ git reset --hard @~10
$ git pull
```

如果要从系统中删除以前的安装，则必须运行以下命令：

```text
$ ./configure --prefix=/usr/local
$ make purge
```


# 历史

2006年，塞尔吉·阿尔瓦雷斯（SergiÁlvarez，又名pancake）担任法医分析师。由于不允许他使用私人软件来满足个人需要，因此他决定编写一个具有非常基本特征的小型工具（十六进制编辑器）：

* 极其便携（unix友好，命令行，c，小型）
* 打开磁盘设备，这使用64位偏移量
* 搜索字符串或十六进制对
* 查看结果并将其转储到磁盘

该编辑器最初旨在从HFS +分区中恢复已删除的文件。

之后，pancake决定扩展该工具，使其具有可插入的io，以便能够附加到进程，并实现了调试器功能，对多种体系结构的支持以及代码分析。

从那时起，该项目发展到提供一个用于分析二进制文件的完整框架，同时利用了基本的UNIX概念。这些概念包括著名的“一切都是文件”，“使用stdin / stdout进行交互的小型程序”和“保持简单”范例。

对脚本的需求表明了初始设计的脆弱性：单片工具使API难以使用，因此需要进行深度重构。2009年，radare2（r2）作为radare1的分支而诞生。重构增加了灵活性和动态功能。这实现了更好的集成，为使用[不同编程语言的](https://github.com/radare/radare2-bindings) r2铺平了道路。稍后，[r2pipe API](https://github.com/radare/radare2-r2pipe) 允许通过任何语言的管道访问radare2。

最初是由一个人参与的项目开始，最终做出了一些贡献，但在2014年左右逐渐发展成为一个大型的社区项目。用户数量迅速增长，作者和主要开发人员不得不将角色从编码人员转换为经理为了整合加入该项目的不同开发人员的工作。

指示用户报告他们的问题，使该项目可以定义发展的新方向。一切都在[radare2的GitHub](https://github.com/radare/radare2)中进行管理，并在Telegram频道中进行讨论。

在撰写本书时，该项目仍处于活动状态，并且有多个附带项目，除其他外，这些项目提供图形用户界面（[Cutter](https://github.com/radareorg/cutter)），反编译器（[r2dec](https://github.com/wargio/r2dec-js)，[radeco](https://github.com/radareorg/radeco)），Frida集成（[r2frida](https://github.com/nowsecure/r2frida)），Yara，Unicorn ，Keystone和其他许多在\[[r2pm](https://github.com/radare/radare2-pm)\]（radare2软件包管理器）中建立索引的项目。

自2016年以来，社区每年都在\[r2con\([https://www.radare.org/con/\)\]上聚会，r2con是在巴塞罗那举行的一次围绕rarad2的大会。](https://www.radare.org/con/%29]上聚会，r2con是在巴塞罗那举行的一次围绕rarad2的大会。)


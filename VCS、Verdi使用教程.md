## VCS、Verdi使用教程

### 简介

VCS是编译型Verilog仿真器

Verdi自动调试平台，除了源代码浏览器的标准功能，原理图，波形，状态机图和波形比较（用于比较FSDB格式的仿真结果），Verdi平台还包括使用时间流视图自动跟踪信号活动的高级功能，基于断言的调试，功耗感知调试以及事务和消息数据的调试和分析。

### VCS相关参数

* -full64 ： VCS以64位运行
* -sverilog：如果你的文件后缀名用的是.sv，那么编译时要加上该参数
* -cpp g++ -4.8 ： 要根据的VCS的版本寻找对应的G++的版本
* -cc gcc-4.8：对应gcc的版本
* -LDFLAGS ：这个参数和后面的参数不写有时还会报错，指用GCC等编译器中的一些优化* 参数
* -Wl, --no-as-needed：把用户指定的链接库全部写入可执行文件中，而不是依赖的库写入
* -debug_all：这个参数是肯定需要的
* -rdynamic：指示需要加载的动态库
* -R：表示在编译完成之后自动运行仿真
* -f ：filelist.f文件，用于表示所有源文件
* -o ：指定输出的可执行文件的名字，缺省是simv

### VCS查看覆盖率，并用dve查看

1. 将文件路径加入到filelist.f中
2. 在终端下使用命令 vcs -f filelist.f -full64 -debug_all -cm line+cond+fsm+tgl -LDFLAGS -Wl,--no-as-needed
3. 运行./simv -cm line+cond+fsm+tgl
4. 查看文件，多了一个*.vdb的文件夹，使用dve查看覆盖率：dve -full64 -cov simv.vdb

### 问题

1. 安装教程参考网上，每次使用VCS、Dve和Verdi，都需要开启激活。激活指令：lmg_synopsys
2. 若破解后运行VCS报出"/bin/sh illegal option -h"的错误，则需要检查是否安装了sh。若没有安装sh，则需要sudo apt-get install sh。
3. error:illegal 'timescale for module,module"AN2D0" has 'timescale but previous module do not

   解决方案：把文件中第一行的timescale注释掉，并在调用VCS时加入参数-timescale=1ns/10ps

4. 报如“libvcsnew.so: undefined reference to...”的错误

   解决方案： -LDFLAGS -Wl,--no-as-needed
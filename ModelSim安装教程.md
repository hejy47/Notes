# ModelSim安装及破解教程

## ModelSim安装

本教程是在ubuntu上安装并破解modelsim，从intelFPGA中下载并安装modelsim即可。

## ModelSim破解

1. 安装wine：`sudo apt-get install wine64`
2. 生成license：`wine MentorKG.exe`
3. `export LM_LICENSE_FILE=/path/to/modelsim/LICENSE.TXT`
4. `export PATH=$PATH:/path/to/modelsim/linux/bin`

## ModelSim使用问题

### bash:./vsim: No such file or directory

1. `sudo dpkg --add-architecture i386`
2. `sudo apt-get update`
3. `sudo apt-get install libc6:i386 libncurses5:i386 libstdc++:i386`

### libXext.so.6: cannot open shared object file: No such file or directory

`sudo apt-get install libxext6:i386`

### libXft.so.2: cannot open shared object file: No such file or directory

`sudo apt-get install libxft2:i386`

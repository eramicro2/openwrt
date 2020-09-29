IP 192.168.1.1, 密码 password

编译命令如下:
-
1.  Ubuntu  18 LTS x64

2. 环境准备

   `sudo apt-get update`

   `sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget curl swig rsync`

3. `git clone https://github.com/coolsnowwolf/lede openwrt` 下载源代码，然后进入目录中。

4. ```bash

   下载feeds源中的软件包源码
   
    ./scripts/feeds update -a
   
   安装feeds中的软件包
   
    ./scripts/feeds install -a
   
   调整 OpenWrt 系统组件
   
     make menuconfig

5. 预下载编译所需的软件包

    `make -j8 download V=s`
   
6. 检查文件完整性

    `find dl -size -1024c -exec ls -l {} \;`
   
   此命令列出下载不完整的文件，如果存在这样的文件可以使用find dl -size -1024c -exec rm -f {} \;命令将它们删除，然后重新执行make download下载并反复检查，确认所有文件完整可大大提高编译成功率。
   
7. 开始编译

    `make -j1 V=s` 
 
    -j1 后面是线程数。第一次编译推荐用单线程。



二次编译：
```bash
cd lede
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make -j8 download
make -j$(($(nproc) + 1)) V=s
```

如果需要重新配置：
```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make -j$(($(nproc) + 1)) V=s
```

编译完成后输出路径：/lede/bin/targets

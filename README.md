# openwrt
openwrt-wifidog编译

	提示：openwrt编译包在编译的过程中会在git中下载文件，编译之后的文件相当的大，所以需要一个大的磁盘编译环境，
	我用的是VMware虚拟机中安装ubuntu16.04编译openwrt的，在安装虚拟机时，我的100G磁盘空间的ubuntu内存分配方案为：
			 (1) 根文件目录分配了60G;
			 (2) home文件目录分配了10G;
			 (3) swp目录分配了20G;
			 (4) tmp文件目录分配了10G。（一开始编译的时候我是在home文件目录中编译的，结果很悲催，磁盘用的一滴不剩，后来转用根文件目录）
	编译完成之后整个文件大小为13G。

1，下载环境
	(1) 下载环境

	sudo apt-get install gcc g++ binutils patch bzip2 flex bison make autoconf gettext texinfo unzip sharutils subversion libncurses5-dev ncurses-term zlib1g-dev gawk asciidoc libz-dev git libssl-dev
	 
	(2) 更新程序
	    ./script/feeds update -a
	    ./script/feeds install -a
	    make menuconfig
	    cp feeds.conf.default feeds.conf

	(3) 权限修改为用户权限
	(4) 修改环境：
	     export FORCE_UNSAFE_CONFIGURE=1
	     export SOURCE_DATE_EPOCH=1
	(5) make defconfig
	     make prereq   --显示未安装的环境包 
	     make V=99

在编译的过程中出现了错误，
		（mkyaffsimage.o报的错误）
		environment variable SOURCE_DATE_EPOCH must expand to a non-negative integer less than or equal to xxx

这个错误耽搁了我很久，当时没仔细想，直接在网上找答案，以为能行，结果也很悲催。再一次闲来没事做的时候，我决定自己解决，发现很简单，只要把这个二进制文件删除重新编译就行。
		我的mkyaffs文件放置的文件目录为build_dir/host/yaffs2_android/yaffs2/utils（这个路径在错误的前面可以找得到）
		cd build_dir/host/yaffs2_android/yaffs2/utils/
		rm mkyaffsimage.o
		make
		然后退出到之前的目录，make V=99就行了。

2，openwrt中wifidog的编译包为：

     wifidog-gateway-master.tar

wifidog-gateway-master放置的位置为：

     cd /openwrt-x86/openwrt-master/build_dir/target-x86_64_musl-1.1.15/wifidog-normal/wifidog-1.3.0

3，openwrt的源码包路径：

	src/gz puppies_base http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/base
	src/gz puppies_kernel http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/kernel
	src/gz puppies_telephony http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/telephony
	src/gz puppies_oldpackages http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/oldpackages
	src/gz puppies_packages http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/packages
	src/gz puppies_qca http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/qca
	src/gz puppies_puppies http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/puppies
	src/gz puppies_routing http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/routing
	src/gz puppies_luci http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/luci
	src/gz puppies_management http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/management
	# src/gz puppies_targets http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/targets



我只在下面这一个源上下载到过lrzsz.ipk（使用sz和rz命令的源码包）：
src/gz attitude_adjustment http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/packages/Packages.gz
--------------------
还可以使用wget自己下载
wget http://downloads.openwrt.org/snapshots/trunk/x86/64/packages/packages/lrzsz_0.12.20-1_x86_64.ipk


###Build Android 5.x Lollipop on Ubuntu Flash Device

----------

1:   搭建Ubuntu 编译Android 源码环境 
Ubuntu 14.04   LTS  
下载地址为：
http://www.ubuntu.org.cn/download/desktop
Select 64-bit  Downloading
Install  Ubuntu 64-bit   

Ubuntu 14.04   LTS
> sudo apt-get install 
> 
> bison  g++-multilib  git gperf  libxml2-utils  make  python-networkx zlib1g-dev:i386 zip

Ubuntu 12.04

> sudo apt-get install 
> 
>   bison  git gnupg flex bison gperf build-essential \
  zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
  libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
  libgl1-mesa-dev g++-multilib mingw32 tofrodos \
  python-markdown libxml2-utils xsltproc zlib1g-dev:i386  
> 
> 
> sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so

Ubuntu  10.04 — 11.10   Android5.x Building不再支持 

构建android aosp 编译环境 完成


----------


2:  Downloading the repo

    $ mkdir ~/bin
    $ PATH=~/bin:$PATH

    $ sudo apt-get install curl 

    $ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    $ chmod a+x ~/bin/repo
完成以上命令后
此时 你的~/bin  目录中 会出现 repo 套件 并拥有可执行权限
3. Initializing a Repo client  
     初始化一个存放源码的目录 

> $ mkdir WORKING_DIRECTORY
> 
> $ cd WORKING_DIRECTORY

运行repo 初始化 android aosp 源码分支

> $ repo init -u https://android.googlesource.com/platform/manifest

 以上命令会在本地初始化Git本地仓库并连接Google android 远程仓库  检出所有的分支名称  来供你选择
此命令完成后  Ubuntu 控制台会输出 所有的android aosp各个版本分支名称

开始选择你所要检出的分支名称
这里假设我们要检出的android aosp 版本为 android-4.0.1_r1

> repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1

当然 5.x已经出来了  那么我们只需要把 android-4.0.1_r1  替换为支持你设备的aosp源码分支即可   android-5.x.x_rx


----------
选择支持你自己Nexus Device 的源码分支

http://source.android.com/source/build-numbers.html
5.1.x 源码支持的Nexus Device 系列

| Build  | Branch  | Version Supported |devices|
| :------------ |:---------------|:-----|:-----|
| LVY48E      | android-5.1.1_r13 | Lollipop |Nexus 6 (For Project Fi ONLY) |
| LYZ28J      | android-5.1.1_r12       |   Lollipop |Nexus 6 (For T-Mobile ONLY) |
| LMY48J | android-5.1.1_r10        |    Lollipop |Nexus Player |
| LMY48I      | android-5.1.1_r9 | Lollipop |Nexus 4, Nexus 5, Nexus 6, Nexus 7 (flo), Nexus 9 (volantis/volantisg), Nexus 10 |
| LVY48C      | android-5.1.1_r8        |   Lollipop |Nexus 6 (For Project Fi ONLY) |
| LMY48G | android-5.1.1_r6        |    Lollipop |Nexus 7 (flo) |
| LYZ28E      | android-5.1.1_r5 | Lollipop |Nexus 6 (For T-Mobile ONLY) |
| LMY47Z      | android-5.1.1_r4        |   Lollipop |Nexus 6 (All carriers except T-Mobile US) |
| LMY48B | android-5.1.1_r3        |    Lollipop |Nexus 5 |
| LMY47X      | android-5.1.1_r2 | Lollipop |Nexus 9 (volantis) |
| LMY47V      | android-5.1.1_r1        |   Lollipop |Nexus 7 (flo/grouper), Nexus 10, Nexus Player |
| LMY47O | android-5.1.0_r5        |    Lollipop |Nexus 4, Nexus 7 (flo/deb) |
| LMY47M      | android-5.1.0_r4 | Lollipop |Nexus 6 (For T-Mobile ONLY) |
| LMY47I     | android-5.1.0_r3       |   Lollipop |Nexus 5, Nexus 6 |
| LMY47E | android-5.1.0_r2        |    Lollipop |Nexus 6 |
| LMY47D | android-5.1.0_r1        |    Lollipop |Nexus 5, Nexus 6, Nexus 7 (grouper/tilapia), Nexus 10, Nexus Player |

> Downloading the Android Source Tree
> 
> 开始同步你选择的android aosp源码分支
> 
> $ repo sync

此命令可能 会执行  很长一段时间   这 取决与 你的 网络 和vpn翻墙的 稳定性
如果中途同步失败 可再次执行以上命令继续上次的同步

直到Ubuntu 控制台输出 如下       恭喜： 这个时候已经 说明 你已经同步成功了


----------

> ####Downloading the drivers
> 下载和你nexus设备相匹配的驱动  
> https://developers.google.com/android/nexus/drivers
> 这里有三点需要注意下
 1- 选择你的机型  device
 2- 选择你编译的源码版本
 3- 选择你编译的版本号  Build
 如：Nexus 5 (GSM/LTE) binaries for Android 5.1.1 (LMY48B)
 device:   Nexus5
 source Release: Android 5.1.1  
 build: LMY48B
>

驱动下载完成后解压到你的android源码根目录

###Building android source

----------

>$ source build/envsetup.sh
>
>or
> 
>$ . build/envsetup.sh
>
>Choose a Target
>选择我们要编译的设备
>
>$ lunch aosp_hammerhead-userdebug
> aosp_hammerhead-userdebug  指定的是 Nexus5 Device
>
>android aosp 源码根目录  依次 执行 解压出来的驱动文件
>
>$ sh *.sh
>
> I ACCEPT
会将相关驱动放到源码根目录 vender目录下面
> 
> 清除 操作   等同于  rm -rf ./out
>$ make clobber
>
>$ nohup make -j4 > buildLog.txt 2>&1 &
> 
> -j4    内核数*2=线程数
>
> 以上命令将编译得到的标准输出与错误输出重定向到buildLog.txt 文件中 并将此次任务放入后台执行
> 
> $ jobs
> 查看后台任务执行情况
> 
>$ vi buildLog.txt 
>查看编译输出的内容
> 
> 这里有个小技巧
> $ du -sh ./out  
> 查看编译输出的目录及子目录和文件的大小  一般编译完成 out 目录大小为 20多个g
> 


####Flash Device


----------


1:   如果要刷入我们的设备中  则需要  fastboot  
这里 我们开始编译  fastboot  和  adb     
这里  说明下   fastboot  套件 用于 刷机  解锁   重新锁定  等 功能  主要作用 和刷机息息相关
adb  主要用于调试我们的android 真机 设备

好了  开始  编译  fastboot 与 adb 工具套件  同样源码根目录下执行

> $ make fastboot adb

以上命令会执行完后  你就拥有  fastboot 和 adb 工具套件了
Booting into fast boot mode
准备开始进入 刷机引导模式

nexus5 进入刷机引导模式

> 关机情况下  同时按下音量+ 音量- 与电源 三个按键    这里的电源按键  就是  我们平时用于开关机的按键
or
> $ adb reboot bootloader

Unlocking the bootloader    
解锁引导程序 

>$ fastboot oem unlock

 这时  手机屏幕上会弹出确认选项窗口  按下你的音量上或下按键 选择 同意 Yes…  然后按下 电源按键 确认

格式化我们的设备（如果你的设备是 Nexus 10 则必须运行下列命令 格式化用户数据）这里 我们统一选择格式化

注意:  如果你的手机设备上面有重要的数据   请通过usb  将设备上面的数据  备份至 你的电脑  以下操作 会清除掉你手机上的所有用户数据

开始执行清除命令

> $ fastboot format cache
> 
> $ fastboot format userdata

刷入到你的nexus设备当中

> $ fastboot -w flashall

刷机完成会自动重启
查看 
设置>关于手机

> Baseband version
> Unknown

这个时候是没有通讯基带的
ok 我们刷入通讯基带

1:  下载和你系统版本号相同的nexus images
https://developers.google.com/android/nexus/images
解压  把里面的  radio-*.img  复制到你的android源码根目录
同样在源码根目录下执行

>$ adb reboot bootloader 
>
> $ fastboot flash radio radio*.img

恢复bootloader锁

> $ fastboot oem lock




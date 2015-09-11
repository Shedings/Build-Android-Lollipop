ld Android 5.x Lollipop on Ubuntu Flash Device

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





> * 整理知识，学习笔记
> * 发布日记，杂文，所见所想
> * 撰写发布技术文稿（代码支持）
> * 撰写发布学术论文（LaTeX 公式支持）

![cmd-markdown-logo](https://www.zybuluo.com/static/img/logo.png)

除了您现在看到的这个 Cmd Markdown 在线版本，您还可以前往以下网址下载：

### [Windows/Mac/Linux 全平台客户端](https://www.zybuluo.com/cmd/)

> 请保留此份 Cmd Markdown 的欢迎稿兼使用说明，如需撰写新稿件，点击顶部工具栏右侧的 <i class="icon-file"></i> **新文稿** 或者使用快捷键 `Ctrl+Alt+N`。

------

## 什么是 Markdown

Markdown 是一种方便记忆、书写的纯文本标记语言，用户可以使用这些标记符号以最小的输入代价生成极富表现力的文档：譬如您正在阅读的这份文档。它使用简单的符号标记不同的标题，分割不同的段落，**粗体** 或者 *斜体* 某些文字，更棒的是，它还可以

### 1. 制作一份待办事宜 [Todo 列表](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#13-待办事宜-todo-列表)

- [ ] 支持以 PDF 格式导出文稿
- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [x] 修复 LaTex 公式渲染问题
- [x] 新增 LaTex 公式编号功能

### 2. 书写一个质能守恒公式[^LaTeX]

$$E=mc^2$$

### 3. 高亮一段代码[^code]

```python
@requires_authorization
class SomeClass:
    pass

if __name__ == '__main__':
    # A comment
    print 'hello world'
```

### 4. 高效绘制 [流程图](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#7-流程图)

```flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end

st->op->cond
cond(yes)->e
cond(no)->op
```

### 5. 高效绘制 [序列图](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#8-序列图)

```seq
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

### 6. 绘制表格

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |

### 7. 更详细语法说明

想要查看更详细的语法说明，可以参考我们准备的 [Cmd Markdown 简明语法手册][1]，进阶用户可以参考 [Cmd Markdown 高阶语法手册][2] 了解更多高级功能。

总而言之，不同于其它 *所见即所得* 的编辑器：你只需使用键盘专注于书写文本内容，就可以生成印刷级的排版格式，省却在键盘和工具栏之间来回切换，调整内容和格式的麻烦。**Markdown 在流畅的书写和印刷级的阅读体验之间找到了平衡。** 目前它已经成为世界上最大的技术分享网站 GitHub 和 技术问答网站 StackOverFlow 的御用书写格式。

---

## 什么是 Cmd Markdown

您可以使用很多工具书写 Markdown，但是 Cmd Markdown 是这个星球上我们已知的、最好的 Markdown 工具——没有之一 ：）因为深信文字的力量，所以我们和你一样，对流畅书写，分享思想和知识，以及阅读体验有极致的追求，我们把对于这些诉求的回应整合在 Cmd Markdown，并且一次，两次，三次，乃至无数次地提升这个工具的体验，最终将它演化成一个 **编辑/发布/阅读** Markdown 的在线平台——您可以在任何地方，任何系统/设备上管理这里的文字。

### 1. 实时同步预览

我们将 Cmd Markdown 的主界面一分为二，左边为**编辑区**，右边为**预览区**，在编辑区的操作会实时地渲染到预览区方便查看最终的版面效果，并且如果你在其中一个区拖动滚动条，我们有一个巧妙的算法把另一个区的滚动条同步到等价的位置，超酷！

### 2. 编辑工具栏

也许您还是一个 Markdown 语法的新手，在您完全熟悉它之前，我们在 **编辑区** 的顶部放置了一个如下图所示的工具栏，您可以使用鼠标在工具栏上调整格式，不过我们仍旧鼓励你使用键盘标记格式，提高书写的流畅度。

![tool-editor](https://www.zybuluo.com/static/img/toolbar-editor.png)

### 3. 编辑模式

完全心无旁骛的方式编辑文字：点击 **编辑工具栏** 最右测的拉伸按钮或者按下 `Ctrl + M`，将 Cmd Markdown 切换到独立的编辑模式，这是一个极度简洁的写作环境，所有可能会引起分心的元素都已经被挪除，超清爽！

### 4. 实时的云端文稿

为了保障数据安全，Cmd Markdown 会将您每一次击键的内容保存至云端，同时在 **编辑工具栏** 的最右侧提示 `已保存` 的字样。无需担心浏览器崩溃，机器掉电或者地震，海啸——在编辑的过程中随时关闭浏览器或者机器，下一次回到 Cmd Markdown 的时候继续写作。

### 5. 离线模式

在网络环境不稳定的情况下记录文字一样很安全！在您写作的时候，如果电脑突然失去网络连接，Cmd Markdown 会智能切换至离线模式，将您后续键入的文字保存在本地，直到网络恢复再将他们传送至云端，即使在网络恢复前关闭浏览器或者电脑，一样没有问题，等到下次开启 Cmd Markdown 的时候，她会提醒您将离线保存的文字传送至云端。简而言之，我们尽最大的努力保障您文字的安全。

### 6. 管理工具栏

为了便于管理您的文稿，在 **预览区** 的顶部放置了如下所示的 **管理工具栏**：

![tool-manager](https://www.zybuluo.com/static/img/toolbar-manager.jpg)

通过管理工具栏可以：

<i class="icon-share"></i> 发布：将当前的文稿生成固定链接，在网络上发布，分享
<i class="icon-file"></i> 新建：开始撰写一篇新的文稿
<i class="icon-trash"></i> 删除：删除当前的文稿
<i class="icon-cloud"></i> 导出：将当前的文稿转化为 Markdown 文本或者 Html 格式，并导出到本地
<i class="icon-reorder"></i> 列表：所有新增和过往的文稿都可以在这里查看、操作
<i class="icon-pencil"></i> 模式：切换 普通/Vim/Emacs 编辑模式

### 7. 阅读工具栏

![tool-manager](https://www.zybuluo.com/static/img/toolbar-reader.jpg)

通过 **预览区** 右上角的 **阅读工具栏**，可以查看当前文稿的目录并增强阅读体验。

工具栏上的五个图标依次为：

<i class="icon-list"></i> 目录：快速导航当前文稿的目录结构以跳转到感兴趣的段落
<i class="icon-chevron-sign-left"></i> 视图：互换左边编辑区和右边预览区的位置
<i class="icon-adjust"></i> 主题：内置了黑白两种模式的主题，试试 **黑色主题**，超炫！
<i class="icon-desktop"></i> 阅读：心无旁骛的阅读模式提供超一流的阅读体验
<i class="icon-fullscreen"></i> 全屏：简洁，简洁，再简洁，一个完全沉浸式的写作和阅读环境

### 8. 阅读模式

在 **阅读工具栏** 点击 <i class="icon-desktop"></i> 或者按下 `Ctrl+Alt+M` 随即进入独立的阅读模式界面，我们在版面渲染上的每一个细节：字体，字号，行间距，前背景色都倾注了大量的时间，努力提升阅读的体验和品质。

### 9. 标签、分类和搜索

在编辑区任意行首位置输入以下格式的文字可以标签当前文档：

标签： 未分类

标签以后的文稿在【文件列表】（Ctrl+Alt+F）里会按照标签分类，用户可以同时使用键盘或者鼠标浏览查看，或者在【文件列表】的搜索文本框内搜索标题关键字过滤文稿，如下图所示：

![file-list](https://www.zybuluo.com/static/img/file-list.png)

### 10. 文稿发布和分享

在您使用 Cmd Markdown 记录，创作，整理，阅读文稿的同时，我们不仅希望它是一个有力的工具，更希望您的思想和知识通过这个平台，连同优质的阅读体验，将他们分享给有相同志趣的人，进而鼓励更多的人来到这里记录分享他们的思想和知识，尝试点击 <i class="icon-share"></i> (Ctrl+Alt+P) 发布这份文档给好友吧！

------

再一次感谢您花费时间阅读这份欢迎稿，点击 <i class="icon-file"></i> (Ctrl+Alt+N) 开始撰写新的文稿吧！祝您在这里记录、阅读、分享愉快！

作者 [@ghosert][3]     
2015 年 06月 15日    

[^LaTeX]: 支持 **LaTeX** 编辑显示支持，例如：$\sum_{i=1}^n a_i=0$， 访问 [MathJax][4] 参考更多使用方法。

[^code]: 代码高亮功能支持包括 Java, Python, JavaScript 在内的，**四十一**种主流编程语言。

[1]: https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown
[2]: https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#cmd-markdown-高阶语法手册
[3]: http://weibo.com/ghosert
[4]: http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference

:wq


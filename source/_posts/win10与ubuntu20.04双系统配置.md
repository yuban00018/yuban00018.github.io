# win10与ubuntu20.04LTS双系统配置方案
## 引言
新学期开学了，买不起新的笔记本只好整点花活装一下双系统。

主要还是暑假学期整CV的库把我整哭了😭，一个两个都不支持windows，但我没有性能好一点的linux主机。于是这次把旧游戏本清空了一下硬盘，然后倒腾了一下双系统。当中有一些坑可以记录一下，
讲不定之后我给台式机上双系统用得上。

本文主要参照了：[ubuntu20.04+windows双系统安装](https://blog.csdn.net/Amorx12345/article/details/97510324) ，
在此感谢原博主，基本上是按照此文的操作完成的双系统安装，本文主要是精简内容然后增加一些个人踩坑的东西。

## 需要提前下载的内容
### UltralSO

官方下载链接：
https://cn.ultraiso.net/uiso9_cn.exe

### Ubuntu20.04

官方下载链接：
https://ubuntu.com/download/desktop/thank-you?version=20.04.1

tuna源：
https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/20.04.1

注：**tuna源**请选择desktop而不是server版本，不要下载错了

## Step1 删除卷
win + X + K 打开磁盘管理，选中一个硬盘，然后压缩出你要的大小，然后删除卷。

## Step2 制作启动盘
[下载UltralSO](https://cn.ultraiso.net/uiso9_cn.exe)，打开选择试用，拖入iso文件。
单击工具栏上的[启动]->[写入硬盘映像]。然后选择**硬盘驱动器**为指定U盘，写入方式为USB-HDD+。请不要和博主一样手贱选了软盘驱动器……然后按下写入，就完成了。

## 非必须项，关闭快速启动
按住Win + X，选择[电源选项]->[其他电源设置]，依次执行：[选择电源按钮的功能] -> [更改当前不可用的设置] -> [取消勾选快速启动] -> [保存修改]

虽然原博主写了这条，但我感觉好像是用不着的，开机长按对应主板的bios按键就可以顺利进入bios。但是禁用快速启动可以有效解决B450M主板关机后第一次开机不过自检的问题，
至于为什么有效，我也不懂😓。

## Step3 进入bios修改启动顺序
插上U盘并重启，长按对应按钮，我的机械革命笔记本按的是F2。进入bios，选择boot，然后把插进去的U盘设为开机首选项。保存并退出。

## Step4 安装Ubuntu
重启后会自动进入到安装引导，建议选择英文系统，然后后续安装中文输入法，因为如果选择了中文系统你的Desktop会变成中文的[桌面]，非常蛋疼。
选择Minimal installation安装，不要勾选Other Option。

#### 注意：Installation type
这里是我和原博文产生冲突的地方，我选择的是Install Ubuntu alongside WindowsBoot Manager而不是Something else.
实测，直接使用第一个选项完全可以正常安装系统，不需要特别设置。当然，如果有特殊需求可以直接点击[原博文](https://blog.csdn.net/Amorx12345/article/details/97510324)
查看如何自定义分区。

后面就是常规流程，重启，然后按提示拔下U盘。

## Step5 设置(个人踩坑点)
在按要求拔掉U盘之后我进入的是Windows系统，但是重新启动也没有进入引导菜单，按照[其他教程](https://blog.csdn.net/billy_chen_2013/article/details/17425363?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~first_rank_v2~rank_v25-6-17425363.nonecase&utm_term=easybcd%E8%AF%86%E5%88%AB%E4%B8%8D%E4%BA%86linux)
下载了EasyCBD之类的软件，然而并没有什么卵用😥于是自己摸索了一下，发现只要再进到bios里，然后到Boot栏目，找到**UEFI Hard Disk Drive BBS Priorities**
然后把Ubuntu移到windows上面就可以了，保存然后退出就可以进入到正常的引导界面。

打开terminal，自动获取更新。

```terminal
sudo apt update
sudo apt-get update
sudo apt-get upgrade
```

## 结语
重新看了一下流程感觉也没很多东西，但当时还是折腾了很久……之后再更新一篇用hexo+github.io+travisci搭博客的记录，差不多暑假为数不多干的有意义的事情就这些了。
讲不定之后再写个十三机兵防卫圈的安利文（迫真）。我以后应该每个月更新一次刷洛谷还有leetcode的进度才是。

# GistLib
Gist库
### msi-GE实现Win键与Fn键对换

> 微星的GE系列有个脑残设计, 所谓为了防止游戏玩家误按Win键而切出游戏, 将win键移到右侧边, 导致使用win[快捷键和Fn键调音量必须双手使用, 严重影响日常快捷键使用
显然小脑发育正常的游戏玩家都不可能误触Win键(大小手感显然不同), 如果要是能按错我看玩游戏也悬, 应该尽快去医院治疗一下.(笔者吐槽)
去年看到了一篇文章, 给我指明了思路, 可以通过改EC的键映射表来实现改键. 查了资料, 顺便弄到个GE60的改完的现成EC参照, 没几天就搞好了.
虽然也是依葫芦画瓢, 不求甚解, 但确实有效. 一直想找个时间整理一下, 但拖延症直到现在才腾出时间写一篇文章
本篇文章不会写的太详细, 毕竟只是提供一个成功案例. 原理和修改方法请参考下面提供的参考文章或Google.

#### 步骤:

1. 键帽对换

 - 抠起键帽的一角
 - 伸入拆机工具
 - 对X架施加力度下压
 - 抠出键帽

#### 结构参考: (点击图片可看原图)
<img src="https://raw.githubusercontent.com/muink/GistLib/master/Swapping-Win-key-and-the-Fn-key-on-msi-GE/img/1.png" width = "289" height = "300" alt="图片1" align=center />
<img src="https://raw.githubusercontent.com/muink/GistLib/master/Swapping-Win-key-and-the-Fn-key-on-msi-GE/img/2.png" width = "255" height = "300" alt="图片2" align=center />
<img src="https://raw.githubusercontent.com/muink/GistLib/master/Swapping-Win-key-and-the-Fn-key-on-msi-GE/img/2.png" width = "329" height = "300" alt="图片3" align=center />

--------

2. 固件键位映射对换

+ 前期准备
 - 重启电脑, 在出现msi图标时按`Delete`进入Bios
 - 在`Main`页面移动方向键选择`System Information`, 回车进入
 - 找到`EC Version`项, 记录下后面的值, 之后有用
 - 打开msi官网, 右上角选择产品型号对应的国别
 - 点开技术支持/Service
 - 左下角选择下载 & 手册/Download & Manual
 - 选择完型号后点击右侧下载
 - 点击固件/Firmware并选择对应操作系统版本
 - 找到`如何更新固件,请参阅说明文件。`, 下载说明文件
 - 找到`EC Firmware`, 用`F12`审查下载按钮, 找到并复制下载连接
 - 修改复制的连接地址后面的版本号, 使其对应自己机器的`EC Version`, 然后使用下载工具下载

+ 备份原机EC
 - 解压下载的zip文件, 打开其中的`Win`文件夹
 - 用记事本打开`config.ini`, 修改`BackUp=0`字段为`BackUp=1`, 然后保存将启用备份功能
 - 打开`winflash.exe`点击`Start`, 弹出`Do you want to backup EC F/W?`, 点击确认进行Backup(备份期间风扇会失去速度控制, 属正常情况)
 - 备份完成后会在`winflash.exe`所在目录生成`BACKUP.bin`文件(此文件应该与下载来的EC固件拥有相同的Hash值)

+ 在按键映射表中修改Win/Fn键的映射
 - 使用16进制编辑器(Uedit或Winhex/https://www.mh-nexus.de)打开EC固件文件
 - 互换按键映射表中的`0x9C`和`0x96`这两个字节(详细参考[以下文件][Comp]和[这篇文章][Smth]) (倒数第2节，搜索16进制B396和9CBD）
 - 注：原地址不可访问了.用16进程编辑器打开以后做下对比(请查看我上传的图片）
<img src="https://raw.githubusercontent.com/youwi/GistLib/master/Swapping-Win-key-and-the-Fn-key-on-msi-GE/img/find-key2.png" width = "329" height = "300" alt="图片3" align=center /> 
<img src="https://raw.githubusercontent.com/youwi/GistLib/master/Swapping-Win-key-and-the-Fn-key-on-msi-GE/img/find-keys.png" width = "329" height = "300" alt="图片3" align=center /> 
 
+ 回刷EC
 - 使用修改完的EC文件替换下载的EC文件, 按照说明文件的步骤更新修改后的EC(刷入期间风扇会失去速度控制, 属正常情况)

--------

[Comp]: https://github.com/muink/GistLib/tree/master/Swapping-Win-key-and-the-Fn-key-on-msi-GE/files  
[Smth]: http://ar.newsmth.net/thread-11b0f745c7f49ec-1.html "微星这个混蛋，把Win键放在右边-水木社区"

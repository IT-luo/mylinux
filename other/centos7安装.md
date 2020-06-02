## CentOS 7.5系统安装

### 前言

提前做好系统分区，其中swap的话，若你的内存只有1G或者2G的话Swap是内存的1.5、2倍即可。

 	用到的工具和软件：
		VMware workstation 12 Pro
		CentOS 7.5 

	系统分区安排
		/boot    1G
		/		 50G
		/data    30G
		swap	 3G

### 1、打开虚拟机首页选择创建新的虚拟机。
![](https://i.imgur.com/kBI4chz.jpg)


### 2、选择第一个典型即可、下一步。
![](https://i.imgur.com/3UBkybU.jpg)

### 3、安装来源，选择稍后安装操作系统
![](https://i.imgur.com/alA6ut5.jpg)

### 4、在这块选择linux ---> CentOS 64位
![](https://i.imgur.com/JUNupvq.jpg)

### 5、虚拟机名称使用默认即可，存放路径自己指定即可。
![](https://i.imgur.com/4CNWwlt.jpg)

### 6、硬盘大小这里最好大一点，即使硬盘空间没有这么多也不用怕，它只是用到实际使用的大小，选择将虚拟磁盘存储为单个文件，后续看起来比较整洁些，没什么乱七八糟的文件
![](https://i.imgur.com/OL58Wvt.jpg)

### 7、这一块的话虚拟机的基本配置已经完成，然后修改虚拟机具体配置可以点自定义配置或者点完成，后续再配置虚拟机。
![](https://i.imgur.com/3G1qeO1.jpg)

### 8、编辑虚拟机配置，这里是1核1G的配置，为了后续实验能正常进行这里给改成双核2G的配置，配置完之后就可以开启虚拟机装系统
![](https://i.imgur.com/aEl8vVX.jpg)
![](https://i.imgur.com/cpt0xes.jpg)

### 9、安装选项，这块就选择第一项即可，然后按回车键(Enter)

> 1、 安装图形界面

> 2、测试并安装纯字符命令界面

> 3、里面包含有救援模式等选项

![](https://i.imgur.com/ud92XeF.jpg)

### 10、选择安装语言、用默认即可不要选中文安装否则后面在做操作的时候会出现问题
![](https://i.imgur.com/gA9i0fS.jpg)

### 11、修改时区为亚洲上海、安装方式为GNOME Destktop
![](https://i.imgur.com/VbtV2s6.jpg)

### 12、硬盘分区
![](https://i.imgur.com/FPGHPkz.jpg)
![](https://i.imgur.com/v4UGvkD.jpg)
![](https://i.imgur.com/HQzRkzM.jpg)
![](https://i.imgur.com/udqBmPA.jpg)

### 13、网络配置，选择自动获取即可、修改主机名为centos7，后面kdump、security policy 默认即可、然后点击begin installation进行安装
![](https://i.imgur.com/yxdj2Cc.jpg)
![](https://i.imgur.com/Qc6iDK3.jpg)

### 14、最后一项给root 配置密码、创建普通用户，创建普通用户待到系统创建完之后再建也行。最后等待系统安装好！
![](https://i.imgur.com/s885vru.jpg)
![](https://i.imgur.com/OmNyhdX.jpg)
![](https://i.imgur.com/WNZwxix.jpg)

### 15、最后完成安装，reboot即可
![](https://i.imgur.com/OB7swfi.jpg)





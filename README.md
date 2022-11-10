# klipper-on-android
Run klipper, moonraker, fluidd, KlipperScreen one-click configuration script on android system.


前言：

1.本教程中安卓系统必须root。因每种手机硬件和系统的root方法各有不同，网络上教程很多，这里不再赘述。

2.本教程软硬件环境：

  小米2S手机。
  
  16G机身存储，2G运行内存。
		
  运行基于android9的魔趣9.0操作系统。
		
  系统已使用魔趣官方补丁进行root。
		
  刷机及系统root参考官方教程：
		
  ROM下载与刷机：https://download.mokeedev.com/aries.html
		
  获取root权限：https://bbs.mokeedev.com/t/topic/6577
		
3.理论上只要能root的安卓手机此教程都能适用，待测试。	
	  
4.理论上，在termux里安装proot容器后安装的debian系统也可以使用，待测试。
	  
	  
本教程特点：

1.使用一个单独的APP在图形界面下安装linux系统，配置系统自动重启，系统中文汉化等。方便管理和使用。

2.klipper系统依旧使用流行的kiauh脚本进行安装，升级或卸载。手机klipperscreen界面或者网页界面都可以进行升级操作。

3.使用一键脚本对klipper全家桶进行针对android系统的兼容配置，方便快捷。
			
4.结合我自己的使用经验，修改定制了klipper全家桶的基本配置文件，关键操作更实用。
           								
5.安装好以后无任何报错，可单独控制klipper，moonraker等服务启动，重启，关闭。
			
6.使用体验与树莓派基本没有区别，包括使用输入整形与压力补偿。
			
################################################################################################################################			  

具体安装步骤：

本教程假设安卓手机已root！！！！！！

安装过程较长，手机需要连接到充电器！！！！！！

需要用的软件：

手机端：

XServer-XSDL-1.20.51.apk（必装） 下载链接：https://sourceforge.net/projects/libsdl-android/files/apk/XServer-XSDL/
    
linuxdeploy-2.6.0-259.apk（必装） 下载链接：https://github.com/meefik/linuxdeploy/releases/download/2.6.0/linuxdeploy-2.6.0-259.apk

kerneladiutor_248.apk（开启所有CPU核心，推荐安装） 下载链接：https://f-droid.org/en/packages/com.nhellfire.kerneladiutor/

termux_118.apk（选装，需要时再安装）下载链接：https://github.com/termux/termux-app/releases/tag/v0.118.0
		
电脑端：

Xshell（必装）官网地址：https://www.netsarang.com/en/xshell/

1.安装kerneladiutor

高通处理器默认有个MPD功耗控制方案，默认情况下会关闭部分CPU核心来控制功耗。
由此带来的最大的问题就是在debian系统里会发现4核心的处理器大多数情况下却只识别出2个核心。
kerneladiutor是安卓系统的内核管理软件，用来调整CPU和GPU的频率和性能。可以强制开启所有CPU核心，充分利用手机的性能。。

2.安装linuxdeploy

根据系统提示安装apk文件，安装完成后打开软件点击主界面左上角“三横杠”打开软件设置：

锁定Wi-Fi （勾选）

CPU唤醒   （勾选）

开机自启动（勾选）

语言      （简体中文）

启动延迟  （5）  （以秒为单位，根据需要设置，想迟一点启动就将数字调大）

联网更新  （勾选）

其他选项保持默认！！！！！！！！！！

！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！


3.安装debian系统

设置好后回到主界面，点击右下角“三横杠滑动开关”打开linux系统安装配置。

发行版 （Debian）

架构（armhf）（自行查询自己手机处理器型号，如果支持64位就选arm64）

发行版本 （stable）

源地址 （ http://ftp.cn.debian.org/debian/ ）（国内源安装更快）

安装类型 （目录）

安装路径 （/data/debian11）

用户名 （print3D）（因为涉及到klipper配置文件的路径，所以建议就用这个用户名）

密码：（123456）（随便设置，能记住就行）

本地化 （zh-CN.UTF-8）（系统汉化，便于使用）

初始化 （启用）

初始化系统 （sysV）

挂载点 （启用）

挂载点列表 （/sdcard/-/mnt/sdcard/）（可选，一般用不着）

SSH （启用）（必须开启，否则连不上linux系统）

图形界面 （启用）

图形子系统 （X11）

图形界面设置 （取消勾选自动打开XServer-XSDL）（一键脚本里已经配置自动开启XServer-XSDL客户端，此处不需要勾选）

桌面环境 （XTerm）

其他选项保持默认！！！！！！！！！！

！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！

配置完成后回到主界面

点击右上角“三个竖点” → 安装 （安装过程比较漫长，根据网络情况大概在5-15分钟）

安装完成后点击主界面下方“停止”后再点击“启动”（这一步不能少，否则debian系统启动不了）

系统启动完成后使用Xshell连接linux系统（IP地址显示在linuxdeploy软件主界面最上方，登录名和密码就是上面步骤里设置的，端口22，协议ssh）

至此，debian系统安装完成！！！！！！！！！！！！！！！！

！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！


4.klipper安装环境配置

ssh登录进入debian系统后执行以下命令：

sudo usermod -a -G aid_inet,aid_net_raw root
###此命令可以解决使用sudo命令时root用户无法联网的问题

sudo apt update  
###更新系统软件包

sudo apt install -y git vim
###安装必要的git和vim工具软件

5.使用kiauh安装klipper

cd ~
###进入登录用户家目录

git clone https://github.com/th33xitus/kiauh.git
###官方kiauh脚本地址

git clone https://gitee.com/miroky/kiauh.git
###国内kiauh脚本地址（与上面官方地址二选一即可）

###需要安装klipper，moonraker，fluidd（一键脚本暂时不支持Mainsail配置），KlipperScreen 这4个组件。
###每安装完一个组件都会提示无法启动服务，这是安卓系统的原因，不用管它，如果能启动起来就不用一键脚本去配置了。
###组件安装涉及部分编译过程，耗时较长，耐心等待。只要是每个脚本都能自动安装到最后，基本就没有问题。

6.klipper全家桶安卓与安卓系统兼容配置

打开 → 小米2S安卓klipper安装教程 → klipper配置文件

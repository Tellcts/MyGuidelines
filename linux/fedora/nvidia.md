# <center>英伟达显卡驱动的安装</center>



## 1. 前置条件

- **确保显卡硬件版本能够适配仓库中的驱动**
- **关闭系统的安全启动（进入BIOS设置）**
- **启用了RPM Fusion仓库**



## 2. 命令

~~~bash
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
~~~



## 3. 重启系统
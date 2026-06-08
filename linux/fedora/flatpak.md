# <center>Flatpak的配置</center>



# 1. 确保已经安装`flatpak`

~~~bash
sudo dnf update && sudo dnf install flatpak
~~~



## 2. 配置国内的`flathub`镜像源（USTC）

~~~bash
# FIRST
flatpak remote-add --if-not-exists flathub                                          https://dl.flathub.org/repo/flathub.flatpakrepo

# SECOND
sudo flatpak remote-modify flathub --url=https://mirrors.ustc.edu.cn/flathub

# 恢复官方源
sudo flatpak remote-modify flathub --url=https://dl.flathub.org/repo
~~~



## 3. 更新`flatpak`应用

~~~bash
flatpak update
~~~



## 4. 安装`Typora`

~~~bash
flatpak install flathub io.typora.Typora
~~~


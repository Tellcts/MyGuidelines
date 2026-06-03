# Linux上相关字体安装



## **1. JetBrains Mono Nerd Font**

**获取方式：**

- **GitHub CLI**

~~~bash
gh release download --repo ryanoasis/nerd-fonts -p "JetBrainsMono.tar.xz" -D ~/下载 

cd ~/下载 && tar -xf JetBrainsMono.tar.xz -C ./JetBrainsMono

mkdir -p ~/.local/share/fonts && cp ~/下载/JetBrainsMono/* ~/.local/share/fonts/
# 更新字体缓存
fc-cache -fv
~~~

- **curl**

~~~bash
~~~






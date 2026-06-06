# <center>UV使用指南</center>



## 一、UV的安装

- **Linux or MacOS**

  ~~~bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  # OR
  wget -qO- https://astral.sh/uv/install.sh | sh
  ~~~

- **Windows**

  ~~~bash
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
  ~~~

  

## 二、UV的升级与卸载

~~~bash
# 升级
uv self update

# 卸载
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"
rm ~/.local/bin/uv ~/.local/bin/uvx
~~~



## 三、开启UV和UVX的Shell自动补全功能

~~~bash
# Bash
echo 'eval "$(uv generate-shell-completion bash)"' >> ~/.bashrc
echo 'eval "$(uvx --generate-shell-completion bash)"' >> ~/.bashrc

# Zsh
echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc
echo 'eval "$(uvx --generate-shell-completion zsh)"' >> ~/.zshrc
~~~



## 四、Python版本管理

~~~bash
# 安装、卸载Python版本
uv python install <version>
uv python uninstall <version>

# 查看可用的Python版本
uv python list

# 将所在项目的Python版本固定至指定值
uv python pin <version>

# 查找已安装的Python版本
uv python find 
~~~




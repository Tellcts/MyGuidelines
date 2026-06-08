# <center>Starship配置文件</center>

## 1. 文件路径

~~~plain
~/.config/starship.toml
~~~



## 2. 配置文件内容

~~~toml
# 根据 schema 提供自动补全
"$schema" = 'https://starship.rs/config-schema.json'

# 在提示符之间插入空行
add_newline = true
command_timeout = 500

# 提示符样式：['❯','➜']
[character]
success_symbol = '[❯](bold green)'
error_symbol = '[❯](bold red)'

# 目录显示设置
[directory]
truncation_length = 4
truncate_to_repo = true

# 禁用 'package' 组件，将其隐藏
[package]
disabled = true

# 命令耗时
[cmd_duration]
min_time = 2000
format = '⏳ [$duration]($style)'
~~~


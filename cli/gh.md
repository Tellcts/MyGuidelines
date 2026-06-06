# <center>**GitHub CLI使用指南**</center>



## **一、身份认证相关**

~~~bash
# 登录GitHub CLI
gh auth login

# 登出GitHub CLI
gh auth logout

# 打印当前所使用的token
gh auth token

# 展示当前账号登录状态
gh auth status

# 刷新授权令牌/权限
gh auth refresh

# 让git使用gh的登陆session，免配置ssh或者密码登录
gh auth setup-git
~~~



## 二、仓库操作相关

### 1.仓库基础操作

~~~bash
# 交互式创建git仓库，可选将本地仓库推送至远程
gh repo create
~~~

### 2.仓库克隆操作

~~~bash
# 克隆GitHub存储库,如果没有OWNER参数则克隆当前账户的仓库
gh repo clone OWNER/REPO

# 通过URL克隆仓库
gh repo clone https://github.com/cli/cli.git
gh repo clone git@github.com:cli/cli.git

# 克隆仓库文件到指定目录
gh repo clone cli/cli /path/to/your/target

# 克隆时传递git clone参数
gh repo clone cli/cli -- --depth=1

# 克隆远程仓库时不添加上游仓库，默认会将上游仓库作为origin
gh repo clone cli/cli --no-upstream
~~~

### 3.仓库fork操作

~~~bash
~~~

### 4.仓库删除操作

~~~bash
# 交互式删除指定远程仓库，不带参数就删除当前仓库所对应的远程仓库
gh repo delete REPO

# 无交互性提示，直接删除
gh repo delete REPO --yes
~~~

### 5. 仓库查看操作

~~~bash
# 默认查看当前所在仓库元信息
gh repo view

# 查看指定仓库元信息
gh repo view OWNER/REPO
gh repo view git@github.com:cli/cli.git
gh repo view https://github.com/cli/cli.git

# 查看仓库指定字段值
gh repo view cli/cli --json stargazerCount # star数量
gh repo view cli/cli --json forkCount 	   # fork仓库数量
gh repo view cli/cli --json languages	   # 使用语言概览
gh repo view cli/cli --json url/sshUrl	   # 获取https/ssh url
gh repo view cli/cli --json latestRelease  # 获取最新发布信息

# 同时查看多个字段值
gh repo view cli/cli --json stargazerCount,forkCount

# 配合jq表达式,必须具有--json参数
gh repo view cli/cli --json forkCount --jq <expression>
~~~



## 三、搜索操作相关

### 1. 搜索仓库

~~~bash
# 通过关键词搜索仓库
gh search repos "clash-verge-rev" --sort=stars --order=[desc|asc] --limit=10
~~~

### 2. 搜索代码

~~~bash
gh search code "fn main(){}" --limit=10
~~~

### 3. 搜索PR

~~~bash
gh search prs "style/format-code" --limit=10 --author=Tellcts
~~~

### 4. 搜索Issues

~~~bash
gh search issues "clash" --limit=10 --author=Tellcts
~~~

### 5. 搜索提交

~~~bash
gh search commits "write: gh.md" --limit=10 --author=Tellcts
~~~



## 四、Release相关操作

### 1. 基础操作

~~~bash
# 查看当前所在仓库的Release信息
gh release list
~~~

### 2. 创建/删除 Release

~~~bash
# 交互式创建Release
gh release create v1.0.0 

# 非交互式创建Release
gh release create v1.0.0 --notes="bugs fix"
gh release create v1.0.0 --generate-notes # 提交历史链接作为notes
gh release create v1.0.0 -F changelog.md  # 将指定文件的内容作为Release Notes
gh release create v1.0.0 ./dist/*.tar.gz  # 上传Release包，未指定默认仓库源码压缩包
gh release create v1.0.0 './dist/test.tar.gz#linux-amd64.tar.gz'

# 删除Release,--yes非交互式直接删除，可指定具体仓库
gh release delete v1.0.0 --cleanup-tag --yes [-R|--repo OWNER/REPO]

# 删除Release中某个asset
gh release delete-asset v1.0.0 'test.tar.xz' --yes [-R|--repo OWNER/REPO]
~~~

### 3. 下载Release

~~~bash
# 不带tag默认下载latest版本且必须指定pattern,-D指定下载目录
gh release download [tag] -p '*.deb' -p '*.rpm' -D '~/下载'
~~~


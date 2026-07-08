# <center>Git 命令完全指南</center>

> 按常用程度排列，配合通俗易懂的例子。  
> `[]` 表示可选参数，`<>` 表示必填参数（实际使用时去掉尖括号）。

---

## 一、配置与初始化

### 1. `git config` — 配置 Git

```bash
# 设置全局用户名和邮箱（第一次用 Git 必做）
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@example.com"

# 设置当前仓库的用户名和邮箱（覆盖全局）
git config user.name "公司用户名"
git config user.email "公司邮箱@example.com"

# 查看所有配置
git config --list

# 查看全局配置
git config --global --list

# 查看某个配置项
git config user.name
git config --global user.email

# 设置默认分支名为 main
git config --global init.defaultBranch main

# 设置 Git 别名（偷懒必备）
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
# 以后 git st 就等于 git status

# 设置中文乱码解决
git config --global core.quotepath false
git config --global i18n.logoutputencoding utf-8

# 设置代理
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
# 取消代理
git config --global --unset http.proxy
```

### 2. `git init` — 初始化仓库

```bash
# 在当前目录初始化 Git 仓库
git init

# 初始化时指定默认分支名
git init --initial-branch=main

# 创建裸仓库（常用于服务器端）
git init --bare
```

### 3. `git clone` — 克隆远程仓库

```bash
# 克隆仓库到当前目录
git clone https://github.com/用户名/仓库名.git

# 克隆到指定目录
git clone https://github.com/用户名/仓库名.git 我的文件夹

# 浅克隆（只克隆最近 1 次提交，速度快）
git clone --depth=1 https://github.com/用户名/仓库名.git

# 克隆指定分支
git clone -b main https://github.com/用户名/仓库名.git

# 克隆时也克隆子模块
git clone --recurse-submodules https://github.com/用户名/仓库名.git
```

---

## 二、日常开发（每日必用）

### 4. `git status` — 查看工作区状态

```bash
# 查看文件变更状态（最常用的命令）
git status

# 以简洁模式显示
git status -s
# 输出示例：
#  M README.md    ← 已修改但未暂存（M 在右边）
# M  index.js     ← 已暂存（M 在左边）
# ?? newfile.txt  ← 未跟踪的文件

# 查看具体哪些文件变更了
git status -u
```

### 5. `git add` — 将修改添加到暂存区

```bash
# 添加单个文件
git add index.js

# 添加所有变更（最常用）
git add .
git add -A    # 等价于 git add --all
git add --all

# 添加当前目录的所有变更（不包括上级目录）
git add *

# 交互式添加（只添加文件的部分改动）
git add -p    # 逐一确认每个改动块，y=是 n=否 s=拆分

# 添加所有 .txt 文件
git add *.txt

# 撤销对某个文件的 add（文件从暂存区移回工作区）
git reset HEAD index.js
```

### 6. `git commit` — 提交暂存区的更改

```bash
# 提交暂存区的更改
git commit -m "feat: 添加用户登录功能"

# 跳过暂存区，直接提交已跟踪文件的修改
git commit -a -m "fix: 修复空指针异常"

# 修改上一次提交（还没 push 时用）
git commit --amend -m "修正后的提交信息"

# 修改上一次提交并保留原提交说明
git commit --amend --no-edit

# 使用编辑器编写详细的提交信息
git commit
# 会打开 vim/编辑器，第一行写标题，空一行后写详细说明
```

### 7. `git log` — 查看提交历史

```bash
# 查看提交历史
git log

# 单行显示（最常用）
git log --oneline
# 输出示例：
# a1b2c3d feat: 添加登录页面
# e4f5g6h fix: 修复样式问题
# i7j8k9l docs: 更新README

# 图形化显示分支历史
git log --oneline --graph --all
# 输出示例：
# *   a1b2c3d (HEAD -> main) 合并分支
# |\
# | * e4f5g6h (feature) 新功能开发
# * | i7j8k9d 主分支提交
# |/

# 显示最近 N 条提交
git log -5

# 显示每次提交的改动内容
git log -p

# 按作者筛选
git log --author="张三"

# 按日期筛选
git log --after="2025-01-01"
git log --before="2025-12-31"

# 搜索提交信息
git log --grep="登录"

# 查看某个文件的提交历史
git log -- README.md

# 可视化分支和标签
git log --oneline --graph --all --decorate
```

### 8. `git diff` — 查看文件差异

```bash
# 查看工作区与暂存区的差异（还没 add 的改动）
git diff

# 查看暂存区与上次提交的差异（已 add 但没 commit 的改动）
git diff --staged
git diff --cached    # 同上

# 查看工作区与上次提交的差异
git diff HEAD

# 查看两个分支的差异
git diff main feature

# 查看两个分支的差异，只显示文件名
git diff main feature --stat

# 查看特定文件的差异
git diff --  README.md

# 查看两个提交之间的差异
git diff a1b2c3d..e4f5g6h
```

---

## 三、撤销与恢复

### 9. `git restore` — 撤销修改（Git 2.23+ 推荐）

```bash
# 丢弃工作区的修改（危险！不可恢复）
git restore index.js

# 撤销所有文件的未暂存修改
git restore .

# 从暂存区撤回（等价于 git reset HEAD file）
git restore --staged index.js

# 从暂存区撤回 + 丢弃工作区修改
git restore --staged --worktree index.js

# 恢复到某个历史版本的内容
git restore --source=a1b2c3d index.js
```

### 10. `git reset` — 回退版本

```bash
# 从暂存区撤回文件，不改变工作区（保留修改）
git reset HEAD index.js
git reset index.js            # 简写

# 软回退：移动 HEAD，保留工作区和暂存区的修改
git reset --soft HEAD~1       # 回到上一个提交，但保留改动

# 混合回退（默认）：移动 HEAD，保留工作区，清空暂存区
git reset --mixed HEAD~1

# 硬回退：移动 HEAD，丢弃所有修改（危险！慎用）
git reset --hard HEAD~1       # 回到上一个提交，丢弃所有改动

# 回退到特定提交
git reset --hard a1b2c3d

# 回退文件到某次提交的版本
git reset a1b2c3d -- index.js
```

### 11. `git revert` — 安全地撤销某次提交

```bash
# 撤销某次提交（生成一次新的反向提交，推荐用于公共分支）
git revert a1b2c3d

# 撤销但只暂存，不自动提交
git revert --no-commit a1b2c3d
git revert -n a1b2c3d

# 撤销多个提交
git revert a1b2c3d..e4f5g6h
```

### 12. `git clean` — 清理未跟踪文件

```bash
# 查看会被删除的未跟踪文件（演习）
git clean -n

# 删除未跟踪文件
git clean -f

# 删除未跟踪的目录
git clean -fd

# 连 .gitignore 中忽略的文件也删除
git clean -xf
```

---

## 四、分支管理

### 13. `git branch` — 分支管理

```bash
# 列出本地分支（当前分支前有 * 号）
git branch

# 列出所有分支（含远程）
git branch -a

# 创建分支
git branch feature-login

# 创建分支并切换到该分支
git branch -m feature-login       # 重命名当前分支
git branch -m old-name new-name   # 重命名指定分支

# 删除已合并的分支
git branch -d feature-login

# 强制删除分支（即使未合并）
git branch -D feature-login

# 查看哪些分支已合并到当前分支
git branch --merged

# 查看哪些分支未合并到当前分支
git branch --no-merged

# 查看分支与远程的跟踪关系
git branch -vv
```

### 14. `git checkout` — 切换分支 / 恢复文件（传统方式）

```bash
# 切换到已有分支
git checkout main

# 创建并切换分支（传统方式）
git checkout -b feature-login

# 切换到某个提交（进入"分离头指针"状态，一般不推荐）
git checkout a1b2c3d

# 丢弃工作区的修改（传统方式）
git checkout -- index.js
```

### 15. `git switch` — 切换分支（Git 2.23+ 推荐）

```bash
# 切换到已有分支
git switch main

# 创建并切换到新分支
git switch -c feature-login

# 切换到上一个分支
git switch -
```

### 16. `git merge` — 合并分支

```bash
# 将 feature 分支合并到当前分支
git switch main           # 先切换到 main
git merge feature        # 再把 feature 合并进来

# 创建合并提交（即使可以快进合并）
git merge --no-ff feature

# 压缩合并（将 feature 的所有提交压缩成一个）
git merge --squash feature

# 合并时只快进，不创建合并提交（如果无法快进就报错）
git merge --ff-only feature

# 中止合并（遇到冲突时）
git merge --abort

# 使用可视化合并工具
git mergetool
```

### 17. `git rebase` — 变基（整理提交历史）

```bash
# 将当前分支的提交移到目标分支的顶端
git switch feature
git rebase main
# 效果：feature 以 main 的最新提交为基，看起来像是从 main 最新处拉的分支

# 交互式变基（修改历史，最常用）
git rebase -i HEAD~3
# 会打开编辑器，可以：
# pick    = 保留该提交
# reword  = 修改提交信息
# edit    = 修改提交内容
# squash  = 合并到上一个提交
# fixup   = 合并到上一个提交（丢弃提交信息）
# drop    = 删除该提交

# 在变基中跳过某个提交
git rebase --skip

# 中止变基（回到变基前的状态）
git rebase --abort

# 变基完成后继续
git rebase --continue

# 从 main 变基，但只处理最近 3 个提交
git rebase -i main~3
```

---

## 五、远程仓库

### 18. `git remote` — 管理远程仓库

```bash
# 查看远程仓库
git remote
# 输出：origin

# 查看远程仓库地址
git remote -v
# 输出：
# origin  https://github.com/用户名/仓库名.git (fetch)
# origin  https://github.com/用户名/仓库名.git (push)

# 添加远程仓库
git remote add origin https://github.com/用户名/仓库名.git

# 修改远程仓库地址
git remote set-url origin https://github.com/新地址/仓库名.git

# 将远程地址从 HTTPS 改为 SSH
git remote set-url origin git@github.com:用户名/仓库名.git

# 删除远程仓库
git remote remove origin

# 重命名远程仓库
git remote rename origin upstream

# 查看某个远程仓库的详细信息
git remote show origin
```

### 19. `git push` — 推送至远程仓库

```bash
# 推送当前分支到远程
git push origin main

# 推送并设置上游（以后可以直接 git push）
git push -u origin main

# 推送所有分支
git push --all origin

# 推送标签
git push origin --tags

# 强制推送（危险！会覆盖远程历史，慎用）
git push --force origin main

# 更安全的强制推送（远程也有新增时会被拒绝）
git push --force-with-lease origin main

# 删除远程分支
git push origin --delete feature-login

# 删除远程标签
git push origin --delete v1.0.0

# 将本地分支与远程不同名时推送
git push origin local-branch:remote-branch

# 推送并设置上游（简写，如果 remote 的默认分支一致）
git push -u origin HEAD
```

### 20. `git pull` — 拉取远程并合并

```bash
# 拉取远程并自动合并（相当于 git fetch + git merge）
git pull origin main

# 拉取远程并变基（相当于 git fetch + git rebase，推荐）
git pull --rebase origin main

# 设置当前分支的 pull 行为为 rebase
git config branch.main.rebase true
# 设置全局 pull 行为为 rebase
git config --global pull.rebase true

# 只拉取不合并
git fetch origin

# 拉取时也获取子模块更新
git pull --recurse-submodules
```

### 21. `git fetch` — 获取远程更新但不合并

```bash
# 获取远程所有分支的更新
git fetch origin

# 获取远程所有信息（包括标签）
git fetch --all

# 只获取某个远程分支
git fetch origin main

# 拉取后清理本地已不存在的远程分支引用
git fetch --prune

# 获取远程标签
git fetch --tags
```

---

## 六、储藏与临时切换

### 22. `git stash` — 暂存工作区修改

```bash
# 暂存当前工作区的修改（工作区变干净）
git stash
git stash push -m "正在开发的登录功能"

# 查看暂存列表
git stash list
# 输出：
# stash@{0}: On main: 正在开发的登录功能
# stash@{1}: WIP on feature: a1b2c3d

# 恢复最近一次暂存（并从列表中删除）
git stash pop

# 恢复指定暂存（不删除列表中的记录）
git stash apply stash@{1}

# 恢复最近一次暂存（但不删除列表中的记录）
git stash apply

# 删除最近一次暂存
git stash drop

# 删除所有暂存
git stash clear

# 创建一个新分支并应用暂存（解决冲突的好办法）
git stash branch new-branch stash@{0}
```

---

## 七、标签管理

### 23. `git tag` — 标签管理

```bash
# 列出所有标签
git tag
git tag -l "v1.*"    # 通配符筛选

# 创建轻量标签
git tag v1.0.0

# 创建附注标签（推荐，包含作者、日期、说明）
git tag -a v1.0.0 -m "正式版 v1.0.0"

# 给历史提交打标签
git tag -a v0.9.0 a1b2c3d -m "候选版 v0.9.0"

# 查看标签详细信息
git show v1.0.0

# 推送单个标签到远程
git push origin v1.0.0

# 推送所有标签到远程
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0    # 传统方式

# 切换到标签（进入分离头指针）
git checkout v1.0.0
```

---

## 八、高级分支操作

### 24. `git cherry-pick` — 挑选其他分支的提交

```bash
# 将某个提交应用到当前分支
git cherry-pick a1b2c3d

# 挑选多个连续提交
git cherry-pick a1b2c3d..e4f5g6h

# 挑选但不自动提交
git cherry-pick -n a1b2c3d

# 添加提交信息
git cherry-pick a1b2c3d -x    # 在提交信息中标注来源
```

### 25. `git rm` — 删除文件

```bash
# 删除文件并暂存删除操作
git rm index.js

# 从 Git 中删除但保留工作区文件（不再跟踪）
git rm --cached .env
git rm --cached -r node_modules/   # 删除整个目录

# 递归删除
git rm -r src/old-module/
```

### 26. `git mv` — 移动/重命名文件

```bash
# 重命名文件
git mv old.js new.js

# 移动文件到目录
git mv style.css src/
```

---

## 九、调试与排查

### 27. `git blame` — 查看每行代码的最后修改者

```bash
# 查看文件的每行是谁改的
git blame index.js
# 输出：
# a1b2c3d (张三 2025-03-15 10:30:00 +0800 10) const x = 1
# e4f5g6h (李四 2025-03-16 14:20:00 +0800 11) const y = 2

# 显示指定行范围
git blame -L 10,20 index.js

# 忽略空格变更
git blame -w index.js

# 显示修改的提交信息和作者邮箱
git blame -e index.js
```

### 28. `git bisect` — 二分查找引入 Bug 的提交

```bash
# 开始二分查找
git bisect start

# 标记当前提交为"有 Bug"
git bisect bad

# 标记某个历史版本为"无 Bug"
git bisect good v1.0.0
# Git 会检出中间的一个提交，你测试后标记：
git bisect good      # 如果这个版本没问题
git bisect bad       # 如果这个版本有 Bug

# 重复直到找到第一个有 Bug 的提交

# 自动二分查找（运行脚本测试）
git bisect run npm test

# 结束二分查找
git bisect reset
```

### 29. `git grep` — 在 Git 管理的文件中搜索

```bash
# 搜索包含某个字符串的文件
git grep "TODO"
git grep "function"

# 搜索并显示行号
git grep -n "console.log"

# 只显示文件名
git grep -l "export default"

# 搜索某次提交中的内容
git grep "bug" a1b2c3d

# 大小写不敏感搜索
git grep -i "user"

# 或运算搜索
git grep -e "TODO" --or -e "FIXME"
```

### 30. `git reflog` — 查看操作历史（后悔药）

```bash
# 查看所有 HEAD 移动历史（包括已删除的提交）
git reflog
# 输出：
# a1b2c3d HEAD@{0}: commit: 添加新功能
# e4f5g6h HEAD@{1}: checkout: 切换到 main
# i7j8k9 HEAD@{2}: reset: moving to HEAD~1
# 万一 reset --hard 后悔了，这里能找到原来的提交

# 回滚到某个历史状态
git reset --hard HEAD@{2}

# 只查看当前分支的 reflog
git reflog main

# 查看指定时间范围内的操作
git reflog --date=relative
```

---

## 十、子模块

### 31. `git submodule` — 管理子模块

```bash
# 添加子模块
git submodule add https://github.com/用户名/仓库名.git libs/依赖库

# 克隆含子模块的仓库（同时拉取子模块）
git clone --recurse-submodules https://github.com/用户名/父仓库.git

# 如果已克隆父仓库，再拉取子模块
git submodule init
git submodule update

# 更新子模块到最新提交
git submodule update --remote

# 迭代地拉取所有子模块
git submodule foreach git pull

# 移除子模块
git submodule deinit libs/依赖库
git rm libs/依赖库
```

---

## 十一、工作树

### 32. `git worktree` — 同时检出多个分支

```bash
# 在另一个目录检出分支（可以同时工作在不同分支）
git worktree add ../hotfix hotfix-branch

# 查看所有工作树
git worktree list
# 输出：
# /path/to/project        a1b2c3d [main]
# /path/to/hotfix         e4f5g6h [hotfix-branch]

# 移除工作树
git worktree remove ../hotfix

# 清理已删除的工作树记录
git worktree prune
```

---

## 十二、代码审查与补丁

### 33. `git shortlog` — 统计提交者

```bash
# 按作者统计提交数
git shortlog
# 输出：
# 张三 (10):
#       feat: 添加登录
#       fix: 修复样式
# 李四 (5):
#       docs: 更新文档

# 只统计邮箱
git shortlog -e

# 按提交数量排序
git shortlog -sn

# 按时间统计
git shortlog --after="2025-01-01" --before="2025-06-30"
```

### 34. `git format-patch` — 生成补丁文件

```bash
# 生成最近 N 次提交的补丁
git format-patch -3

# 生成两个版本之间的补丁
git format-patch v1.0.0..v1.1.0

# 生成某个分支独有的提交补丁
git format-patch main --stdout > feature.patch
```

### 35. `git am` — 应用补丁

```bash
# 应用某个补丁文件
git am 0001-添加新功能.patch

# 应用所有补丁
git am *.patch

# 跳过当前补丁
git am --skip

# 中止应用补丁
git am --abort
```

### 36. `git apply` — 应用 diff 但不生成提交

```bash
# 应用补丁但不当成提交
git apply fix.patch

# 检查补丁能否应用成功
git apply --check fix.patch
```

---

## 十三、归档与打包

### 37. `git archive` — 打包项目

```bash
# 将当前分支打包成 ZIP
git archive -o ../project.zip HEAD

# 打包指定分支
git archive -o ../feature.tar.gz feature

# 打包某次提交
git archive -o ../v1.0.0.zip v1.0.0

# 不包含 .gitignore 中忽略的文件
git archive -o ../project.zip --format=zip HEAD
```

### 38. `git bundle` — 离线传输（无网络也能传）

```bash
# 把整个分支打包成一个文件（可以 U 盘拷走）
git bundle create ../repo.bundle HEAD main

# 从 bundle 文件克隆
git clone ../repo.bundle

# 从 bundle 文件拉取
git fetch ../repo.bundle main:temp-branch
```

### 39. `git describe` — 生成可读的版本名

```bash
# 基于最近的标签描述当前提交
git describe
# 输出：v1.0.0-5-g a1b2c3d
# 含义：在 v1.0.0 标签之后又有了 5 次提交

# 带标签匹配
git describe --tags
```

---

## 十四、优化与维护

### 40. `git gc` — 垃圾回收

```bash
# 手动触发垃圾回收（压缩对象、优化存储）
git gc

# 更积极的回收（耗时更长，但更彻底）
git gc --aggressive

# Git 会在适当时自动执行 gc，通常不需要手动操作
```

### 41. `git maintenance` — 自动维护（Git 2.31+）

```bash
# 启动后台自动维护（推荐在大型仓库上启用）
git maintenance start

# 停止后台自动维护
git maintenance stop

# 手动执行维护（增量）
git maintenance run

# 执行全部维护任务
git maintenance run --task=gc
```

### 42. `git sparse-checkout` — 部分检出（大型仓库必备）

```bash
# 开启稀疏检出模式
git sparse-checkout init --cone

# 只检出指定目录
git sparse-checkout set src/components

# 添加更多目录
git sparse-checkout add src/utils

# 查看当前稀疏检出的目录
git sparse-checkout list

# 关闭稀疏检出（恢复全部）
git sparse-checkout disable
```

### 43. `git fsck` — 仓库完整性检查

```bash
# 检查 Git 仓库的对象完整性
git fsck

# 检查并显示丢失的对象
git fsck --lost-found

# 检查所有对象
git fsck --full
```

---

## 十五、其他实用命令

### 44. `git notes` — 为提交添加备注（不修改提交本身）

```bash
# 给某次提交添加备注
git notes add a1b2c3d -m "这个提交引入了后续要修改的重构"

# 查看备注
git notes show a1b2c3d

# 推送备注到远程
git push origin refs/notes/*

# 拉取远程备注
git fetch origin refs/notes/*:refs/notes/*
```

### 45. `git replace` — 替换提交对象

```bash
# 用一个提交替换另一个（不会改写历史，但效果上像改了）
git replace a1b2c3d e4f5g6h

# 列出所有替换
git replace -l
```

### 46. `git rerere` — 复用冲突解决方案

```bash
# 启用 rerere（记住冲突是如何解决的）
git config --global rerere.enabled true
# 下次遇到相同冲突时 Git 会自动应用之前的解决方案

# 查看已记录的冲突解决方案
git rerere status
```

### 47. `git instaweb` — 启动网页版 Git 浏览器

```bash
# 启动一个 Web 服务器浏览 Git 仓库
git instaweb

# 指定端口启动
git instaweb --port=8080
```

---

## 十六、Git 使用小贴士

### 常用别名配置

```bash
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.last "log -1 HEAD"
git config --global alias.unstage "restore --staged"
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.df diff
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

### .gitignore 快速参考

```gitignore
# 常见 .gitignore 规则

# 忽略编译产物
node_modules/
dist/
build/
*.pyc
__pycache__/

# 忽略环境配置
.env
.env.local
*.env

# 忽略系统文件
.DS_Store
Thumbs.db

# 忽略 IDE 配置（团队统一标准时可用）
.idea/
.vscode/
*.swp

# 忽略日志文件
*.log

# 忽略大文件
*.zip
*.tar.gz
*.7z
```

### 常用工作流速查

```bash
# 日常开发（未推送时）
git checkout -b my-feature      # 创建功能分支
git add .                       # 暂存
git commit -m "feat: xxx"      # 提交
git commit --amend             # 修正提交
git rebase -i HEAD~3           # 整理提交历史

# 同步上游更新
git fetch origin               # 获取远程更新
git rebase origin/main         # 变基到最新
# 或：git pull --rebase origin main

# 提交 PR 前
git checkout main
git pull --rebase
git checkout my-feature
git rebase main                # 确保基于最新 main

# 紧急修复（需要切分支）
git stash                      # 保存当前工作
git switch main                # 切到 main
git checkout -b hotfix         # 创建修复分支
# 修复完成后...
git switch 原分支
git stash pop                  # 恢复工作

# 后悔了怎么办？
git reset --soft HEAD~1        # 撤回本次提交，保留改动
git restore 文件               # 撤销文件的修改
git reflog                     # 找到丢失的提交
git reset --hard HEAD@{2}      # 回到 reflog 中的某个状态
```

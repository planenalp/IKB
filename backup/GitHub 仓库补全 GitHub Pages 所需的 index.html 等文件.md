<!-- ##{"timestamp":1743734231}## -->

# BB
有的仓库没有准备好对应的 `gh-pages` 分支方便直接创建 GitHub Pages 页面来使用，但是主分支如 `main` 内有对应目录 site（以此作为假设情况），里面包含了 GitHub Pages 网页创建的所需内容，此时有 N 种方式可以用

以下由简单到复杂整理了一下

----------------------------------------------------------------
# 1. 重命名目录法
下载源码，单独把解压后 site 目录重命名为 docs，当前仓库首页根目录 `Add file` - `Upload files`，把整个 docs 目录拖进去得到 - `Commit change` 提交

设置 GitHub Pages
Settings - Pages - Branch
`main` `/docs` `Save`

等待完成

----------------------------------------------------------------
# 2. 新建仓库法
下载源码，新建仓库，单独把解压后 site 目录内的所有文件上传到新建仓库的根目录（不包含 site 目录本身）

设置 GitHub Pages
Settings - Pages - Branch
`main` `/(root)` `Save`

等待完成

----------------------------------------------------------------
# 3. 根目录直接放置法
目录会很乱，不太推荐

下载源码，单独把解压后 site 目录内的全部文件上传到主分支的根目录（不包含 site 目录本身）

设置 GitHub Pages
Settings - Pages - Branch
`main` `/(root)` `Save`

等待完成

----------------------------------------------------------------
# 4. 网页手动创建分支法
这个不用额外装工具，但是文件一个个删略微麻烦

## 4.1 下载文件
仓库主页 - Code - 中间栏绿色 `Code` 按钮 - `Download ZIP` - 解压缩获得其中所需的包含 `index.html` 文件的 site 目录

## 4.2 创建 `gh-pages` 分支
仓库主页 - Code - 中间栏 `main` 按钮 - `View All branches` - `New Branch`

弹出新窗口 Create a branch
New branch name: `gh-pages`
Source: `main`（默认）

`Create new branch`

## 4.3 清空 `gh-pages` 分支
仓库主页 - Code - 中间栏 `main` 按钮 - `gh-pages` - 删除所有可见文件

删除目录/文件：
点击目录目录/文件 - 右上角 ... - `Delete directory` - `Commit changes...`
以此类推，直到全部删除为止

## 4.4 上传文件
继续在 `gh-pages` 分支状态下 - 中间栏按钮 `Add file` - `Upload files`
将之前下载并解压缩获得其中所需的包含 `index.html` 文件的 site 目录内所有文件拖进来上传到 `gh-pages` 根目录下

`Commit changes` 提交

## 4.5 设置 GitHub Pages
Settings - Pages - Branch
`gh-pages` `/(root)` `Save`

等待完成

----------------------------------------------------------------
# 5. 创建分支法
## 5.1 安装
Windows 为例，
下载安装 [GitHub Desktop](https://desktop.github.com/download/)
下载安装 [Git](https://git-scm.com/downloads)

## 5.2. 导入仓库
打开登录

软件首页 Let's get started
搜索 `用户名/ABC`
`Clone 用户名/ABC`
选择目录
Clone

## 5.3 创建分支
点击顶栏第二个按钮 Current branch: `main`（或 master 等） - `New branch`

弹出新窗口 Create a branch
Name: `gh-pages`
`Create branch`

顶栏 Current branch 变为 `gh-pages` 状态

创建的新分支 `gh-pages` 会默认自带主分支 `main` 的所有内容，需要先删除后再把 site 目录文件放进来

打开当前仓库的本地目录，根据之前设置的位置找，或点击软件中间的按钮 Show in Explorer 直达

// 虽然 Current branch 改成 `main` 还是 `gh-pages` 状态，这个本地目录都没区别
指向的是哪个分支，修改目录就会同步到那个分支上，只需当成是 Current branch 对应的状态就可以了

把隐藏的 .git 文件和 site 目录保留，其余文件全部删除
然后把 site 目录内全部文件剪切出来和 .git 目录同级，再删掉空白的 site 目录

回到 GitHub Desktop 软件
左下角头像栏旁的小长条输入框 Summary (required): `gh-pages`（随便输点内容）
`Commit to gh-pages`

点击顶栏第三个按钮 `Publish branch` 同步修改到 GitHub 仓库上

## 5.4 检查
来到 GitHub 仓库主页，点击中间栏 `main` 按钮，可以看到 Branches 上新增了名为 `gh-pages` 的分支
点击进去确认之前 site 目录文件在里面，特别是 `index.html`

## 5.5 设置 GitHub Pages
Settings - Pages - Branch
`gh-pages` `/(root)` `Save`

等待完成


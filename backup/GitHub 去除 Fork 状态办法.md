<!-- ##{"timestamp":1743715562}## -->

# BB
看到喜欢仓库，有想法要自定义一些内容时，比如最近在研究的 Gmeek 博客程序有想按自己想法修改点源码时，像我这种新手在正常 Fork 后很容易尴尬地不小心点错 pull request 让原作者以为要提交代码
这教程主要就是为了防止出现这种情况，我已经手贱出现过好几次了..

并且设置了模板 Tempelate 状态之后有什么想修改的可直接通过这个模板复制一份出来搞坏了随时删随时重建，还挺方便

按步骤操作完成后可再正常 Fork 一次，互不冲突

---------------------------------------------------
# 1. 假设 Fork 一个名为 ABC 的仓库 
仓库主页 - 右上角 Fork - `Create a new fork`
Owner: `当前用户名`（默认） / Repository name: `ABC`（默认）
Description: `***`（默认）
Copy the main branch only：`取消打勾`（重要）
`Create fork`

---------------------------------------------------
# 2. 设置为模板
ABC 仓库内 Settings - General - Template repository：`打勾`

---------------------------------------------------
# 3. 通过模板二次创建模板
## 3.1 记录下顶部信息（非必要）
ABC 仓库主页 - Code
内容大致为 `forked from 原作者用户名/ABC`

## 3.2 从模板再次创建
右上角 Use this template - `Create a new repositioy`

Repository template: `当前用户名/ABC`（默认）
Include all branches：`打勾`（重要）
Owner: `当前用户名`（默认） / Repository name: `ABC-Tempelate`（起个自己记得住的名就行）
Description: `forked from 原作者用户名/ABC`（非必要）
`Private`

`Create repository`

## 3.3 二次创建仓库设置为模板
自动来到 ABC-Tempelate 主页，顶部显示状态 `generated from 当前用户名/ABC`

Settings - General - Template repository：`打勾`

---------------------------------------------------
# 4. 删除最初 Fork 的仓库 ABC
点击 ABC-Tempelate 主页顶部显示的 `generated from 当前用户名/ABC`
来到 Fork 的仓库 `当前用户名/ABC`
Settings - General - 拉到最底 Danger Zone - `Delete this repository`
`I want to delete this repository`
`I have read and understand these effects`
复制或粘贴文字 `当前用户名/ABC`
`Delete this repository`

---------------------------------------------------
# 5. 确认状态
刷新仓库 ABC-Tempelate 主页，顶部状态 `generated from 当前用户名/ABC` 消失
完成

---------------------------------------------------
# 6. 使用，通过二次模板三次创建模板
仓库 ABC-Tempelate 主页
右上角 Use this template - `Create a new repositioy`
剩余步骤参考 [# 3.3 二次创建仓库设置为模板](#3.3-二次创建仓库设置为模板)

可以开始愉快地玩耍了🥳

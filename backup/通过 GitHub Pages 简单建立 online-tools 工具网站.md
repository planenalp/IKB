<!-- ##{"timestamp":1743617776}## -->

# BB
这个项目似乎没适配自定义域名的情况，绑定了 `online-tools.abc.com` 后打开的状态还是维持 `用户名.github.io/online-tools` 的格式全部变成依然在读取 `online-tools.abc.com/online-tools` 目录内的文件，导致各种 CSS 和 js 都加载失败

所以目前只能通过下面这两种不绑定域名的方式来使用：

- Repository name: `online-tools`，域名就是 `用户名.github.io/online-tools`（注意大小写）
- Repository name: `用户名.github.io`，域名就是 `用户名.github.io`

除非设置域名转发，比如在 Cloudflare 内设置 `online-tools.abc.com` 重定向到 `用户名.github.io/online-tools`

成品在此（没法使用自定义域名，所以链接作废）：[online-tools](https://online-tools.klein.blue/)

刚用 GitHub Pages 简单搭建了个 CyberChef 工具网站，顺手把 [emn178/online-tools](https://github.com/emn178/online-tools) 也搞了

基本步骤和前一篇 [通过 GitHub Pages 简单建立 CyberChef 工具网站](https://international.klein.blue/post/19.html) 大同小异

# Fork 克隆项目
来到 [emn178/online-tools](https://github.com/emn178/online-tools) ，右上角 Fork - Create a new fork

Owner: `用户名` / Repository name: `online-tools`（默认，也可以填 `用户名.github.io`）

Description: `默认`

取消打勾 `Copy the master branch only` （非必要）

Create fork

# 初始化设置
Fork 完成后自动跳到新页面 `用户名/online-tools`
Settings - Pages

## 修改 Branch
Build and deployment
Branch
`None` `Save` 这里点击 `None`
Select branch: `master`
变成状态为
`master` `/(root)` `Save`
点击 `Save` 保存状态

## 自定义域名（有的话）
Custom domain
填入域名如 `online-tools.abc.com`

官方教程：[Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

<details><summary>Cloudflare 设置</summary> 

以 Cloudflare 为例，其它大同小异

情况1和情况2的 DNS 记录本身并不冲突，可同时存在，实现 `abc.com` 和 `online-tools.abc.com` 分别指向两个不同仓库内的 GihHub Pages 项目

### 情况1. 二级域名直接做 GitHub Pages 的 online-tools 项目地址
如 `abc.com` 这种

域名 - DNS - Add record 添加记录
| Type: A | Name: @ | IPv4 address: 185.199.108.153 | Proxy status: Proxied | TTL: Auto |
| :-------: | :---------: | :--------------------------------: | :----------------------: | :---------: |
| Type: A | Name: @ | IPv4 address: 185.199.109.153 | Proxy status: Proxied | TTL: Auto |
| Type: A | Name: @ | IPv4 address: 185.199.110.153 | Proxy status: Proxied | TTL: Auto |
| Type: A | Name: @ | IPv4 address: 185.199.111.153 | Proxy status: Proxied | TTL: Auto |

GitHub 仓库
Settings - Pages - Custom domain: `abc.com`（不需要带 http:// 或 https:// 前缀）
Save

### 情况2. 三级域名做 GitHub Pages 的 online-tools 项目地址
如 `www.abc.com` 或 `online-tools.abc.com` 或 `ot.abc.com`

域名 - DNS - Add record 添加记录
`| Type: CNAME | Name: online-tools | Target: 用户名.github.io | Proxy status: Proxied | TTL: Auto |`
或先添加情况1的 DNS记录，然后直接
`| Type: CNAME | Name: online-tools | Target: @ | Proxy status: Proxied | TTL: Auto |`

GitHub 仓库
Settings - Pages - Custom domain: `online-tools.abc.com`（不需要带 http:// 或 https:// 前缀）
Save

</details>

验证
等待黄色的 DNS Check in Progress 变成绿色 DNS check successful 即可（不等变黄也没所谓，以浏览器能打开为准）

## 检查
回到仓库首页 `用户名/online-tools`，等待一段时间刷新直到中间主栏目顶部的 `pending 🟡` 变成 `success ✅`
完成后打开域名访问一次看是否正常

- Repository name: `online-tools`，域名就是 `用户名.github.io/online-tools`（注意大小写）
- Repository name: `用户名.github.io`，域名就是 `用户名.github.io`
- Custom domain: `online-tools.abc.com`，域名就是自定义的地址 `online-tools.abc.com`

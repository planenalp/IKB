<!-- ##{"timestamp":1743613938}## -->

# BB
成品在此：https://cyberchef.klein.blue/

想用 GitHub Pages 简单搭建一个 CyberChef 工具网站，找了半天没有合适的教程，不是让下载到本地就是下载到本地再上传的误导，研究了下发现其实直接 Fork 就已经可以，还能随时更新版本，干脆自己写一个算了

可惜这个项目好像没做移动端适配，只能说横屏方式能凑合用用
有做适配的 [CorentinTh/it-tools](https://github.com/CorentinTh/it-tools) 暂时又没找到 GitHub Pages 的用法，之后再看吧

[emn178/online-tools](https://github.com/emn178/online-tools) 这个倒是好像不错，也能像 CyberChef 这样直接用，就是简陋了点

# Fork 克隆项目
来到 [CyberChef 项目仓库](https://github.com/gchq/CyberChef)，右上角 Fork - Create a new fork

Owner: `用户名` / Repository name: `CyberChef`（默认，也可以填 `用户名.github.io`）

Description: `默认`

> [!IMPORTANT]
> 取消打勾 `Copy the master branch only`

Create fork

# 初始化设置
Fork 完成后自动跳到新页面 `用户名/CyberChef`
Settings - Pages

## 修改 Branch
Build and deployment
Branch
`None` `Save` 这里点击 `None`
Select branch: `gh-pages`
变成状态为
`gh-pages` `/(root)` `Save`
点击 `Save` 保存状态

## 自定义域名（有的话）
Custom domain
填入域名如 `cyberchef.abc.com`

官方教程：[Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)

<details><summary>Cloudflare 设置</summary> 

以 Cloudflare 为例，其它大同小异

情况1和情况2的 DNS 记录本身并不冲突，可同时存在，实现 `abc.com` 和 `cyberchef.abc.com` 分别指向两个不同仓库内的 GihHub Pages 项目

### 情况1. 二级域名直接做 GitHub Pages 的 CyberChef 项目地址
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

### 情况2. 三级域名做 GitHub Pages 的 CyberChef 项目地址
如 `www.abc.com` 或 `cyberchef.abc.com` 或 `cc.abc.com`

域名 - DNS - Add record 添加记录
`| Type: CNAME | Name: cyberchef | Target: 用户名.github.io | Proxy status: Proxied | TTL: Auto |`
或先添加情况1的 DNS记录，然后直接
`| Type: CNAME | Name: cyberchef | Target: @ | Proxy status: Proxied | TTL: Auto |`

GitHub 仓库
Settings - Pages - Custom domain: `cyberchef.abc.com`（不需要带 http:// 或 https:// 前缀）
Save

</details>

验证
等待黄色的 DNS Check in Progress 变成绿色 DNS check successful 即可（不等变黄也没所谓，以浏览器能打开为准）

## 检查
完成后打开域名访问一次看是否正常

- Repository name: `CyberChef`，域名就是 `用户名.github.io/CyberChef`（注意大小写）
- Repository name: `用户名.github.io`，域名就是 `用户名.github.io`
- Custom domain: `cyberchef.abc.com`，域名就是自定义的地址 `cyberchef.abc.com`

<!-- ##{"timestamp":1743617776}## -->

# BB
成品在此：[online-tools](https://online-tools.klein.blue/)

这个使用自定义域名的话情况略微复杂，请仔细参考文章后半段

刚用 GitHub Pages 简单搭建了个 CyberChef 工具网站，顺手把 [emn178/online-tools](https://github.com/emn178/online-tools) 也搞了


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
等待黄色的 <font color=yellow>DNS Check in Progress</font>
变成绿色 <font color=green>DNS check successful</font> 即可（不等变黄也没所谓，以浏览器能打开为准）


# 修复自定义域名 bug
这个项目没适配自定义域名的情况，绑定了 `online-tools.abc.com` 后打开的状态还是维持 `用户名.github.io/online-tools` 的格式全部变成依然在读取 `online-tools.abc.com/online-tools` 目录内的文件，导致各种 CSS 和 js 都加载失败

解决方案
问题的核心是：资源在仓库根目录，但 `<base>` 标签或 Jekyll 配置导致路径多了一个 `/online-tools/`

以下是修复步骤：

## 步骤 1：修正 `<base>` 标签
在仓库首页 `用户名/online-tools` 根目录下拉找到并打开 `index.html`

右上角 `Edit this file` 进行编辑状态

随便点击一下中间代码框然后 `Ctrl + F` 搜索将 `<base href="/online-tools/">` 替换为 `<base href="/">`（只有一条）
这确保资源从自定义域名的根路径 `https://online-tools.abc.com/` 加载，例如 `https://online-tools.abc.com/css/style.css`

`Commit changes...` 保存并提交到  GitHub

## 步骤 2：添加 `_config.yml` 以适配 Jekyll
仓库被 GitHub Pages 识别为 Jekyll 站点（因为它是 HTML 项目），但缺少 `_config.yml` 会导致默认配置干扰路径

在仓库根目录 `Add file` - `Create new file` 创建文件
在 `Name your file` 输入框中，输入 `_config.yml`

在编辑器中粘贴以下代码：

```
baseurl: ""
url: "https://online-tools.abc.com"
```

`Commit changes...` 保存并提交到 GitHub

- baseurl: ""表示站点部署在根路径，而不是 `/online-tools/`
- url 指定自定义域名

## 步骤 3：设置 GitHub Actions 自动化更新流程防止更新后 index.html 的改动丢失
### 步骤 3.1 创建工作流文件
- 创建一个简单的 GitHub Action，在每次拉取上游更新后自动调整 `<base>` 标签
- 示例工作流 `.github/workflows/fix-base.yml`

在仓库根目录 `Add file` - `Create new file` 创建文件
在 `Name your file` 输入框中，输入路径：`.github/workflows/fix-base.yml`

- 这会创建一个 `.github/workflows` 目录，并添加一个名为 `fix-base.yml` 的文件
- 注意：文件名可以随便取，但后缀必须是 `.yml` 或 `.yaml`

在编辑器中粘贴以下代码：

```
name: Fix Base URL
on:
  push:
    branches: [ main ]
jobs:
  fix-base:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Replace base href
        run: sed -i 's|<base href="/online-tools/">|<base href="/">|' index.html
      - name: Commit changes
        run: |
          git config --local user.email "github-actions@github.com"
          git config --local user.name "GitHub Actions"
          git add index.html
          git commit -m "Fix base href for custom domain" || echo "No changes to commit"
          git push
```

代码解释：
- name: 工作流名称，随便取，这里是“Fix Base URL”
- on: 触发条件，push 表示推送代码时触发，branches: [ main ] 限定在 main 分支
- jobs: 任务列表，这里只有一个任务 fix-base
- runs-on: 运行环境，用 Ubuntu 虚拟机
- steps: 执行步骤：
  1. actions/checkout@v3: 拉取你的仓库代码
  2. Replace base href: 用sed命令替换 `<base>` 标签
  3. Commit changes: 将修改提交回仓库

`Commit changes...` 保存并提交到 GitHub

效果：
每次推送（包括从上游同步）后，Action 会自动修正 `<base>` 标签，无需手动干预

### 步骤 3.2 测试工作流
1. 触发 Actions：
- 随便改动一个文件（比如在 index.html 加个空格），然后提交到 main 分支
- 或者直接手动运行（见步骤 3）

2. 查看运行状态：
- 转到仓库的 “Actions” 选项卡（页面顶部，旁边有 “Code”、“Issues” 等）
- 你会看到一个名为 “Fix Base URL” 的工作流正在运行
- 点击它，展开详情，看日志：
  - 如果成功，会有类似 “Replace base href” 和 “Commit changes” 的输出
  - 如果失败，会显示错误信息

3. 检查 `index.html`：
- 运行完成后，刷新仓库页面，打开 index.html，确认 `<base href="/online-tools/">` 已改为 `<base href="/">`

### 步骤 3.3 手动运行（可选）
如果想立刻测试，不用改代码：
1. 去 “Actions” 选项卡
2. 在左侧选择 “Fix Base URL” 工作流
3. 点击右侧的 “Run workflow” 按钮（可能需要先启用 Actions）
4. 选择 main 分支，点击绿色 “Run workflow” 按钮
5. 等待几秒，刷新页面查看结果

# 检查
回到仓库首页 `用户名/online-tools`，等待一段时间刷新直到中间主栏目顶部的 `pending 🟡` 变成 `success ✅`
完成后打开域名访问一次看是否正常

- Repository name: `online-tools`，域名就是 `用户名.github.io/online-tools`（注意大小写）
- Repository name: `用户名.github.io`，域名就是 `用户名.github.io`
- Custom domain: `online-tools.abc.com`，域名就是自定义的地址 `online-tools.abc.com`

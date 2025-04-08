<!-- ##{"timestamp":1744073386}## -->

# BB
GitHub Pages 的 index.html ***.css ***.js 等文件引用逻辑，不同情况需使用不同逻辑
做个记录方便翻看

------------------------------------------------------
# 三种部署形式
https://用户名.github.io/
https://用户名.github.io/仓库名/
https://仓库名.com/

------------------------------------------------------
# 0. 目录本身命名规则
- GitHub Pages 默认会使用 Jekyll 来构建站点，而 Jekyll 默认会忽略所有以下划线开头的目录（例如 _folder）
因此，假设放在 _css 目录下的 style.css 文件在发布时不会被处理和部署，从而导致返回 404

解决方法：
1. 修改目录名称（推荐）
如果不想禁用 Jekyll，可以将 _folder 改名为不以 _ 开头的目录名，例如 folder，这样 Jekyll 就不会忽略这个目录中的内容。

2. 添加 .nojekyll 文件
在项目根目录添加一个空的 .nojekyll 文件，这样可以禁用 Jekyll 的处理，所有文件和目录都会原封不动地部署。只需在项目根目录创建一个名为 .nojekyll 的空文件并提交即可。

选择一种方法后，重新部署 GitHub Pages 项目，CSS 文件应该就能正确加载了。

------------------------------------------------------
# 1. index.html 文件内引用
- 目录名前不加任何东西（推荐），兼容 [# 三种部署形式](#三种部署形式)
如 `<script src="js/script.js"></script>`

- 在前面添加 `./` ，兼容 [# 三种部署形式](#三种部署形式)
如 `<script src="./js/script.js"></script>`

- 仅用 `/`，只能适配如 https://用户名.github.io/仓库名/ 的方式
如 `<script src="/js/script.js"></script>`

------------------------------------------------------
# 2. CSS 文件内引用
兼容 [# 三种部署形式](#三种部署形式)

假设 GitHub Pages 的 main 分支内有以下目录文件
css/style.css
images/avatar.svg

style.css 文件内引用 avatar.svg，引用文件路径需使用 `../`

如
```
.bg-\[url\(\'\/grid\.svg\'\)\] {
    background-image: url(../images/avatar.svg);
}
```

------------------------------------------------------
# 3. JS 文件内引用
兼容 [# 三种部署形式](#三种部署形式)

现有文件
仓库/js/tools/tool.js
仓库/js/script.js

仓库/js/tools/tool.js 的内容想要引用仓库/js/script.js
需要`直接使用目录前缀`，使用的路径为 `js/script.js，`

如
```
            const r = document.createElement("script");
            r.src = "js/script.js";
            r.async = true;
```


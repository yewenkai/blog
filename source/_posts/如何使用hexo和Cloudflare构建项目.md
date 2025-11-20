---
title: 如何使用hexo和Cloudflare构建项目
date: 2025-11-20 15:05:59
tags: [Hexo, Cloudflare, 博客, 部署]
---

# 如何使用Hexo和Cloudflare构建项目

## 第一步：本地搭建 Hexo

### 初始化 Hexo

在电脑上找个空文件夹（比如 D:\my-blog），右键打开终端（Terminal/PowerShell）：

```bash
# 安装 Hexo 脚手架
npm install -g hexo-cli

# 初始化博客文件夹
hexo init blog
cd blog
npm install
```

### 验证本地效果

在终端运行：

```bash
hexo clean
hexo g
hexo s
```

打开浏览器访问 http://localhost:4000。如果看到页面，说明本地搭建成功。

## 第二步：配置 Git 并上传到 GitHub

Cloudflare Pages 需要从 GitHub 拉取代码。

### 1. 在 GitHub 创建仓库

1. 登录 GitHub
2. 创建一个新仓库，命名为 my-blog（或者你喜欢的名字）
3. 注意：设为 Public（公开）或 Private（私有）都可以

### 2. 推送代码

在 VS Code 的终端（确保在 blog 根目录下）执行：

```bash
# 初始化 git
git init

# 生成 .gitignore 文件 (防止把生成的静态文件和依赖包传上去，这一步很重要)
# 如果你 hexo init 时自动生成了 .gitignore，检查里面是否有 node_modules 和 public
echo "node_modules/" >> .gitignore
echo "public/" >> .gitignore
echo "db.json" >> .gitignore

# 提交代码
git add .
git commit -m "Init"
git branch -M main
# 将下面的地址换成你刚才创建的 GitHub 仓库地址
git remote add origin https://github.com/你的用户名/my-blog.git
git push -u origin main
```

## 第三步：在 Cloudflare Pages 部署

这是最关键的一步，把你的代码变成全球可访问的网站。

1. 登录 Cloudflare Dashboard
2. 左侧菜单点击 Workers & Pages -> Overview -> Create application
3. 点击 Pages 标签 -> Connect to Git
4. 选择你刚才创建的 my-blog 仓库，点击 Begin setup

### 配置构建环境 (Build Settings) - 重点！

这里填错会导致构建失败。请严格按照以下填写：

- **Project name**: 随便填（例如 my-blog）
- **Production branch**: main
- **Framework preset (框架预设)**: 选 无（新版本没有hexo可以选择）
- **Build command**: `npx hexo generate` (默认即可)
- **Build output directory**: `public` (默认即可)

### 关键设置（必须做）

点击 Environment variables (环境变量) -> Add variable：

- **Variable name**: `NODE_VERSION`
- **Value**: `24.0.0` (推荐锁死版本，防止 CF 默认环境过老导致报错)

点击 Save and Deploy。

Cloudflare 会开始拉取代码 -> 安装依赖 -> 运行 hexo g。等待约 1-2 分钟，当看到 Success 时，点击给出的 xxx.pages.dev 链接，你的博客就上线了！

## 第四步：绑定自定义域名

1. 在 Cloudflare Pages 的项目页面，点击 Custom domains
2. 点击 Set up a custom domain
3. 输入你的域名（例如 blog.yoursite.com），前提是这个域名已经在 Cloudflare 解析了
4. 点击 Activate。Cloudflare 会自动添加 CNAME 记录并配置 SSL 证书

## 总结

通过以上四个步骤，你就成功使用 Hexo + Cloudflare Pages 搭建了一个免费的静态博客网站。整个过程不仅免费，而且 Cloudflare 提供了全球 CDN 加速，让你的博客访问速度非常快！
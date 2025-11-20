---
title: 使用Cloudflare R2构建图床
date: 2025-11-20 15:13:09
tags: [Cloudflare, R2, 图床, 博客, 图片托管]
---

# 使用Cloudflare R2构建图床

## 为什么这是最优解？

- **免费额度大**：10GB 存储，每月 1000 万次读取，个人博客根本用不完
- **速度极快**：直接走 Cloudflare 的全球网络
- **域名统一**：你可以挂载一个二级域名，如 img.你的域名.com
- **高可用性**：依托 Cloudflare 的基础设施，稳定性有保障

## 操作步骤

### 1. 开通 R2

1. 登录 Cloudflare 面板
2. 进入 R2 -> Create bucket（起个名，如 `imgs-asset`）

### 2. 绑定域名

1. 进入 Bucket -> Settings -> Public Access -> Connect Custom Domain
2. 输入 `img.yourdomain.com`（前提是你的域名已经在 CF 解析）
3. 等待 SSL 证书自动配置完成

### 3. 获取密钥

1. 在 R2 概览页 -> Manage R2 API Tokens -> Create API Token
2. 权限选 **Admin Read & Write**
3. 记录下以下信息：
   - Access Key ID
   - Secret Access Key
   - Endpoint

### 4. 配置上传工具 (PicGo)

你需要一个能"截图后自动上传并生成 Markdown 链接"的工具。

#### 下载安装 PicList

推荐使用 PicList（PicGo 的增强版，对 R2 支持更好）。

#### 配置 S3 插件

在设置中选择 Amazon S3 插件（R2 兼容 S3 协议），填写以下配置：

- **图床配置名**: R2
- **AccessKeyId**: 填刚才申请的 Access Key ID
- **SecretAccessKey**: 填刚才申请的 Secret Access Key
- **Bucket**: imgs-asset
- **设定自定义节点**: 填刚才获取的S3 API的链接
- **设定自定义域名**: `https://img.yourdomain.com/imgs-asset`

![](https://imgs.yewenkai.cn/imgs-asset/2025/11/896fa775a195a6249709b89a8a3fab5d.png)

![](https://imgs.yewenkai.cn/imgs-asset/2025/11/8ef07f7fff52f4404a6ed509628a09e0.png)
## 写作流程

配置完成后，你的图片上传流程变得非常简单：

1. **截图** -> `Ctrl+C`（复制到剪贴板）
2. **在 Markdown 编辑器里** `Ctrl+V`（粘贴）
3. PicGo 自动上传图片到 R2，并自动插入 Markdown 链接：`![](https://img.yourdomain.com/xxx.jpg)`

## 总结

使用 Cloudflare R2 + PicList 搭建图床的优势：

1. **完全免费**：对于个人博客来说，免费额度绰绰有余
2. **全球加速**：Cloudflare CDN 确保图片加载速度快
3. **简单易用**：截图粘贴即可，无需手动上传
4. **品牌统一**：使用自己的域名，更专业
5. **数据安全**：Cloudflare 提供可靠的数据存储

这个方案是目前个人博客图床的最优解！

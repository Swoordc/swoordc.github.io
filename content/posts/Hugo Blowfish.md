```
---
title: "从零搭建 Hugo Blowfish 中文博客的全过程与坑点总结"
date: 2025-11-05
draft: false
tags: ["Hugo", "Blowfish", "博客搭建", "复盘"]
---

## 前言

最近我决定给自己搭建一个博客，用来记录学习笔记、项目总结以及生活思考。作为自由职业者，日常时间大多花在网络、编程和游戏上，我希望这个博客不仅能上线，还能让我在搭建过程中学到东西，而不是单纯完成任务。  

这篇文章记录了整个过程，包括遇到的坑和解决方法，方便未来参考，也给同样想搭建博客的朋友一些思路。

---

## 1. 搭建环境

我使用的是 Windows 电脑，本来想用 WSL（Windows Subsystem for Linux），但是 WSL 和 Hyper-V 有冲突，最后决定直接在 Windows 上操作。

主要工具：

- Git（版本控制）
- Hugo（静态博客生成器）
- Blowfish（Hugo 主题）
- VSCode（编辑器）

**遇到的坑：**

- Git 推送时分支不一致（默认 `master` vs `main`）  
- GitHub 推送需要 OAuth 浏览器认证，密码已不支持  
- 初次推送时远程仓库已有 README，需要处理强制覆盖或合并  

**解决方案：**

​```bash
git init
git add .
git commit -m "Initial commit"
git branch -m master main  # 如果本地是 master
git push -u origin main --force
```


## 2. 安装主题 Blowfish

使用 Git 克隆主题：

```
git submodule add https://github.com/digitalcraftsman/hugo-blowfish.git themes/blowfish
```

然后修改 `hugo.toml` 指定主题：

```
theme = "blowfish"
```

**遇到的坑：**

- 克隆主题时提示：

  ```
  warning: LF will be replaced by CRLF
  ```

  这是 Windows 换行符提示，直接忽略即可

- 用记事本打开 hugo.toml 会乱码

- 最好用 VSCode 或 Notepad++ 打开

------

## 3. 博客配置

我最终使用了一个中文、卡片风格的主页配置模板，包括头像、博客标题、一句话介绍、顶部菜单、深浅色切换等：

```
[params]
homepageLayout = "cards"
defaultTheme = "auto"
colorScheme = "blowfish"
showReadingTime = true
showPostSummary = true
avatar = "images/avatar.jpg"
tagline = "记录学习与成长的点滴"
```

- 头像放在 `static/images/avatar.jpg`
- 菜单包含：首页 / 文章 / 标签 / 关于我

------

## 4. 创建内容

### 关于我页面

```
hugo new content/about/_index.md
---
title: "关于我"
---

你好，我是陈健，自由职业者。  
这里记录我的学习过程、项目进展和生活思考。
```

### 测试文章

```
hugo new content/posts/第一篇文章.md
title: "第一篇文章"
draft: false
summary: "这是文章摘要，会在首页卡片显示"
```

------

## 5. 本地预览

```
hugo server --buildDrafts
```

访问：

```
http://localhost:1313/
```

- 首页显示卡片布局
- 顶部菜单完整
- 深浅色自动切换
- 文章摘要 + 阅读时间

------

## 6. 推送到 GitHub

确保 GitHub 仓库准备好后：

```
git add .
git commit -m "Add Hugo Blowfish blog"
git push -u origin main
```

**坑点：**

- 初次推送可能报错 `src refspec main does not match any` → 因为本地分支名不对或者没有提交
- 强制推送可以覆盖远程已有内容，但注意备份

------

## 7. 部署到 Cloudflare Pages

1. 连接 GitHub 仓库
2. Build command: `hugo --minify`
3. Build output directory: `public`
4. 添加 Variables（等价于环境变量）：

```
HUGO_VERSION = 0.133.0
HUGO_ENV = production
HUGO_ENABLEGITINFO = true
```

1. 部署成功后，访问 `https://<your-project>.pages.dev`

**自定义域名绑定：**

- 在 Cloudflare Pages → Custom Domains 添加域名
- 在 DNS 中添加 CNAME 指向 `<your-project>.pages.dev`
- Cloudflare 自动申请 HTTPS

------

## 8. 复盘与心得

1. **摸石头过程很长，但知识点不少**
   - Git 分支管理、推送认证
   - Hugo 配置、主题参数
   - Cloudflare Pages 部署和域名绑定
2. **重复踩坑可以总结成模板**
   - `hugo.toml` 配置模板
   - 博客项目结构和部署流程
   - 以后做类似项目可以直接套用
3. **任务完成 ≠ 收获**
   - 博客上线只是“任务完成”
   - 收获来自复盘、整理方法和内化技能
4. **下一步**
   - 继续完善博客内容
   - 写学习笔记、项目总结
   - 形成自己的复用模板
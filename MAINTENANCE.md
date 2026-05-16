# 网站维护指南

## 环境要求

- **Node.js** 已安装 (v24)
- **GitHub Desktop** 用于同步代码
- **代码编辑器** VS Code / Typora / 任意 Markdown 编辑器

---

## 一、GitHub Desktop 同步流程

### 首次设置
1. 打开 GitHub Desktop
2. File → Add local repository
3. 选择路径 `D:\apps\GitHub\Elohim000.github.io\Elohim000.github.io`
4. 确认仓库已关联远程 `Elohim000/Elohim000.github.io`
5. 点击 Publish branch / Push 推送到 GitHub

### 日常同步
1. 写完文章或修改后，打开 GitHub Desktop
2. 在 Changes 面板查看改动，填写 Summary（如 "新增一篇笔记"）
3. 点击 **Commit to main**
4. 点击 **Push origin** 推送到 GitHub
5. GitHub Actions 自动构建部署，1-2 分钟后网站更新

---

## 二、创建新文章

### 方法：命令行创建（推荐）

在项目根目录 `Elohim000.github.io` 下打开终端（或在此目录右键 → Open in Terminal）：

```bash
# 创建一篇新文章（博客形式）
npx hexo new post "文章标题"

# 示例
npx hexo new post "JavaScript闭包详解"
```

这会在 `source/_posts/` 下生成 `文章标题.md`。

### 编写 Markdown 内容

打开生成的 `.md` 文件，修改顶部的 Front Matter 信息：

```yaml
---
title: JavaScript闭包详解        # 文章标题
date: 2026-05-20                  # 创建日期
categories: 前端                  # 分类（可多个）
tags:                            # 标签（可多个）
  - JavaScript
  - 基础
description: 深入理解闭包原理       # 文章摘要
photos:                          # 封面图（可选，填URL）
  -
---
正文内容从这开始...
```

然后正常写 Markdown 即可。

---

## 三、删除文章或内容

### 删除文章
直接删除 `source/_posts/` 下的对应 `.md` 文件即可。

```bash
# 或者直接在文件管理器中删除
rm source/_posts/文章标题.md
```

### 删除页面（笔记、关于等）
删除 `source/` 下对应文件夹。

---

## 四、创建笔记

笔记是独立页面，适合整理系列知识点。

```bash
# 创建新笔记页面
npx hexo new page "notes/数据库原理"

# 会在 source/notes/数据库原理/index.md 创建文件
# 编辑该文件写笔记内容
```

---

## 五、本地预览

在推送前预览效果：

```bash
npx hexo server
```

浏览器打开 `http://localhost:4000` 查看。编辑 Markdown 后会自动刷新。

---

## 六、修改网站配置

### 站点配置 (`_config.yml`)
修改网站标题、描述、作者等信息。

### 主题配置 (`_config.butterfly.yml`)
修改外观、特效、小组件等。常用设置：

| 需求 | 配置位置 | 选项 |
|------|----------|------|
| 修改头像 | `avatar.img` | 填入图片 URL |
| 修改导航菜单 | `menu` | 增删菜单项 |
| 开关暗色模式 | `darkmode.enable` | `true`/`false` |
| 开关打字机效果 | `subtitle.enable` | `true`/`false` |
| 开关飘带背景 | `canvas_ribbon.enable` | `true`/`false` |
| 开关时钟小组件 | `clock.enable` | `true`/`false` |
| 修改社交链接 | `social` | 增删图标和链接 |

Butterfly 主题完整配置文档：https://butterfly.js.org/

---

## 七、常用命令速查

```bash
npx hexo new post "标题"     # 创建文章
npx hexo new page "页面名"   # 创建页面
npx hexo server             # 启动本地预览
npx hexo generate           # 生成静态文件
npx hexo clean              # 清理缓存（网站异常时用）
```

---

## 八、故障排查

| 问题 | 解决 |
|------|------|
| 网站不更新 | 检查 GitHub Actions 是否构建成功 |
| 本地预览报错 | `npx hexo clean && npx hexo server` |
| 修改配置不生效 | 重启 `hexo server` |
| 依赖缺失 | `npm install` 重新安装 |

# CLAUDE.md - 个人主页项目指南

> **给新接手的 AI 助手**：这是使用 Jekyll + al-folio 主题 + GitHub Pages 部署的个人主页项目。请在开始任何工作前先阅读下面的"项目约定与踩坑经验"部分。

---

## ⚠️ 项目约定与踩坑经验（清空对话后必读）

### 📁 文件组织约定

```bash
# ✅ 正确做法：在项目目录中创建临时文件
temp/           # 用于临时草稿和工作文件
drafts/         # 用于未发布的内容草稿
assets/img/     # 图片资源
assets/pdf/     # PDF 文件（如简历）

# ❌ 错误做法：不要使用系统临时目录
/tmp/           # 不要在 /tmp/ 中创建项目相关文件
```

**原因**：在 `/tmp/` 中创建的文件难以追踪，不便于后续参考和调试。所有项目相关文件都应该在项目目录中。

### 🚀 部署流程的特殊注意事项

1. **自动部署不工作**
   - ⚠️ 问题：直接 `git push` 不会自动触发部署
   - 原因：workflow scope 权限问题
   - ✅ 解决：必须手动触发 GitHub Actions
   ```bash
   gh workflow run deploy.yml --repo Zhu-Luyu/Zhu-Luyu.github.io
   ```

2. **.nojekyll 文件是必需的**
   - ⚠️ 问题：缺少 `.nojekyll` 会导致 GitHub Pages 无法正确服务网站
   - ✅ 解决：确保项目根目录有 `.nojekyll` 文件
   ```bash
   touch .nojekyll
   ```

3. **GitHub Actions 权限配置**
   - 访问：https://github.com/Zhu-Luyu/Zhu-Luyu.github.io/settings/actions
   - 设置 "Workflow permissions" 为 "Read and write permissions"

### 🔧 文件更新操作约定

1. **推荐使用 GitHub CLI 直接更新文件**
   ```bash
   # 获取文件 SHA
   sha=$(gh api repos/Zhu-Luyu/Zhu-Luyu.github.io/contents/_pages/about.md --jq '.sha')

   # 更新文件
   gh api repos/Zhu-Luyu/Zhu-Luyu.github.io/contents/_pages/about.md \
     --method PUT \
     --input - <<< "{\"message\":\"描述\",\"content\":\"$(base64 -i 文件路径)\",\"sha\":\"$sha\"}"
   ```

2. **或者使用标准 Git 流程**
   ```bash
   git add .
   git commit -m "描述"
   git push
   gh workflow run deploy.yml --repo Zhu-Luyu/Zhu-Luyu.github.io
   ```

### 📝 内容文件约定

1. **项目文件命名**：按重要性命名（如 `1_project.md`, `2_project.md`）
2. **PDF 简历命名**：必须为 `cv_ZhuLuyu.pdf`
3. **日期格式**：使用 `YYYY-MM-DD` 格式
4. **Markdown 语法**：使用 Kramdown (GitHub Flavored Markdown)

### 🐛 常见问题速查

| 问题 | 解决方案 |
|------|----------|
| 网站显示 404 | 检查 GitHub Pages 状态，重新触发部署 |
| 部署失败 | 检查 `.nojekyll` 文件存在，检查 Actions 权限 |
| 修改后未更新 | 手动触发 workflow，清除浏览器缓存 |
| 本地开发失败 | 检查 Docker 运行状态，或重新构建 |

### 🎯 求职目标

- **目标岗位**：AIGC / 大模型后端 / RAG / 具身智能
- **内容策略**：突出扩散模型、LLM、RAG 系统经验
- **技术栈重点**：PyTorch、FastAPI、Docker、全栈开发

---

## 项目概述

- **技术栈**: Jekyll (静态站点生成器) + al-folio 主题
- **部署方式**: GitHub Pages (通过 GitHub Actions 自动部署)
- **仓库**: https://github.com/Zhu-Luyu/Zhu-Luyu.github.io
- **网站地址**: https://zhu-luyu.github.io
- **本地开发**: Docker (推荐) 或 Bundler

## 关键配置文件

| 文件路径 | 用途 |
|---------|------|
| `_config.yml` | 网站主配置文件（标题、URL、个人信息等） |
| `_data/socials.yml` | 社交媒体链接（GitHub、LinkedIn、邮箱等） |
| `_pages/about.md` | 个人简介页面 |
| `_pages/cv.md` | CV/简历页面配置 |
| `_bibliography/papers.bib` | 论文列表（BibTeX 格式） |
| `_news/` | 新闻/动态公告 |
| `_projects/` | 项目展示 |
| `.github/workflows/deploy.yml` | GitHub Actions 部署工作流 |
| `assets/pdf/cv_ZhuLuyu.pdf` | PDF 简历文件 |

## 快速操作指南

### 本地开发

**方式一：使用 Docker（推荐）**
```bash
# 启动 Docker Desktop
open -a Docker

# 构建并启动网站
docker compose up

# 访问 http://localhost:4000
```

**方式二：使用 Bundler**
```bash
# 安装依赖
bundle install

# 启动本地服务器
bundle exec jekyll serve

# 访问 http://localhost:4000
```

### 标准部署流程

```bash
# 1. 修改内容
# 2. 提交到 GitHub
git add .
git commit -m "描述你的更改"
git push

# 3. 手动触发部署（重要！）
gh workflow run deploy.yml --repo Zhu-Luyu/Zhu-Luyu.github.io

# 4. 查看部署状态
gh run list --repo Zhu-Luyu/Zhu-Luyu.github.io --limit 1

# 5. 等待 5-10 分钟后访问网站
```

## 内容更新模板

### 更新论文

```bibtex
@inproceedings{zhu2025maid,
  abbr          = {ICASSP},
  bibtex_show   = {true},
  selected      = {true},
  title         = {MAID: Model Attribution via Inverse Diffusion},
  author        = {Zhu, Luyu and Ye, Kai and Hao, Jie and Wang, Derui},
  booktitle     = {IEEE ICASSP},
  year          = {2025},
  code          = {https://github.com/Zhu-Luyu/MAID},
  pdf           = {链接到PDF},
}
```

### 添加新闻

文件位置：`_news/文件名.md`
```markdown
---
title: "新闻标题"
date: 2025-03-15
collection: news
---

新闻内容...
```

### 添加项目

文件位置：`_projects/数字_project.md`
```markdown
---
title: "项目名称"
collection: projects
permalink: /project/项目简称
date: 2025-03-15
venue: "所属机构或公司"
role: "角色"
status: "状态"  # In Progress, Published, Completed 等
---

项目描述...
```

### 更新简历 PDF

```bash
# 复制新的 PDF 到 assets/pdf/
cp 新简历.pdf assets/pdf/cv_ZhuLuyu.pdf

# 确认 _pages/cv.md 中的路径正确
cv_pdf: /assets/pdf/cv_ZhuLuyu.pdf
```

## GitHub Actions 工作流详情

### 部署工作流程

1. **触发条件**：
   - Push 到 `main` 或 `master` 分支
   - Pull Request 到 `main` 或 `master` 分支
   - 手动触发 (`workflow_dispatch`)

2. **构建步骤**：
   - 检出代码
   - 设置 Ruby 环境 (3.3.5)
   - 设置 Python 环境 (3.13)
   - 安装系统依赖 (ImageMagick)
   - 安装 Python 依赖 (nbconvert)
   - 构建网站 (`bundle exec jekyll build`)
   - 压缩 CSS (purgecss)
   - 部署到 `gh-pages` 分支

3. **部署位置**：
   - 构建产物在 `_site` 目录
   - 自动部署到 `gh-pages` 分支
   - GitHub Pages 从 `gh-pages` 分支提供服务

### 查看部署日志

```bash
# 查看最新的部署日志
gh run view --repo Zhu-Luyu/Zhu-Luyu.github.io --log

# 查看特定部署的日志
gh run view RUN_ID --repo Zhu-Luyu/Zhu-Luyu.github.io --log
```

## 定期维护检查清单

- [ ] 确认 GitHub Actions 正常运行
- [ ] 检查网站访问正常（https://zhu-luyu.github.io）
- [ ] 更新最新论文和项目
- [ ] 检查所有链接是否有效
- [ ] 更新简历 PDF
- [ ] 检查社交媒体链接

## 快速命令参考

```bash
# 本地开发
docker compose up                    # 启动本地开发环境
bundle exec jekyll serve            # 使用 Bundler 启动

# 部署相关
gh workflow run deploy.yml --repo Zhu-Luyu/Zhu-Luyu.github.io  # 触发部署
gh run list --repo Zhu-Luyu/Zhu-Luyu.github.io               # 查看运行状态
gh run view --repo Zhu-Luyu/Zhu-Luyu.github.io --log          # 查看日志

# 文件操作
git add . && git commit -m "描述" && git push                 # 提交更改

# 检查状态
gh api repos/Zhu-Luyu/Zhu-Luyu.github.io/pages               # 检查 Pages 状态
curl -s -o /dev/null -w "HTTP: %{http_code}\n" https://zhu-luyu.github.io  # 检查网站
```

## 相关资源

- **al-folio 官方文档**: https://github.com/alshedivat/al-folio
- **Jekyll 文档**: https://jekyllrb.com/docs/
- **GitHub Pages 文档**: https://docs.github.com/en/pages
- **BibTeX 指南**: https://www.overleaf.com/learn/8/How_to_use_BibTeX

---

**最后更新**: 2026-03-15
**维护者**: Luyu Zhu

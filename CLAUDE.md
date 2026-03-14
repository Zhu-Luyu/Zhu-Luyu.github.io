# CLAUDE.md - 个人主页项目指南

这是使用 Jekyll + al-folio 主题 + GitHub Pages 部署的个人主页项目。

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

### 更新内容并部署

**重要说明**：
- ⚠️ 直接 `git push` 不会自动部署，因为 workflow scope 权限问题
- ✅ 必须通过 GitHub CLI 手动触发部署

**部署流程**：
```bash
# 1. 修改内容后提交到 GitHub
git add .
git commit -m "描述你的更改"
git push

# 2. 手动触发 GitHub Actions 部署
gh workflow run deploy.yml --repo Zhu-Luyu/Zhu-Luyu.github.io

# 3. 查看部署状态
gh run list --repo Zhu-Luyu/Zhu-Luyu.github.io --limit 1

# 4. 等待 5-10 分钟后访问网站查看
```

### 通过 GitHub CLI 直接更新文件

```bash
# 更新单个文件（如 about.md）
gh api repos/Zhu-Luyu/Zhu-Luyu.github.io/contents/_pages/about.md \
  --method PUT \
  --input - <<< '{
    "message": "更新描述",
    "content": "'$(base64 -i 文件路径)'",
    "sha": "'$(gh api repos/Zhu-Luyu/Zhu-Luyu.github.io/contents/_pages/about.md --jq '.sha')'"
  }'
```

## 常见内容更新

### 1. 更新个人信息

编辑 `_config.yml`：
```yaml
title: Luyu Zhu
first_name: Luyu
last_name: Zhu
email: lzhu911@connect.hkust-gz.edu.cn
```

编辑 `_data/socials.yml`：
```yaml
github_username: Zhu-Luyu
linkedin_username: luyu-zhu-8076452b9
email: lzhu911@connect.hkust-gz.edu.cn
```

### 2. 更新论文

编辑 `_bibliography/papers.bib`，添加 BibTeX 条目：
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

### 3. 添加新闻

在 `_news/` 目录创建新的 `.md` 文件：
```markdown
---
title: "新闻标题"
date: 2025-03-15
collection: news
---

新闻内容...
```

### 4. 添加项目

在 `_projects/` 目录创建新的 `.md` 文件：
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

### 5. 更新简历 PDF

```bash
# 复制新的 PDF 到 assets/pdf/
cp 新简历.pdf /Users/torian/Base/Projects/MyPage/assets/pdf/cv_ZhuLuyu.pdf

# 确认 _pages/cv.md 中的路径正确
cv_pdf: /assets/pdf/cv_ZhuLuyu.pdf
```

## 常见问题与解决方案

### Q: 网站显示 404 错误

**原因**：GitHub Pages 配置问题或部署失败

**解决方案**：
```bash
# 检查 GitHub Pages 状态
gh api repos/Zhu-Luyu/Zhu-Luyu.github.io/pages

# 检查最新的构建状态
gh run list --repo Zhu-Luyu/Zhu-Luyu.github.io --limit 3

# 重新触发部署
gh workflow run deploy.yml --repo Zhu-Luyu/Zhu-Luyu.github.io
```

### Q: GitHub Actions 部署失败

**常见原因**：
1. 缺少 `.nojekyll` 文件
2. Workflow 权限配置问题
3. 构建超时

**解决方案**：
```bash
# 确保 .nojekyll 文件存在
touch .nojekyll
git add .nojekyll
git commit -m "Add .nojekyll"
git push

# 检查 Actions 权限
# 访问: https://github.com/Zhu-Luyu/Zhu-Luyu.github.io/settings/actions
# 确保 "Workflow permissions" 设置为 "Read and write permissions"
```

### Q: 修改后网站没有更新

**原因**：GitHub Pages 缓存或部署未触发

**解决方案**：
```bash
# 1. 确认 GitHub Actions 已运行
gh run list --repo Zhu-Luyu/Zhu-Luyu.github.io

# 2. 如果没有运行，手动触发
gh workflow run deploy.yml --repo Zhu-Luyu/Zhu-Luyu.github.io

# 3. 清除浏览器缓存后刷新网站
# Chrome: Cmd+Shift+R (硬刷新)
```

### Q: 本地开发环境启动失败

**Docker 相关**：
```bash
# 确保 Docker 正在运行
docker info

# 重新构建
docker compose down
docker compose up --build
```

**Bundler 相关**：
```bash
# 更新 Ruby gems
bundle update

# 清理 Jekyll 缓存
bundle exec jekyll clean
bundle exec jekyll serve
```

## GitHub Actions 工作流

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

## 内容组织建议

### 针对求职优化（AIGC/大模型/RAG/具身智能）

1. **个人简介**：
   - 突出研究方向和项目经验
   - 强调技术栈（PyTorch、扩散模型、LLM 等）
   - 展示跨领域能力（研究 + 工程）

2. **论文发表**：
   - 将 `selected: true` 标记给重要论文
   - 添加 code 和 pdf 链接
   - 使用合适的会议缩写（ICASSP、NeurIPS、ICML 等）

3. **项目展示**：
   - 按重要性排序（最新的放前面）
   - 每个项目包含：技术栈、角色、成果
   - 突出与目标岗位相关的项目

4. **新闻动态**：
   - 保持 3-5 条最新动态
   - 按时间倒序排列
   - 突出重要里程碑（论文接收、新工作等）

## 仓库维护

### 定期检查清单

- [ ] 确认 GitHub Actions 正常运行
- [ ] 检查网站访问正常（https://zhu-luyu.github.io）
- [ ] 更新最新论文和项目
- [ ] 检查所有链接是否有效
- [ ] 更新简历 PDF
- [ ] 检查社交媒体链接

### 备份与恢复

```bash
# 克隆完整仓库
git clone https://github.com/Zhu-Luyu/Zhu-Luyu.github.io.git

# 备份特定配置
cp _config.yml _config.yml.backup
cp _data/socials.yml _data/socials.yml.backup
```

## 相关资源

- **al-folio 官方文档**: https://github.com/alshedivat/al-folio
- **Jekyll 文档**: https://jekyllrb.com/docs/
- **GitHub Pages 文档**: https://docs.github.com/en/pages
- **BibTeX 指南**: https://www.overleaf.com/learn/8/How_to_use_BibTeX

## 注意事项

1. **环境隔离**：使用 Docker 开发，避免污染本地环境
2. **版本控制**：所有内容通过 Git 管理，保留提交历史
3. **自动部署**：Push 后手动触发 GitHub Actions
4. **文件命名**：
   - PDF 简历命名为 `cv_ZhuLuyu.pdf`
   - 项目文件按重要性命名 `1_project.md`, `2_project.md` 等
5. **图片资源**：放在 `assets/img/` 目录
6. **Markdown 语法**：使用 Kramdown (GitHub Flavored Markdown)

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
gh api repos/.../contents/文件路径 --method PUT              # 直接更新文件
```

---

**最后更新**: 2026-03-15
**维护者**: Luyu Zhu

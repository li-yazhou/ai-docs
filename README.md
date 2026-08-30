# AI技术文摘

Anthropic 工程博客与 Claude 产品博客的**中英对照双语文摘站**，由 [docsify](https://docsify.js.org) 驱动：直接渲染仓库中的 Markdown，无构建步骤，推送到 main 后 GitHub Pages 自动发布。

**在线阅读：<https://li-yazhou.github.io/ai-docs/>**
**GitHub 仓库：<https://github.com/li-yazhou/ai-docs>**

> 内容源自 ai-notes 笔记库（`li-yazhou/vaults/ai-notes`）的 Anthropic 博客翻译项目，2026-08 拆分为独立仓库。

## 内容概览

| 目录 | 来源 | 篇数 | 主题 |
| --- | --- | --- | --- |
| [01-anthropic/](01-anthropic/) | Anthropic 工程博客（engineering.anthropic.com） | 24 篇 | Agent 工程、上下文工程、评测（evals）、工具与 harness 设计 |
| [02-claude/](02-claude/) | Claude 产品博客（claude.com/blog） | 84 篇 | Claude Code、多智能体、Skills、hooks、工作流模式、客户案例 |
| [03-openai/](03-openai/) | OpenAI 博客 | 待补充 | — |

每个目录配有一份**登记总表**（清单 md），按发布时间倒序登记原文链接、星级与翻译进度：

- [01-anthropic/anthropic-engineering.md](01-anthropic/anthropic-engineering.md) — 工程博客全量清单（来源 25 篇，含未译篇目标注）
- [02-claude/claude-blog.md](02-claude/claude-blog.md) — 产品博客清单（sitemap 全量 229 篇中筛选出 84 篇有阅读价值的文章，按主题分类）

## 文章体例

一篇文章一个文件夹。`01-anthropic` 与 `02-claude` 命名为 `YYMM-slug`；`04-anthropic-research` 命名为 `YYYYMMDD-slug`（YYYYMMDD 为文章发布日期）：

```
02-claude/2608-warp-self-improving-agents/
├── 2608-warp-self-improving-agents-bilingual.md   # 中英对照正文
└── images/                                        # 原文图片（相对路径引用）
```

- md 头部为元信息块：原文标题 / 链接 / 作者 / 发布日期 / 翻译模型 / 评分（星级 + 一句看点）
- 正文每段**英文原文在前，中文翻译紧随其后**，默认收录正文主体（不译附录）
- 文件名、文件夹名与 slug 一一对应，便于清单互链与全站搜索

## 站点文件

| 文件 | 说明 |
| --- | --- |
| `index.html` | docsify 入口：vue 主题、全文搜索、图片缩放、auto2top；`homepage: '_home.md'`，侧边栏与文章内相对路径均已开启 |
| `_home.md` | 首页导航：阅读说明、站点说明、文章导航（Anthropic 24 篇 / Claude 84 篇，含星级与看点） |
| `_sidebar.md` | 侧边栏目录：按发布时间排序，顶部链接到两份登记总表 |
| `.nojekyll` | 让 GitHub Pages 放行 `_` 开头的 docsify 约定文件 |
| `.gitignore` | 忽略 `.idea/`、`.DS_Store` |

## 本地预览

```bash
python3 -m http.server 3000
```

访问 <http://localhost:3000> 即可（docsify 需经 HTTP 访问，直接双击 index.html 打不开）。

## 新增文章流程

1. 新建文章文件夹（`01/02` 目录为 `YYMM-slug/`，`04` 目录为 `YYYYMMDD-slug/`），放入同名 `-bilingual.md` 与 `images/`
2. 在对应目录的登记总表中「中英文版本」列登记该篇
3. 更新 `_sidebar.md`（目录）与 `_home.md`（导航）
4. 提交并推送 main，GitHub Pages 自动发布（Pages 页面约有 10 分钟缓存）

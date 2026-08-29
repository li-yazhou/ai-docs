# AI技术文摘

Anthropic 工程博客与 Claude 产品博客的中英对照双语文摘，由 [docsify](https://docsify.js.org) 驱动：直接渲染仓库中的 Markdown，无构建步骤，推送后自动更新。

**在线阅读：<https://li-yazhou.github.io/ai-docs/>**

## 内容

- [01-anthropic/](01-anthropic/) — Anthropic 工程博客（engineering.anthropic.com），24 篇：Agent 工程、上下文工程、评测（evals）、工具与 harness 设计
- [02-claude/](02-claude/) — Claude 产品博客（claude.com/blog），84 篇：Claude Code、多智能体、Skills、hooks、工作流模式、客户案例
- [03-openai/](03-openai/) — OpenAI 博客（待补充）

## 站点文件

| 文件 | 说明 |
| --- | --- |
| `index.html` | docsify 入口页 |
| `_sidebar.md` | 侧边栏目录（按发布时间排序） |
| `_home.md` | 首页导航（按主题分六节，含星级与看点） |

## 本地预览

```bash
python3 -m http.server 3000
```

访问 <http://localhost:3000> 即可。

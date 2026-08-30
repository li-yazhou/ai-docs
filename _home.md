# AI技术文摘

本站收录 Anthropic 官方博客的中英对照翻译，共 132 篇：

- **Anthropic 工程博客**（engineering.anthropic.com，24 篇）：Agent 工程、上下文工程、评测（evals）、工具与 harness 设计等工程方法类文章
- **Claude 产品博客**（claude.com/blog，84 篇）：Claude Code 使用方法、多智能体、Skills、hooks、工作流模式等产品实践类文章
- **Anthropic 研究博客**（anthropic.com/research，24 篇）：Alignment、多智能体风险、可解释性、AI for Science、经济研究等研究侧文章

## 阅读说明

- 排版为**英文原文在前、中文翻译紧随其后**，逐段对照；
- 术语保留英文原文并附中文释义（如：工作流（workflows））；
- 每篇开头的引用块保留原文元信息：作者、原文链接、发布日期；
- 每组第一篇「文章登记总表」是 Obsidian 登记表，含重要程度星级与未翻译篇目，站内链接（`[[...]]`）不可点击，仅作参考。

## 站点说明

- 由 [docsify](https://docsify.js.org) 驱动：直接渲染仓库中的 Markdown，无构建步骤，仓库推送后自动更新；
- 左侧为文档目录（当前文章自动展开二级标题），左侧顶部搜索框支持全文检索；
- 文中图片可点击放大。

## 文章导航

### Anthropic 工程博客（24 篇）

[文章登记总表（星级与未译篇目）](/01-anthropic/anthropic-engineering.md)

| 发布时间 | 英文原文标题 | 中英文版本 |
| --- | --- | --- |
| 2024-09-19 | [Introducing Contextual Retrieval](https://www.anthropic.com/engineering/contextual-retrieval) | [上下文检索（Contextual Retrieval）介绍](/01-anthropic/2409-contextual-retrieval/2409-contextual-retrieval-bilingual.md) |
| 2024-12-19 | [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) | [构建有效的 Agent](/01-anthropic/2412-building-effective-agents/2412-building-effective-agents-bilingual.md) |
| 2025-01-06 | [Raising the bar on SWE-bench Verified with Claude 3.5 Sonnet](https://www.anthropic.com/engineering/swe-bench-sonnet) | [用 Claude 3.5 Sonnet 刷新 SWE-bench Verified 基准](/01-anthropic/2501-swe-bench-sonnet/2501-swe-bench-sonnet-bilingual.md) |
| 2025-03-20 | [The 'think' tool: Enabling Claude to stop and think in complex tool use situations](https://www.anthropic.com/engineering/claude-think-tool) | ["思考"（Think）工具：让 Claude 在复杂的工具使用场景中停下来思考](/01-anthropic/2503-claude-think-tool/2503-claude-think-tool-bilingual.md) |
| 2025-06-13 | [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) | [我们如何构建多 Agent 研究系统](/01-anthropic/2506-multi-agent-research-system/2506-multi-agent-research-system-bilingual.md) |
| 2025-06-26 | [Desktop Extensions: One-click MCP server installation for Claude Desktop](https://www.anthropic.com/engineering/desktop-extensions) | [桌面扩展：为 Claude Desktop 一键安装 MCP 服务器（Desktop Extensions: One-click MCP server installation for Claude Desktop）](/01-anthropic/2506-desktop-extensions/2506-desktop-extensions-bilingual.md) |
| 2025-09-11 | [Writing effective tools for agents — with agents](https://www.anthropic.com/engineering/writing-tools-for-agents) | [为 Agent 编写高效工具——与 Agent 协作（Writing effective tools for agents — with agents）](/01-anthropic/2509-writing-tools-for-agents/2509-writing-tools-for-agents-bilingual.md) |
| 2025-09-17 | [A postmortem of three recent issues](https://www.anthropic.com/engineering/a-postmortem-of-three-recent-issues) | [三起近期问题的复盘（A postmortem of three recent issues）](/01-anthropic/2509-postmortem-three-issues/2509-postmortem-three-issues-bilingual.md) |
| 2025-09-29 | [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) | [面向 AI Agent 的有效上下文工程](/01-anthropic/2509-effective-context-engineering/2509-effective-context-engineering-bilingual.md) |
| 2025-10-16 | [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) | [用 Agent Skills 为真实世界装备 Agent](/01-anthropic/2510-agent-skills/2510-agent-skills-bilingual.md) |
| 2025-10-20 | [Beyond permission prompts: making Claude Code more secure and autonomous](https://www.anthropic.com/engineering/claude-code-sandboxing) | [超越权限提示：让 Claude Code 更安全、更自主](/01-anthropic/2510-claude-code-sandboxing/2510-claude-code-sandboxing-bilingual.md) |
| 2025-11-04 | [Code execution with MCP: Building more efficient agents](https://www.anthropic.com/engineering/code-execution-with-mcp) | [MCP 代码执行：构建更高效的 Agent](/01-anthropic/2511-code-execution-with-mcp/2511-code-execution-with-mcp-bilingual.md) |
| 2025-11-24 | [Introducing advanced tool use on the Claude Developer Platform](https://www.anthropic.com/engineering/advanced-tool-use) | [在 Claude 开发者平台上推出高级工具使用](/01-anthropic/2511-advanced-tool-use/2511-advanced-tool-use-bilingual.md) |
| 2025-11-26 | [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) | [长时运行 Agent 的高效 harness（运行框架）](/01-anthropic/2511-effective-harnesses-for-long-running-agents/2511-effective-harnesses-for-long-running-agents-bilingual.md) |
| 2026-01-09 | [Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) | [揭开 AI Agent 评测（Evals）的神秘面纱](/01-anthropic/2601-demystifying-evals-for-ai-agents/2601-demystifying-evals-for-ai-agents-bilingual.md) |
| 2026-01-21 | [Designing AI-resistant technical evaluations](https://www.anthropic.com/engineering/AI-resistant-technical-evaluations) | [设计抗 AI 的技术评测](/01-anthropic/2601-designing-ai-resistant-technical-evaluations/2601-designing-ai-resistant-technical-evaluations-bilingual.md) |
| 2026-02-05 | [Building a C compiler with a team of parallel Claudes](https://www.anthropic.com/engineering/building-c-compiler) | [用一队并行的 Claude 构建 C 编译器](/01-anthropic/2602-building-c-compiler/2602-building-c-compiler-bilingual.md) |
| 2026-02-05 | [Quantifying infrastructure noise in agentic coding evals](https://www.anthropic.com/engineering/infrastructure-noise) | [量化 Agent 编码评测中的基础设施噪声](/01-anthropic/2602-quantifying-infrastructure-noise/2602-quantifying-infrastructure-noise-bilingual.md) |
| 2026-03-06 | [Eval awareness in Claude Opus 4.6's BrowseComp performance](https://www.anthropic.com/engineering/eval-awareness-browsecomp) | [Claude Opus 4.6 在 BrowseComp 表现中的评测意识（Eval Awareness）](/01-anthropic/2603-eval-awareness-browsecomp/2603-eval-awareness-browsecomp-bilingual.md) |
| 2026-03-24 | [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps) | [面向长时运行应用开发的 Harness 设计](/01-anthropic/2603-harness-design-long-running-apps/2603-harness-design-long-running-apps-bilingual.md) |
| 2026-03-25 | [How we built Claude Code auto mode: a safer way to skip permissions](https://www.anthropic.com/engineering/claude-code-auto-mode) | [我们如何构建 Claude Code 自动模式：一种更安全的跳过权限审批的方式](/01-anthropic/2603-claude-code-auto-mode/2603-claude-code-auto-mode-bilingual.md) |
| 2026-04-08 | [Scaling Managed Agents: Decoupling the brain from the hands](https://www.anthropic.com/engineering/managed-agents) | [扩展托管 Agent：将大脑与双手解耦](/01-anthropic/2604-scaling-managed-agents/2604-scaling-managed-agents-bilingual.md) |
| 2026-04-23 | [An update on recent Claude Code quality reports](https://www.anthropic.com/engineering/april-23-postmortem) | [关于近期 Claude Code 质量问题报告的更新](/01-anthropic/2604-april-23-postmortem/2604-april-23-postmortem-bilingual.md) |
| 2026-05-25 | [How we contain Claude across products](https://www.anthropic.com/engineering/how-we-contain-claude) | [我们如何在各个产品中收容 Claude](/01-anthropic/2605-how-we-contain-claude/2605-how-we-contain-claude-bilingual.md) |

### Claude 产品博客（claude.com/blog，84 篇）

[文章登记总表（星级与未译篇目）](/02-claude/claude-blog.md)

#### 一、Agent 工程方法论（16 篇）

*与 agent 研究方向最契合的一批：harness 设计、多智能体协调、上下文工程、验证回路等工作方式设计*

| 发布时间 | 英文原文标题 | 中英文版本 | 重要程度 | 主要看点 |
| --- | --- | --- | --- | --- |
| 2026-01-22 | [Building agents with Skills: Equipping agents for specialized work](https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work) | [用 Skills 构建智能体：让智能体胜任专业化工作](/02-claude/2601-building-agents-with-skills/2601-building-agents-with-skills-bilingual.md) | ★★★ | 用 Skills 给 agent 装配专业领域能力 |
| 2026-01-23 | [Building multi-agent systems: When and how to use them](https://claude.com/blog/building-multi-agent-systems-when-and-how-to-use-them) | [构建多智能体系统：何时使用与如何使用](/02-claude/2601-building-multi-agent-systems/2601-building-multi-agent-systems-bilingual.md) | ★★★★ | 何时该用多智能体、何时不该，反过度设计 |
| 2026-03-05 | [Common workflow patterns for AI agents—and when to use them](https://claude.com/blog/common-workflow-patterns-for-ai-agents-and-when-to-use-them) | [常见的 AI 智能体工作流模式及其使用时机](/02-claude/2603-common-workflow-patterns/2603-common-workflow-patterns-bilingual.md) | ★★★★ | 常见 agent 工作流模式（单次/链式/并行/编排）与选型建议 |
| 2026-03-05 | [Skills explained: How Skills compares to prompts, Projects, MCP, and subagents](https://claude.com/blog/skills-explained) | [Skills 详解：Skills 与提示词、Projects、MCP 和 subagents 的对比](/02-claude/2603-skills-explained/2603-skills-explained-bilingual.md) | ★★★★ | Skills 与 prompts/Projects/MCP/subagents 的边界辨析与组合用法 |
| 2026-04-02 | [Agent Harness Design: 3 Patterns for Harnessing Claude's Intelligence](https://claude.com/blog/harnessing-claudes-intelligence) | [Agent Harness 设计：驾驭 Claude 智能的三种模式](/02-claude/2604-harnessing-claudes-intelligence/2604-harnessing-claudes-intelligence-bilingual.md) | ★★★★ | Agent Harness 三种设计模式，与 engineering 板块 harness 系列呼应 |
| 2026-04-09 | [The advisor strategy: Give agents an intelligence boost](https://claude.com/blog/the-advisor-strategy) | [顾问策略：给智能体一次智力升级](/02-claude/2604-the-advisor-strategy/2604-the-advisor-strategy-bilingual.md) | ★★★★ | advisor 模式：给 agent 外挂智囊以低成本提升复杂任务表现 |
| 2026-04-10 | [Multi-agent coordination patterns: Five approaches and when to use them](https://claude.com/blog/multi-agent-coordination-patterns) | [多智能体协调模式：五种方法及其适用场景](/02-claude/2604-multi-agent-coordination-patterns/2604-multi-agent-coordination-patterns-bilingual.md) | ★★★★★ | 五种多智能体协调模式及适用场景，多智能体系统设计直接可用的分类框架 |
| 2026-04-10 | [Seeing like an agent: how we design tools in Claude Code](https://claude.com/blog/seeing-like-an-agent) | [像 Agent 一样看世界：我们如何在 Claude Code 中设计工具](/02-claude/2604-seeing-like-an-agent/2604-seeing-like-an-agent-bilingual.md) | ★★★★★ | Claude Code 工具设计哲学：从 agent 的感知方式出发设计工具界面，视角独特 |
| 2026-04-22 | [Building agents that reach production systems with MCP](https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp) | [构建可通过 MCP 触达生产系统的智能体](/02-claude/2604-agents-reach-production-mcp/2604-agents-reach-production-mcp-bilingual.md) | ★★★ | 经 MCP 安全触达生产系统的架构设计 |
| 2026-06-02 | [A harness for every task: dynamic workflows in Claude Code](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code) | [为每个任务打造 harness：Claude Code 中的动态工作流](/02-claude/2606-dynamic-workflows-in-claude-code/2606-dynamic-workflows-in-claude-code-bilingual.md) | ★★★ | Claude Code 动态工作流：为每类任务配置对应 harness |
| 2026-06-24 | [Building effective human-agent teams](https://claude.com/blog/building-effective-human-agent-teams) | [构建高效的人类-智能体团队](/02-claude/2606-building-effective-human-agent-teams/2606-building-effective-human-agent-teams-bilingual.md) | ★★★★ | 人机协作从单人模式到人机团队模式的演进，附 Anthropic 内部实际案例 |
| 2026-06-30 | [Loop engineering: Getting started with loops](https://claude.com/blog/getting-started-with-loops) | [循环工程：循环入门](/02-claude/2606-getting-started-with-loops/2606-getting-started-with-loops-bilingual.md) | ★★★ | Loop engineering 入门：把任务组织成可迭代的循环 |
| 2026-07-22 | [Building verification loops in Claude Code with skills](https://claude.com/blog/building-verification-loops-in-claude-code-with-skills) | [在 Claude Code 中用 Skills 构建验证循环](/02-claude/2607-building-verification-loops/2607-building-verification-loops-bilingual.md) | ★★★★ | 用 Skills 构建 agent 自验证回路，让产出可自检 |
| 2026-07-24 | [The new rules of context engineering for Claude 5 generation models](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models) | [面向 Claude 5 代模型的上下文工程新规则](/02-claude/2607-new-rules-context-engineering/2607-new-rules-context-engineering-bilingual.md) | ★★★★★ | Claude 5 代模型的上下文工程新规则，接续 effective-context-engineering 的最新官方方法论 |
| 2026-08-18 | [How Claude Tag serves as Anthropic's first responder for CI/CD failures](https://claude.com/blog/ai-ci-cd-on-call) | [Claude Tag：Anthropic 的 CI/CD 故障第一响应人](/02-claude/2608-ai-ci-cd-on-call/2608-ai-ci-cd-on-call-bilingual.md) | ★★★ | Claude Tag 担任 CI/CD 值班 agent 的内部实践，附搭建思路 |
| 2026-08-19 | [Turning conversation into knowledge: how Slack builds human-agent teams](https://claude.com/blog/turning-conversation-into-knowledge-how-slack-builds-human-agent-teams) | [把对话变成知识：Slack 如何构建人机协作团队](/02-claude/2608-slack-human-agent-teams/2608-slack-human-agent-teams-bilingual.md) | ★★★ | Slack CPO 访谈，human-agent teams 系列第二篇 |

#### 二、Claude Code 深度实战（22 篇）

*重度使用 Claude Code 的进阶材料：官方团队自述内部用法、大仓库策略、成本工程、组织推广*

| 发布时间 | 英文原文标题 | 中英文版本 | 重要程度 | 主要看点 |
| --- | --- | --- | --- | --- |
| 2025-07-24 | [How Anthropic teams use Claude Code](https://claude.com/blog/how-anthropic-teams-use-claude-code) | [Anthropic 各团队如何使用 Claude Code](/02-claude/2507-how-anthropic-teams-use-claude-code/2507-how-anthropic-teams-use-claude-code-bilingual.md) | ★★★ | Anthropic 各团队使用 Claude Code 的方式集锦 |
| 2025-10-15 | [How to scale agentic coding across your engineering organization](https://claude.com/blog/scaling-agentic-coding) | [如何在工程组织中规模化推广 agentic coding](/02-claude/2510-scaling-agentic-coding/2510-scaling-agentic-coding-bilingual.md) | ★★★ | 在工程组织内规模化推广 agentic coding |
| 2025-11-25 | [Using CLAUDE.md files: Customizing Claude Code for your codebase](https://claude.com/blog/using-claude-md-files) | [使用 CLAUDE.md 文件：为你的代码库定制 Claude Code](/02-claude/2511-using-claude-md-files/2511-using-claude-md-files-bilingual.md) | ★★★ | CLAUDE.md 的写法与组织方式 |
| 2025-12-11 | [Claude Code power user customization: How to configure hooks](https://claude.com/blog/how-to-configure-hooks) | [Claude Code 高级用户自定义：如何配置 hooks](/02-claude/2512-how-to-configure-hooks/2512-how-to-configure-hooks-bilingual.md) | ★★★ | hooks 配置详解（power user 定制） |
| 2026-03-24 | [Auto mode for Claude Code](https://claude.com/blog/auto-mode) | [Claude Code 的 Auto 模式](/02-claude/2603-auto-mode/2603-auto-mode-bilingual.md) | ★★★ | auto mode 发布：更安全地跳过权限确认（工程侧文章见 03-anthropic） |
| 2026-04-07 | [How and when to use subagents in Claude Code](https://claude.com/blog/subagents-in-claude-code) | [如何以及何时在 Claude Code 中使用 subagents](/02-claude/2604-subagents-in-claude-code/2604-subagents-in-claude-code-bilingual.md) | ★★★★ | 子代理何时用、怎么配置 |
| 2026-04-14 | [Redesigning Claude Code on desktop for parallel agents](https://claude.com/blog/claude-code-desktop-redesign) | [为并行 agent 重新设计桌面端 Claude Code](/02-claude/2604-claude-code-desktop-redesign/2604-claude-code-desktop-redesign-bilingual.md) | ★★★ | 桌面版为并行多 agent 重设计的交互 |
| 2026-04-15 | [Using Claude Code: session management and 1M context](https://claude.com/blog/using-claude-code-session-management-and-1m-context) | [Using Claude Code：会话管理与 1M 上下文](/02-claude/2604-session-management-1m-context/2604-session-management-1m-context-bilingual.md) | ★★★ | 会话管理与 1M 上下文的配合使用 |
| 2026-04-28 | [Onboarding Claude Code like a new developer: Lessons from 17 years of development](https://claude.com/blog/onboarding-claude-code-like-a-new-developer-lessons-from-17-years-of-development) | [像对待新开发者一样让 Claude Code 入职：来自 17 年开发的经验](/02-claude/2604-onboarding-claude-code/2604-onboarding-claude-code-bilingual.md) | ★★★ | 把 Claude Code 当新员工 onboarding 的经验之谈 |
| 2026-04-30 | [Lessons from building Claude Code: Prompt caching is everything](https://claude.com/blog/lessons-from-building-claude-code-prompt-caching-is-everything) | [构建 Claude Code 的经验：提示缓存就是一切](/02-claude/2604-lessons-claude-code-prompt-caching/2604-lessons-claude-code-prompt-caching-bilingual.md) | ★★★★ | prompt caching 是一切：agentic coding 的成本工程细节 |
| 2026-05-13 | [Best practices for computer and browser use with Claude](https://claude.com/blog/best-practices-for-computer-and-browser-use-with-claude) | [Claude 计算机与浏览器使用最佳实践](/02-claude/2605-computer-browser-use/2605-computer-browser-use-bilingual.md) | ★★★ | computer use / browser use 的官方使用最佳实践 |
| 2026-05-14 | [How Claude Code works in large codebases: Best practices and where to start](https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start) | [Claude Code 在大型代码库中如何运作：最佳实践与入手指南](/02-claude/2605-claude-code-large-codebases/2605-claude-code-large-codebases-bilingual.md) | ★★★★ | 大型代码仓库中 Claude Code 的检索机制与起步策略 |
| 2026-05-20 | [Using Claude Code: The unreasonable effectiveness of HTML](https://claude.com/blog/using-claude-code-the-unreasonable-effectiveness-of-html) | [Using Claude Code：HTML 出乎意料的有效性](/02-claude/2605-unreasonable-effectiveness-html/2605-unreasonable-effectiveness-html-bilingual.md) | ★★★ | 用 HTML 作为 agent 交互与输出媒介的奇效 |
| 2026-06-03 | [Lessons from building Claude Code: How we use skills](https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills) | [构建 Claude Code 的经验：我们如何使用 skills](/02-claude/2606-lessons-claude-code-skills/2606-lessons-claude-code-skills-bilingual.md) | ★★★★ | 官方团队自述开发 Claude Code 时内部如何使用 skills |
| 2026-06-03 | [Running an AI-native engineering org](https://claude.com/blog/running-an-ai-native-engineering-org) | [运营一个 AI 原生的工程组织](/02-claude/2606-ai-native-engineering-org/2606-ai-native-engineering-org-bilingual.md) | ★★★★ | AI 原生工程组织的运转方式与度量 |
| 2026-06-18 | [Steering Claude Code: when to use CLAUDE.md, skills, hooks, and subagents](https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more) | [驾驭 Claude Code：何时使用 CLAUDE.md、skills、hooks 与 subagents](/02-claude/2606-steering-claude-code/2606-steering-claude-code-bilingual.md) | ★★★★★ | CLAUDE.md/skills/hooks/subagents 四种扩展机制何时用哪个，一篇文章讲清分界 |
| 2026-07-07 | [Claude Code effort level and model selection](https://claude.com/blog/claude-model-and-effort-level-in-claude-code) | [Claude Code 努力档位与模型选型指南](/02-claude/2607-claude-code-effort-model-selection/2607-claude-code-effort-model-selection-bilingual.md) | ★★★★ | 官方 effort 档位与模型选型指南，重度用户日常可直接查用 |
| 2026-07-16 | [How Anthropic runs large-scale code migrations with Claude Code](https://claude.com/blog/ai-code-migration) | [Anthropic 如何用 Claude Code 执行大规模代码迁移](/02-claude/2607-ai-code-migration/2607-ai-code-migration-bilingual.md) | ★★★★ | Anthropic 大规模代码迁移的编排方法与实战经验 |
| 2026-08-07 | [Running auto mode in production](https://claude.com/blog/auto-mode-in-production) | [在生产环境中运行 auto mode](/02-claude/2608-auto-mode-in-production/2608-auto-mode-in-production-bilingual.md) | ★★★ | auto mode 上生产的实践与注意事项 |
| 2026-08-14 | [Maximizing the value of your Claude Code sessions](https://claude.com/blog/maximizing-the-value-of-your-claude-code-sessions) | [最大化 Claude Code 会话的价值](/02-claude/2608-maximizing-claude-code-sessions/2608-maximizing-claude-code-sessions-bilingual.md) | ★★★ | 会话提效实操清单，建议搭配 SDLC 手册阅读 |
| 2026-08-20 | [The Claude Code Guide For Startups](https://claude.com/blog/claude-code-guide-for-startups) | [Claude Code 初创公司指南：五条规则](/02-claude/2608-claude-code-guide-startups/2608-claude-code-guide-startups-bilingual.md) | ★★★ | 五条规则的实战指引，含十余家初创公司一手案例 |
| 2026-08-21 | [The AI-Native SDLC playbook](https://claude.com/blog/the-ai-native-sdlc-playbook) | [AI 原生 SDLC 实战手册](/02-claude/2608-ai-native-sdlc-playbook/2608-ai-native-sdlc-playbook-bilingual.md) | ★★★★ | 官方六阶段 AI 原生 SDLC 全景手册，方法论成体系、可直接落地 |

#### 三、前沿客户案例（17 篇）

*Working at the frontier 系列等一手材料：一线公司如何把 agent 推进生产*

| 发布时间 | 英文原文标题 | 中英文版本 | 重要程度 | 主要看点 |
| --- | --- | --- | --- | --- |
| 2025-10-30 | [How Brex improves code quality and productivity with Claude Code](https://claude.com/blog/how-brex-improves-code-quality-and-productivity-with-claude-code) | [Brex 如何用 Claude Code 提升代码质量与生产力](/02-claude/2510-brex-claude-code/2510-brex-claude-code-bilingual.md) | ★★★ | Brex 用 Claude Code 提升代码质量与效率 |
| 2025-11-17 | [How three YC startups built their companies with Claude Code](https://claude.com/blog/building-companies-with-claude-code) | [三家 YC 创业公司如何用 Claude Code 打造公司](/02-claude/2511-yc-companies-claude-code/2511-yc-companies-claude-code-bilingual.md) | ★★★ | 三家 YC 创业公司用 Claude Code 构建公司 |
| 2026-02-09 | [Behind the model launch: What customers discovered testing Claude Opus 4.6 early](https://claude.com/blog/behind-model-launch-what-customers-discovered-testing-claude-opus-4-6-early) | [模型发布幕后：客户提前测试 Claude Opus 4.6 时发现了什么](/02-claude/2602-behind-model-launch-opus-46/2602-behind-model-launch-opus-46-bilingual.md) | ★★★ | Opus 4.6 发布前客户早期内测的发现 |
| 2026-04-30 | [How Kepler built verifiable AI for financial services with Claude](https://claude.com/blog/how-kepler-built-verifiable-ai-for-financial-services-with-claude) | [Kepler 如何用 Claude 为金融服务构建可验证 AI](/02-claude/2604-kepler-verifiable-finance/2604-kepler-verifiable-finance-bilingual.md) | ★★★ | Kepler：可验证的金融 AI |
| 2026-05-27 | [How CodeRabbit used Claude to build an agent orchestration system](https://claude.com/blog/how-coderabbit-used-claude-to-build-an-agent-orchestration-system) | [CodeRabbit 如何用 Claude 构建一套 agent 编排系统](/02-claude/2605-coderabbit-agent-orchestration/2605-coderabbit-agent-orchestration-bilingual.md) | ★★★★ | CodeRabbit 用 Claude 构建 agent 编排系统 |
| 2026-07-08 | [Working at the frontier: How Thomson Reuters builds AI for high-stakes professional work](https://claude.com/blog/working-at-the-frontier-how-thomson-reuters-builds-ai-for-high--stakes-professional-work) | [在前沿工作：Thomson Reuters 如何为高风险专业工作构建 AI](/02-claude/2607-thomson-reuters/2607-thomson-reuters-bilingual.md) | ★★★ | Thomson Reuters：高风险专业工作的 AI 构建 |
| 2026-07-10 | [Working at the frontier: How Cognition trusts Claude Fable 5 to work through the night](https://claude.com/blog/working-at-the-frontier-how-cognition-trusts-claude-fable-5-to-work-through-the-night) | [在前沿工作：Cognition 如何放心让 Claude Fable 5 通宵工作](/02-claude/2607-cognition-overnight/2607-cognition-overnight-bilingual.md) | ★★★★ | Cognition 让 agent 通宵自主干活：信任如何逐步建立 |
| 2026-07-13 | [Working at the frontier: How Hebbia builds AI for financial diligence that can't miss a detail](https://claude.com/blog/working-at-the-frontier-how-hebbia-builds-ai-for-financial-diligence-that-cant-miss-a-detail) | [在前沿工作：Hebbia 如何打造不容错漏任何细节的金融尽调 AI](/02-claude/2607-hebbia-financial-diligence/2607-hebbia-financial-diligence-bilingual.md) | ★★★ | Hebbia：金融尽调零容错场景的 AI 构建 |
| 2026-07-15 | [Working at the frontier: Why Base44 trusts Claude Fable 5 with their most challenging engineering work](https://claude.com/blog/working-at-the-frontier-why-base44-trusts-claude-fable-5-with-their-most-challenging-engineering-work) | [在前沿工作：Base44 为什么把他们最具挑战性的工程工作托付给 Claude Fable 5](/02-claude/2607-base44-frontier/2607-base44-frontier-bilingual.md) | ★★★ | Base44 把最具挑战的工程交给 Fable 5 |
| 2026-07-17 | [Working at the frontier: How Cursor knew Claude Fable 5 was ready for the hardest 1% of problems](https://claude.com/blog/working-at-the-frontier-cursor) | [在前沿工作：Cursor 如何确信 Claude Fable 5 已准备好应对最难的 1% 的问题](/02-claude/2607-cursor-hardest-problems/2607-cursor-hardest-problems-bilingual.md) | ★★★★ | Cursor 如何评估 Fable 5 能否承担最难任务 |
| 2026-07-20 | [Working at the frontier: How Rakuten builds agents overnight with Claude Fable 5](https://claude.com/blog/working-at-the-frontier-rakuten) | [在前沿工作：Rakuten 如何用 Claude Fable 5 让代理隔夜干活](/02-claude/2607-rakuten-overnight-agents/2607-rakuten-overnight-agents-bilingual.md) | ★★★ | Rakuten 隔夜跑 agent 的工作流 |
| 2026-07-21 | [How Datadog built a “universal machine tool” for Claude Code](https://claude.com/blog/how-datadog-built-a-universal-machine-tool-for-claude-code) | [Datadog 如何为 Claude Code 打造一台“万能机床”](/02-claude/2607-datadog-universal-machine-tool/2607-datadog-universal-machine-tool-bilingual.md) | ★★★★ | Datadog 给 Claude Code 造万能机器工具，打通内部系统 |
| 2026-07-22 | [How Outtake built a cyber investigator on Claude](https://claude.com/blog/how-outtake-built-a-cyber-investigator-on-claude) | [Outtake 如何基于 Claude 构建网络犯罪调查员](/02-claude/2607-outtake-cyber-investigator/2607-outtake-cyber-investigator-bilingual.md) | ★★★ | 网络安全调查 agent 的构建 |
| 2026-08-06 | [Millennium and Anthropic are building a digital risk analyst with Claude](https://claude.com/blog/millennium-and-anthropic-are-building-a-digital-risk-analyst-with-claude) | [Millennium 与 Anthropic 正在用 Claude 打造数字化风险分析师](/02-claude/2608-millennium-digital-risk-analyst/2608-millennium-digital-risk-analyst-bilingual.md) | ★★★ | Millennium 构建数字风险分析师 |
| 2026-08-17 | [How ABC Legal turned every employee into a builder with Claude Managed Agents](https://claude.com/blog/how-abc-legal-turned-every-employee-into-a-builder-with-claude-managed-agents) | [ABC Legal 如何用 Claude Managed Agents 让每位员工成为建造者](/02-claude/2608-abc-legal-managed-agents/2608-abc-legal-managed-agents-bilingual.md) | ★★★ | 从散乱实验到治理化 agent 舰队的落地路径 |
| 2026-08-20 | [How monday.com transformed its platform into an agent-first product where humans and agents collaborate](https://claude.com/blog/how-monday-com-transformed-its-platform-into-an-agent-first-product-where-humans-and-agents-collaborate) | [monday.com 如何把平台转型为 agent-first 产品](/02-claude/2608-monday-com-agent-first/2608-monday-com-agent-first-bilingual.md) | ★★★ | 25 万客户平台的 agent-first 转型一手案例 |
| 2026-08-26 | [How Warp builds self-improving agents on Claude](https://claude.com/blog/how-warp-builds-self-improving-agents-on-claude) | [Warp 如何在 Claude 上构建自改进 agent](/02-claude/2608-warp-self-improving-agents/2608-warp-self-improving-agents-bilingual.md) | ★★★ | 自改进 agent 的可复用开发模式案例 |

#### 四、Agent 安全与治理（8 篇）

*agent 落地的权限、身份与零信任问题，代表官方安全立场*

| 发布时间 | 英文原文标题 | 中英文版本 | 重要程度 | 主要看点 |
| --- | --- | --- | --- | --- |
| 2025-10-08 | [Beyond permission prompts: making Claude Code more secure and autonomous](https://claude.com/blog/beyond-permission-prompts-making-claude-code-more-secure-and-autonomous) | [超越权限提示：让 Claude Code 更安全也更自主](/02-claude/2510-beyond-permission-prompts/2510-beyond-permission-prompts-bilingual.md) | ★★★★ | 权限确认之外的沙箱与安全模型，兼顾自主性 |
| 2026-04-10 | [Preparing your security program for AI-accelerated offense](https://claude.com/blog/preparing-your-security-program-for-ai-accelerated-offense) | [让你的安全项目为 AI 加速的攻击做好准备](/02-claude/2604-security-program-ai-offense/2604-security-program-ai-offense-bilingual.md) | ★★★ | 为 AI 加速的攻击面做安全准备 |
| 2026-05-27 | [Zero Trust for AI agents](https://claude.com/blog/zero-trust-for-ai-agents) | [面向 AI 智能体的零信任](/02-claude/2605-zero-trust-ai-agents/2605-zero-trust-ai-agents-bilingual.md) | ★★★★ | agent 的零信任架构设计 |
| 2026-05-27 | [Using LLMs to secure source code](https://claude.com/blog/using-llms-to-secure-source-code) | [用 LLM 保障源代码安全](/02-claude/2605-llms-secure-source-code/2605-llms-secure-source-code-bilingual.md) | ★★★ | 用 LLM 做源码安全审查 |
| 2026-06-24 | [Agent identity in Claude Tag: a new access model for autonomous, team-wide AI](https://claude.com/blog/agent-identity-access-model) | [Claude Tag 中的智能体身份：面向自主、团队级 AI 的新型访问模型](/02-claude/2606-agent-identity-access-model/2606-agent-identity-access-model-bilingual.md) | ★★★★ | Claude Tag 的 agent 身份访问模型：自主 agent 的团队级权限设计 |
| 2026-07-17 | [Zero risk isn't the job: a CISO's guide to agentic AI](https://claude.com/blog/ciso-guide-to-agentic-ai) | [零风险并非职责所在：CISO 的智能体 AI 指南](/02-claude/2607-ciso-guide-agentic-ai/2607-ciso-guide-agentic-ai-bilingual.md) | ★★★ | CISO 视角的 agentic AI 风险与对策清单 |
| 2026-07-21 | [How Anthropic secures its AI-native software development lifecycle](https://claude.com/blog/how-anthropic-secures-its-ai-native-software-development-lifecycle) | [Anthropic 如何保障其 AI 原生软件开发生命周期的安全](/02-claude/2607-anthropic-secures-sdlc/2607-anthropic-secures-sdlc-bilingual.md) | ★★★ | Anthropic 自身 AI 原生 SDLC 的安全实践 |
| 2026-08-21 | [Bringing the cybersecurity capabilities of Claude Mythos 5 to more defenders](https://claude.com/blog/bringing-claude-mythos-5-to-more-defenders) | [把 Claude Mythos 5 的网络安全能力带给更多防守者](/02-claude/2608-mythos-5-defenders/2608-mythos-5-defenders-bilingual.md) | ★★★ | Project Glasswing 扩围与前沿模型安全部署进展 |

#### 五、产品与模型演进（16 篇）

*Claude 生态一年内的关键产品节点，理解 Managed Agents/Cowork/Skills/MCP 的演进主线*

| 发布时间 | 英文原文标题 | 中英文版本 | 重要程度 | 主要看点 |
| --- | --- | --- | --- | --- |
| 2025-04-15 | [Claude takes research to new places](https://claude.com/blog/research) | [Claude 把研究带向新境界](/02-claude/2504-claude-research/2504-claude-research-bilingual.md) | ★★★ | Research 深度研究功能 |
| 2025-09-11 | [Bringing memory to Claude](https://claude.com/blog/memory) | [为 Claude 带来记忆](/02-claude/2509-bringing-memory-to-claude/2509-bringing-memory-to-claude-bilingual.md) | ★★★ | 记忆功能发布 |
| 2025-10-16 | [Introducing Agent Skills](https://claude.com/blog/skills) | [Agent Skills 正式发布](/02-claude/2510-introducing-agent-skills/2510-introducing-agent-skills-bilingual.md) | ★★★★★ | Agent Skills 发布：Claude 生态最关键的扩展机制（即本库 yz-* skill 用的那套） |
| 2025-10-31 | [What is Model Context Protocol? Connect AI to your world](https://claude.com/blog/what-is-model-context-protocol) | [什么是模型上下文协议（MCP）？让 AI 连接你的世界](/02-claude/2510-what-is-mcp/2510-what-is-mcp-bilingual.md) | ★★★★ | MCP 官方科普：AI 如何连接外部世界 |
| 2026-03-23 | [Put Claude to work on your computer](https://claude.com/blog/dispatch-and-computer-use) | [让 Claude 在你的电脑上工作](/02-claude/2603-dispatch-and-computer-use/2603-dispatch-and-computer-use-bilingual.md) | ★★★ | Dispatch 与 computer use 功能 |
| 2026-04-08 | [Claude Managed Agents: get to production 10x faster](https://claude.com/blog/claude-managed-agents) | [Claude Managed Agents：10 倍速直达生产环境](/02-claude/2604-claude-managed-agents/2604-claude-managed-agents-bilingual.md) | ★★★★ | Managed Agents 发布：宣称 10 倍速度上生产的新产品形态 |
| 2026-04-23 | [Built-in memory for Claude Managed Agents](https://claude.com/blog/claude-managed-agents-memory) | [Claude Managed Agents 的内置记忆](/02-claude/2604-managed-agents-memory/2604-managed-agents-memory-bilingual.md) | ★★★ | Managed Agents 内置记忆 |
| 2026-05-19 | [New in Claude Managed Agents: dreaming, outcomes, and multiagent orchestration](https://claude.com/blog/new-in-claude-managed-agents) | [Claude Managed Agents 新功能：dreaming、outcomes 与多智能体编排](/02-claude/2605-new-in-managed-agents/2605-new-in-managed-agents-bilingual.md) | ★★★ | Managed Agents 更新：dreaming/outcomes/多智能体编排 |
| 2026-05-19 | [New in Claude Managed Agents: self-hosted sandboxes and MCP tunnels](https://claude.com/blog/claude-managed-agents-updates) | [Claude Managed Agents 新功能：自托管沙箱与 MCP 隧道](/02-claude/2605-managed-agents-sandboxes-tunnels/2605-managed-agents-sandboxes-tunnels-bilingual.md) | ★★★ | Managed Agents 更新：自托管沙箱与 MCP 隧道 |
| 2026-06-05 | [The Claude Cowork product guide](https://claude.com/blog/the-claude-cowork-product-guide) | [Claude Cowork 产品指南](/02-claude/2606-cowork-product-guide/2606-cowork-product-guide-bilingual.md) | ★★★ | Cowork 产品全景指南 |
| 2026-06-10 | [The evolution of agentic surfaces: building with Claude Managed Agents](https://claude.com/blog/building-with-claude-managed-agents) | [智能体界面的演进：使用 Claude Managed Agents 构建](/02-claude/2606-agentic-surfaces-managed-agents/2606-agentic-surfaces-managed-agents-bilingual.md) | ★★★ | agentic surfaces 演进论：Managed Agents 的产品思想 |
| 2026-07-06 | [A field guide to Claude Fable 5: Finding your unknowns](https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns) | [Claude Fable 5 实战指南：找出你的未知项](/02-claude/2607-fable-field-guide/2607-fable-field-guide-bilingual.md) | ★★★ | Fable 5 使用心法：用模型发现你的未知 |
| 2026-07-24 | [Claude models explained: choosing the best model for your use case](https://claude.com/blog/claude-models-explained-choosing-the-best-model-for-your-use-case) | [Claude 模型详解：如何为你的用例选择最合适的模型](/02-claude/2607-claude-models-explained/2607-claude-models-explained-bilingual.md) | ★★★ | 全模型线官方选型指南 |
| 2026-07-28 | [Bringing MCP 2026-07-28 to Claude](https://claude.com/blog/bringing-mcp-2026-07-28-to-claude) | [将 MCP 2026-07-28 带入 Claude](/02-claude/2607-bringing-mcp-2026-07-28/2607-bringing-mcp-2026-07-28-bilingual.md) | ★★★ | MCP 2026-07-28 新规范在 Claude 的落地 |
| 2026-08-20 | [Build production agents with computer use, the Skills API, and the Files API](https://claude.com/blog/computer-use-skills-api-files-api) | [用 computer use、Skills API 与 Files API 构建生产级 agent](/02-claude/2608-computer-use-skills-api-files-api/2608-computer-use-skills-api-files-api-bilingual.md) | ★★★★ | computer use、Skills API、Files API 同日 GA，新增 browser use 工具 |
| 2026-08-25 | [Claude's memory works everywhere, and you decide what's in it](https://claude.com/blog/claudes-memory-works-everywhere-and-you-decide-whats-in-it) | [Claude 的记忆无处不在，内容由你决定](/02-claude/2608-claude-memory-everywhere/2608-claude-memory-everywhere-bilingual.md) | ★★★ | 记忆跨产品统一：Claude 产品主线的关键节点 |

#### 六、行业趋势与组织（5 篇）

*趋势判断与组织层面的思考*

| 发布时间 | 英文原文标题 | 中英文版本 | 重要程度 | 主要看点 |
| --- | --- | --- | --- | --- |
| 2025-12-09 | [How enterprises are building AI agents in 2026](https://claude.com/blog/how-enterprises-are-building-ai-agents-in-2026) | [2026 年企业如何构建 AI 智能体](/02-claude/2512-enterprises-building-ai-agents-2026/2512-enterprises-building-ai-agents-2026-bilingual.md) | ★★★ | 企业构建 AI agent 的现状调查 |
| 2026-01-21 | [Eight trends defining how software gets built in 2026](https://claude.com/blog/eight-trends-defining-how-software-gets-built-in-2026) | [定义 2026 年软件构建方式的八大趋势](/02-claude/2601-eight-trends-2026/2601-eight-trends-2026-bilingual.md) | ★★★★ | 2026 软件构建八大趋势（年度必读） |
| 2026-03-19 | [Product management on the AI exponential ](https://claude.com/blog/product-management-on-the-ai-exponential) | [AI 指数曲线上的产品管理](/02-claude/2603-product-management-ai-exponential/2603-product-management-ai-exponential-bilingual.md) | ★★★ | 指数曲线上做产品管理 |
| 2026-04-29 | [Product development in the agentic era](https://claude.com/blog/product-development-in-the-agentic-era) | [Agentic 时代的产品开发](/02-claude/2604-product-development-agentic-era/2604-product-development-agentic-era-bilingual.md) | ★★★ | agent 时代的产品开发范式 |
| 2026-05-14 | [The founder's playbook: Building an AI-native startup](https://claude.com/blog/the-founders-playbook) | [创始人手册：打造 AI 原生初创公司](/02-claude/2605-founders-playbook/2605-founders-playbook-bilingual.md) | ★★★ | AI 原生创业手册 |

（Claude 区共 84 篇已译，完整清单与星级见[文章登记总表](/02-claude/claude-blog.md)）

### Anthropic 研究博客（anthropic.com/research，24 篇）

*Alignment / Interpretability / Economics / Science 等研究侧内容，与工程博客、产品博客互补*

| 发布时间 | 英文原文标题 | 中英文版本 | 重要程度 | 主要看点 |
| --- | --- | --- | --- | --- |
| 2026-01-08 | [Experimenting with AI to defend critical infrastructure](https://www.anthropic.com/research/critical-infrastructure-defense) | [用 AI 防卫关键基础设施](/04-anthropic-research/2601-critical-infrastructure-defense/2601-critical-infrastructure-defense-bilingual.md) | ★★★ | 与 PNNL 合作：水处理厂工控仿真上的对手仿真从数周缩到 3 小时 |
| 2026-01-09 | [Next-generation Constitutional Classifiers](https://www.anthropic.com/research/next-generation-constitutional-classifiers) | [新一代宪法分类器：更高效地防住通用越狱](/04-anthropic-research/2601-next-generation-constitutional-classifiers/2601-next-generation-constitutional-classifiers-bilingual.md) | ★★★★★ | 线性探针 + 探针-分类器集成级联：拒答率降 87%、开销降至 ~1%，红队 19.8 万次尝试未见通用越狱 |
| 2026-01-14 | [Finding bugs across the Python ecosystem with Claude and property-based testing](https://www.anthropic.com/research/property-based-testing) | [用 Claude 与基于属性的测试找出 bug](/04-anthropic-research/2601-property-based-testing/2601-property-based-testing-bilingual.md) | ★★★★ | PBT agent 在 NumPy/Pandas 等真实包批量找 bug，多个补丁已合并 |
| 2026-01-15 | [Anthropic Economic Index report: Economic primitives](https://www.anthropic.com/research/anthropic-economic-index-january-2026-report) | [Anthropic 经济指数报告：经济基元](/04-anthropic-research/2601-anthropic-economic-index-january-2026-report/2601-anthropic-economic-index-january-2026-report-bilingual.md) | ★★★★★ | 「经济基元」五维度量框架：技能/复杂度/自主性/成功率/用途，含可靠性修正后的生产率估计 |
| 2026-01-16 | [AI models are showing a greater ability to find and exploit vulnerabilities on realistic cyber ranges](https://www.anthropic.com/research/cyber-toolkits-update) | [AI 模型在真实网络靶场上展现更强的漏洞发现与利用能力](/04-anthropic-research/2601-cyber-toolkits-update/2601-cyber-toolkits-update-bilingual.md) | ★★★★ | Sonnet 4.5 无需定制工具包即能在 Equifax 仿真中完成多阶段渗透（2/5 次） |
| 2026-01-19 | [The assistant axis](https://www.anthropic.com/research/assistant-axis) | [助手轴](/04-anthropic-research/2601-assistant-axis/2601-assistant-axis-bilingual.md) | ★★★★ | 人格空间中的「助手」方向：激活封顶稳定人格、抵御人格漂移越狱 |
| 2026-01-22 | [Claude's new constitution](https://www.anthropic.com/research/claude-new-constitution) | [Claude 的新宪法](/04-anthropic-research/2601-claude-new-constitution/2601-claude-new-constitution-bilingual.md) | ★★★★ | 宪法从原则清单升级为"写给 Claude 读"的完整文档，CC0 全文公开 |
| 2026-01-28 | [Disempowerment patterns in real-world AI usage](https://www.anthropic.com/research/disempowerment-patterns) | [真实 AI 使用中的失权模式](/04-anthropic-research/2601-disempowerment-patterns/2601-disempowerment-patterns-bilingual.md) | ★★★★ | 150 万对话的首次大规模失权测量：三类扭曲、放大因子与感知落差 |
| 2026-01-29 | [How AI assistance impacts the formation of coding skills](https://www.anthropic.com/research/AI-assistance-coding-skills) | [AI 辅助如何影响编程技能的形成](/04-anthropic-research/2601-AI-assistance-coding-skills/2601-AI-assistance-coding-skills-bilingual.md) | ★★★★ | 随机对照试验：AI 组测验低 17%，交互模式决定学习留存 |
| 2026-02-05 | [Evaluating and mitigating the growing risk of LLM-discovered 0-days](https://www.anthropic.com/research/zero-days) | [LLM 发现的 0 日漏洞](/04-anthropic-research/2602-zero-days/2602-zero-days-bilingual.md) | ★★★★ | Opus 4.6 开箱即用找出 500+ 高危漏洞，"防守方窗口"论的关键证据 |
| 2026-02-18 | [Measuring AI agent autonomy in practice](https://www.anthropic.com/research/measuring-agent-autonomy) | [在实践中测量 AI 智能体的自主性](/04-anthropic-research/2602-measuring-agent-autonomy/2602-measuring-agent-autonomy-bilingual.md) | ★★★★★ | 数百万交互实测：自主时长三个月翻倍、老手多放权也多打断、agent 自主暂停多于人类打断 |
| 2026-02-23 | [The persona selection model](https://www.anthropic.com/research/persona-selection-model) | [人格选择模型](/04-anthropic-research/2602-persona-selection-model/2602-persona-selection-model-bilingual.md) | ★★★ | 后训练是在预训练人格空间内"选角与润色"，而非从零塑造 |
| 2026-07-06 | [A global workspace in language models](https://www.anthropic.com/research/global-workspace) | [语言模型中的全局工作空间](/04-anthropic-research/2607-global-workspace/2607-global-workspace-bilingual.md) | ★★★★ | Claude 内部自发涌现的 J-space「心理工作区」：可报告、可调控、供推理，兼可监控隐藏意图 |
| 2026-07-08 | [An off switch for dual-use knowledge in AI models](https://www.anthropic.com/research/off-switch-dual-use) | [给 AI 模型中的两用知识装上「开关」](/04-anthropic-research/2607-off-switch-dual-use/2607-off-switch-dual-use-bilingual.md) | ★★★ | GRAM 梯度路由辅助模块：两用知识圈进可删除的权重分区，一次训练 16 种配置 |
| 2026-07-09 | [Claude plays robotics](https://www.anthropic.com/research/claude-plays-robotics) | [Claude 在机器人任务上的表现如何](/04-anthropic-research/2607-claude-plays-robotics/2607-claude-plays-robotics-bilingual.md) | ★★★ | 12 模型 × 4 控制接口 × 多本体的大规模具身横评：高层控制远胜直接驱动 |
| 2026-07-13 | [Claude's values across models and languages](https://www.anthropic.com/research/claude-values-models-languages) | [Claude 的价值观如何随模型与语言而变化](/04-anthropic-research/2607-claude-values-models-languages/2607-claude-values-models-languages-bilingual.md) | ★★★★ | 30 万对话压缩出四条价值轴，量化跨模型与跨语言的价值漂移 |
| 2026-07-24 | [Project Pilot: Can AI control a drone?](https://www.anthropic.com/research/project-pilot) | [Project Pilot：AI 模型能飞无人机吗？](/04-anthropic-research/2607-project-pilot/2607-project-pilot-bilingual.md) | ★★★ | Drone-Bench：无人机定位跟随五子任务，Fable 5 四项过人机基线 |
| 2026-07-28 | [Discovering cryptographic weaknesses with Claude](https://www.anthropic.com/research/discovering-cryptographic-weaknesses) | [用 Claude 发现密码学弱点](/04-anthropic-research/2607-discovering-cryptographic-weaknesses/2607-discovering-cryptographic-weaknesses-bilingual.md) | ★★★★ | HAWK 后量子签名密钥强度减半、7 轮 AES 攻击提速 200–800 倍 |
| 2026-08-10 | [Learning more about Claude's mathematical capabilities](https://www.anthropic.com/research/riemann-zeta) | [深入了解 Claude 的数学能力](/04-anthropic-research/2608-riemann-zeta/2608-riemann-zeta-bilingual.md) | ★★★★ | 研究版 Claude 改进黎曼 ζ 函数零点比例下界至 67.2%，经人类数学家验证 |
| 2026-08-12 | [Reviewing the evidence on worker retraining programs](https://www.anthropic.com/research/reviewing-the-evidence-on-worker-retraining-programs) | [转岗培训项目的效果究竟如何？](/04-anthropic-research/2608-reviewing-the-evidence-on-worker-retraining-programs/2608-reviewing-the-evidence-on-worker-retraining-programs-bilingual.md) | ★★★★ | 56 项随机实验元分析：AI 冲击下最热门政策选项的证据基础 |
| 2026-08-13 | [Patterns and problems in emerging multiagent systems](https://www.anthropic.com/research/multiagent-systems) | [多智能体系统的模式与问题](/04-anthropic-research/2608-multiagent-systems/2608-multiagent-systems-bilingual.md) | ★★★★★ | 从众、认知失灵、目标冲突：多智能体系统性失败的三类根源（年度必读） |
| 2026-08-18 | [How Claude is accelerating protein design and analytical chemistry](https://www.anthropic.com/research/Claude-accelerates-protein-design) | [Claude 如何加速蛋白质设计与分析化学](/04-anthropic-research/2608-claude-accelerates-protein-design/2608-claude-accelerates-protein-design-bilingual.md) | ★★★★ | 真实湿实验验证的蛋白质从头设计战役与分析化学全流程自动化 |
| 2026-08-26 | [Enabling independent research on how people use Claude](https://www.anthropic.com/research/enabling-independent-research) | [让外部研究者独立研究人们如何使用 Claude](/04-anthropic-research/2608-enabling-independent-research/2608-enabling-independent-research-bilingual.md) | ★★★ | 首次向外部研究者开放隐私保护使用数据的试点 |
| 2026-08-28 | [Automated researchers can reliably mitigate alignment failures](https://www.anthropic.com/research/automated-researchers-mitigate-alignment-failures) | [自动化研究员能够可靠地缓解对齐失败](/04-anthropic-research/2608-automated-researchers-mitigate-alignment-failures/2608-automated-researchers-mitigate-alignment-failures-bilingual.md) | ★★★★ | Claude 自主修复 10 类对齐失败且不伤通用能力，对齐研究自动化闭环实证 |

（研究区共 24 篇已译，完整清单与星级见[文章登记总表](/04-anthropic-research/anthropic-research.md)）

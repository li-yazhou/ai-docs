# Code Review 正式发布（中英对照）

> 原文标题：Code Review is now generally available
> 原文链接：https://claude.com/resources/articles/code-review-is-now-generally-available
> 原文作者：Anthropic
> 发布日期：2026-10-15
> **翻译模型：** DeepSeek-V4.1-Flash
> **评分：** ★★☆☆☆ —— 纯功能 GA 公告，内容较薄，与 claude.dev 的「How the Claude Code team does code review」配套阅读
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

Code Review is out of research preview and available to Claude Team and Enterprise organizations.

Code Review 已结束研究预览（research preview），面向 Claude Team 与 Enterprise 组织开放。

> Eligible Claude Enterprise organizations can claim at least $10,000 in Code Review credits by January 15, 2027. Credits expire 30 days after you claim them. Terms apply.

> 符合条件的 Claude Enterprise 组织可在 2027 年 1 月 15 日前领取至少 10,000 美元的 Code Review 抵扣额度（credits）。额度在领取后 30 天过期。适用相关条款。

#### Code Review 有哪些新变化（What's new in Code Review）

Coding agents keep increasing the pace at which teams write code, and the review bottleneck hasn't eased. For general availability, we made Code Review faster, better at finding the bugs that matter, and easier to tune to each of your repositories.

coding agents（编码智能体）不断提升团队写代码的速度，而代码审查（code review）这一瓶颈却未见缓解。为迎接正式发布（general availability，GA），我们让 Code Review 更快、更擅长发现真正要紧的 bug，并且更容易针对你的每个代码仓库做调优。

Since we released the research preview, Code Review has gotten sharper, catching more of the bugs that would otherwise reach production. We also run Code Review on pull requests at Anthropic, which shows us where reviews slow our own engineers down. Teams using Code Review during the preview told us the rest: it was catching real issues before a human reviewer looked at the PR, but they wanted reviews that were quieter, sized to each change, and faster. Here's what we shipped:

自发布研究预览以来，Code Review 变得更敏锐，能捕获更多原本会流入生产环境的 bug。我们在 Anthropic 内部也把它跑在 pull request（PR，拉取请求）上，这让我们看到代码审查是在哪些环节拖慢了自己的工程师。预览期间使用 Code Review 的团队则补齐了其余反馈：它确实能在人类审查者看 PR 之前发现真实问题，但他们希望审查更「安静」（少噪音）、规模与每次改动相匹配，并且更快。以下是我们交付的内容：

- Speed: Most reviews now finish about 3x faster (~20 minutes down to 6-10 minutes) compared to during preview.
- 速度（Speed）：与预览期间相比，如今大多数审查的完成速度约快 3 倍（从约 20 分钟缩短到 6-10 分钟）。

- Quality: Find more of the bugs that matter, with clear, concise findings that state the problem, what triggers it, and the fix in a few sentences.
- 质量（Quality）：发现更多真正要紧的 bug，给出的结论清晰、简洁，用几句话说明问题是什么、由什么触发、以及如何修复。

- Flexibility: Admins choose Quick, Standard (default), or Deep for each repository, so routine codebases get a quick pass and the most critical ones get a deep review. Every level delivers high-quality reviews.
- 灵活度（Flexibility）：管理员可为每个仓库选择 Quick（快速）、Standard（标准，默认）或 Deep（深度）级别，让常规代码库走快速通道、最关键的代码库接受深度审查。每个级别都能交付高质量的审查。

#### 如何开始使用（Get started）

Code Review is now generally available for Team and Enterprise plans.

Code Review 现已面向 Team 与 Enterprise 套餐正式发布。

- For admins: Enable Code Review in your Claude Code settings, install the GitHub App, and select repositories you'd like to run reviews on.
- 管理员（For admins）：在 Claude Code 设置中启用 Code Review，安装 GitHub App，并选择你希望运行审查的仓库。

- For developers: Once enabled, reviews run automatically on new PRs. No configuration needed.
- 开发者（For developers）：启用后，审查会自动在新 PR 上运行，无需任何配置。

Explore the docs for more information.

更多信息请查阅文档（docs）。

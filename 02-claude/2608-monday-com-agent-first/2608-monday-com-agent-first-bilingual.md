# monday.com 如何把平台转型为 agent-first 产品（中英对照）

> 原文标题：How monday.com transformed its platform into an agent-first product where humans and agents collaborate
> 原文链接：https://claude.com/blog/how-monday-com-transformed-its-platform-into-an-agent-first-product-where-humans-and-agents-collaborate
> 原文作者：Anthropic（文中受访者：monday.com 团队）
> 发布日期：2026-08-20
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，值得一看）--25 万客户平台的 agent-first 转型一手案例
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

More than 250,000 companies, from small and midsize businesses to Fortune 500 organizations, use [monday.com](http://monday.com) to manage their work. When the company launched more than a decade ago, its core product was a visual interface that helped teams automate workflows and manage projects. Today, it has rearchitected its product from the ground up around a human-agent collaboration model where AI is woven into work at every level. With Claude at the core, monday’s new platform handles the technical complexity so customers can work at the AI frontier inside workflows they already know.

超过 25 万家公司，从小型与中型企业到财富 500 强组织，都在使用 [monday.com](http://monday.com) 管理工作。这家公司十多年前创立时，核心产品是一个帮助团队自动化工作流、管理项目的可视化界面。如今，它已围绕“人机协作”（human-agent collaboration）模式从零开始重构了产品，把 AI 编织进工作的每一层。以 Claude 为核心，monday 的新平台承担了技术上的复杂度，让客户能够在自己已经熟悉的工作流中工作于 AI 前沿。

"The shift to an agent-first product was one of the most significant decisions we've made as a company," said Daniel Lereya, chief product and technology officer at monday.com. "It meant fundamentally reimagining what the platform should do, not just adding AI to existing workflows. Our vision is for monday to be the place where people and AI agents work together seamlessly and Anthropic and Claude have been trusted partners in helping us bring that vision to life."

“转向 agent-first 产品是我们作为一家公司所做过的最重大的决定之一，”monday.com 首席产品与技术官 Daniel Lereya 说，“这意味着从根本上重新构想平台应该做什么，而不只是给现有工作流加上 AI。我们的愿景是让 monday 成为人类与 AI agent（agents）无缝协作的地方，而 Anthropic 和 Claude 一直是帮助我们把这个愿景变为现实的值得信赖的伙伴。”

## 撞上“AI 尘埃”的天花板（Hitting the “AI dust” ceiling）

monday’s rebuild unfolded in three phases. In the first phase, as frontier LLM technology matured and customer excitement grew, monday teams worked on embedding AI capabilities into its original platform. The effort culminated in May 2025 with an internal “AI month,” with four weeks dedicated to shipping AI features or products across the company.

monday 的重构分三个阶段展开。在第一阶段，随着前沿 LLM 技术走向成熟、客户热情不断高涨，monday 各团队致力于把 AI 能力嵌入其原有平台。这一努力在 2025 年 5 月迎来顶点：公司内部举办“AI 月”（AI month），用整整四周时间在全公司范围内交付 AI 功能或产品。

Adoption was strong and generated excitement, but soon the team hit a ceiling. “We were building ‘AI dust’, sprinkling automations onto existing workflows without embedding them within or changing the product’s fundamental value proposition,” says Orly Stern Izhaki, VP of Product, AI Works Platform at monday.com. “Our features helped users summarize text and categorize information, but they weren’t creating sustained usage patterns.”

采用情况良好，也激发了不少兴奋，但团队很快撞上了天花板。“我们当时在制造‘AI 尘埃’（AI dust）：把自动化撒到现有工作流上，却没有把它们嵌入其中，也没有改变产品根本的价值主张，”monday.com AI Works Platform 产品副总裁 Orly Stern Izhaki 说，“我们的功能帮用户总结文本、归类信息，但并没有形成持续的使用模式。”

The company needed to shift focus from  adding AI to product features to building it natively into the platform. "Adopting AI features is not the same as becoming an AI company," Izhaki says. "Once we understood that, everything changed."

公司需要把重心从“给产品功能添加 AI”转向“把 AI 原生地构建进平台”。“采用 AI 功能并不等于成为一家 AI 公司，”Izhaki 说，“一旦想明白了这一点，一切都变了。”

That’s how Izhaki’s team set out to reimagine monday completely: from a work management tool to a place where people and agents get work done together. While the mandate to transform the product came from the top, it was up to each team and each employee to translate that north star into concrete product choices and build their own agents.

正因如此，Izhaki 的团队着手彻底重新构想 monday：从一个工作管理工具，变成人们与 agent（agents）共同完成工作的地方。转型产品的使命虽然来自最高层，但要把这颗“北极星”落实为具体的产品选择、并构建各自的 agent，靠的是每个团队、每位员工。

After months of intense work, the company announced the most significant change in its history, rebuilding its entire product experience around humans and agents working together, using already built-in context, workflows, boards, permissions, and governance. Since launching in May 2026, monday’s customers have had more than 5 million interactions with agents on its platform.

经过数月的紧张工作，公司宣布了其历史上最重大的变革：围绕人类与 agent 协作重建整个产品体验，充分利用已有的上下文、工作流、看板（boards）、权限与治理体系。自 2026 年 5 月上线以来，monday 客户已与其平台上的 agent 产生了超过 500 万次交互。

## Agent 即队友（Agents as teammates）

Along with access permissions and restrictions, each monday agent is given a name and an avatar, and colleagues can assign agents work through triggers and mentions in the monday platform.

除了访问权限与限制之外，每个 monday agent 都有一个名字和一个头像，同事可以在 monday 平台内通过触发器（triggers）和提及（mentions）给 agent 分派工作。

This design was intentional, addressing a pattern monday noticed across its customer base: many enterprises want to put AI to work, but often stall at an AI chat that runs parallel to where they actually do the work. Embedding agents directly into workflows and enabling people to interact with them like they would with their colleagues turned agentic AI from an abstract concept to a concrete, actionable one.

这种设计是有意为之，针对的是 monday 在客户群中注意到的一种模式：许多企业想让 AI 投入运转，却常常卡在一个与实际工作场所并行运转的 AI 聊天上。把 agent 直接嵌入工作流、让人们可以像对待同事一样与它们互动，使 agentic AI 从一个抽象概念变成了具体、可行动的东西。

The jobs monday has mapped for agents range from IT ticket triage and knowledge-base upkeep to candidate sourcing and interview scheduling, competitive-intelligence briefings for sales and marketing, and chief-of-staff work like meeting prep and converting decisions into tracked tasks.

monday 为 agent 规划的职责涵盖：IT 工单分诊与知识库维护、候选人搜寻与面试安排、面向销售与营销的竞争情报简报，以及会议准备、把决策转化为可跟踪任务等“幕僚长”（chief-of-staff）工作。

*Agent teams and their jobs for four common workflows.*
*四种常见工作流中 agent 团队及其职责。*

| Use case | Jobs |
|---|---|
| IT — From ticket to resolution | **Intake & Triage Agent** — classify tickets, auto-resolve common requests, escalate with full context **Knowledge Agent** — detect knowledge gaps, draft new KB articles **Incident Agent** — detect incidents, open war rooms, trigger post-mortems |
| HR — From job post to hire | **Resume Screener** — score applications, surface top candidates, send rejections **Interview Scheduler** — handle all scheduling and confirmations **Hiring Coordinator** — keep all stakeholders updated throughout the process, so there is always a human in the loop **Feedback Manager** — collect structured interviewer feedback automatically |
| Marketing — Competitive intelligence | **Competitive Intelligence Agent** — monitor competitors, detect and categorize signals, send alerts and weekly briefings **Battlecard Agent** — update battlecards on approved signals, notify sales immediately |
| Executive Office — Chief of Staff as a Service | **Operator Agent** — book meetings, prep briefings, convert decisions into tracked tasks, monitor priorities **Org Health Agent** — scan for revenue risks, cost leaks, and failing initiatives **Strategy Consultant Agent** — identify growth opportunities, generate action plans |

| 用例 | 职责 |
|---|---|
| IT — From ticket to resolution | **Intake & Triage Agent** — classify tickets, auto-resolve common requests, escalate with full context **Knowledge Agent** — detect knowledge gaps, draft new KB articles **Incident Agent** — detect incidents, open war rooms, trigger post-mortems |
| HR — From job post to hire | **Resume Screener** — score applications, surface top candidates, send rejections **Interview Scheduler** — handle all scheduling and confirmations **Hiring Coordinator** — keep all stakeholders updated throughout the process, so there is always a human in the loop **Feedback Manager** — collect structured interviewer feedback automatically |
| Marketing — Competitive intelligence | **Competitive Intelligence Agent** — monitor competitors, detect and categorize signals, send alerts and weekly briefings **Battlecard Agent** — update battlecards on approved signals, notify sales immediately |
| Executive Office — Chief of Staff as a Service | **Operator Agent** — book meetings, prep briefings, convert decisions into tracked tasks, monitor priorities **Org Health Agent** — scan for revenue risks, cost leaks, and failing initiatives **Strategy Consultant Agent** — identify growth opportunities, generate action plans |

## 在 monday 中运行 Claude 的四种方式（Four ways to run Claude in monday）

Customers use Claude inside the monday platform through four capabilities:

客户通过四种能力在 monday 平台内使用 Claude：

With **monday Agents**, teams can build custom agents using prompts, and choose Claude as its model. The platform gives the agent a name, a face, and a place on the board where anyone can assign it work.

通过 **monday Agents**，团队可以用提示词（prompts）构建自定义 agent，并选择 Claude 作为其模型。平台为 agent 起好名字、配好形象，并在看板上给它留出一个位置，任何人都可以向它分派工作。

**Bring Your Own Agent (BYOA)** makes it possible for Claude Managed Agents to join the platform. Once on the monday platform, an agent one person has built can become a teammate the whole team can mention and assign work to.

**Bring Your Own Agent（BYOA，自带 agent）**让 Claude Managed Agents 得以加入平台。一旦进入 monday 平台，某个人构建的 agent 就能成为整个团队都可以提及、都可以派活的队友。

**Pre-built Agents**, available in the monday Agents Store, turn Claude plugins into specialized teammates: a legal team can run a legal plugin as an agent inside its own workflows, and finance teams can do the same with theirs.

**Pre-built Agents（预构建 agent）**可在 monday Agents Store 获取，把 Claude 插件变成专门的队友：法务团队可以在自己的工作流中把法律插件当 agent 运行，财务团队也可以对各自的插件如法炮制。

**The Claude Coding integration** enables teams to connect Claude in the monday dashboard, then plan and assign agents tasks. Claude Managed Agents executes in the customer's own environment, and results and updates land back on the ticket before the task hands off to the next agent or to a human for review. The work runs from business need to working code and back to the business user.

**Claude Coding 集成**让团队可以在 monday 仪表板中连接 Claude，然后规划任务并分派给 agent。Claude Managed Agents 在客户自己的环境中执行，结果与更新会落回工单上，之后任务再交接给下一个 agent 或交由人工审阅。整项工作从业务需求出发，化为可运行的代码，再回到业务用户手中。

## 不离开看板，从简报到落地页（From brief to landing page without leaving the board）

One end-to-end example: a marketing team runs a campaign production line inside a single board item. The marketer and content lead shape the brief on the item, aligning on goal, audience, key message, and channels. A Strategist Agent built with monday Agents turns that raw input into a structured brief covering the campaign objective, messaging pillars, channel breakdown, and success metrics.

一个端到端的例子：营销团队在单个看板条目内运行一条活动生产线。营销人员与内容负责人在该条目上打磨简报（brief），就目标、受众、关键信息与渠道达成一致。一个用 monday Agents 构建的 Strategist Agent 把这些原始输入变成一份结构化简报，涵盖活动目标、信息支柱（messaging pillars）、渠道拆分与成功指标。

From there, a Landing Page Builder takes over. Running on Claude Managed Agents in the company's own environment, it pulls the approved brief and generates a new variant of an existing landing page, with copy, structure, and messaging adapted to the campaign. The output lands back on the monday item automatically. Before the page reaches approval, a Brand Reviewer, a Claude Managed Agent, checks it against brand guidelines and legal standards and flags anything that needs human attention. The marketing manager then makes one decision: publish or refine.

从那里开始，Landing Page Builder 接手。它运行于公司自己环境中的 Claude Managed Agents，拉取已批准的简报，为现有落地页生成一个新的变体，文案、结构与信息都针对本次活动做了调整。产出自动落回 monday 条目。在页面进入审批之前，一个名为 Brand Reviewer 的 Claude Managed Agent 会对照品牌指南与法律标准进行检查，并标记出任何需要人工关注的地方。随后营销经理只需做一个决定：发布，还是继续打磨。

📹 视频演示：https://www.youtube.com/watch?v=3r3xdZsZQKY

## 站在 AI 前沿的家族企业（A family business at the AI frontier）

[Cooke](https://cookeseafood.com/), a family seafood business founded in 1985 in Blacks Harbour, New Brunswick, has grown from a single farm site with 5,000 salmon into the world's largest family-owned seafood company, operating in 16 countries. Today, Cooke runs project delivery, resource management, and contract management on Claude and monday together. Product managers use Claude to turn approved charters and requirements into initial project plans, generate status reports, and surface risks and issues that feed straight into their monday RAID logs, across roughly 200 active and proposed projects. Claude automates the reporting and data prep that keep lifecycle workflows accurate across 130 contracts—upkeep work that used to be tedious and manual.

[Cooke](https://cookeseafood.com/) 是一家家族海鲜企业，1985 年创立于新不伦瑞克省的 Blacks Harbour，从一座只有 5,000 条鲑鱼的养殖场起步，成长为全球最大的家族持有海鲜公司，业务遍布 16 个国家。如今，Cooke 在约 200 个活跃与拟议项目上，同时用 Claude 和 monday 来运行项目交付、资源管理与合同管理。产品经理用 Claude 把已批准的章程与需求转化为初始项目计划、生成状态报告，并识别出直接进入 monday RAID 日志的风险与问题。Claude 还把让 130 份合同生命周期工作流保持准确所需的报告与数据准备工作自动化--这类维护工作过去既繁琐又全靠手工。

“Together, monday and Claude help us read team capacity and make smarter allocation calls,” says Patti Stevens, director of strategy at Cooke. “Monday used to be a platform we had to update. Now we operate from it.”

“monday 和 Claude 一起帮助我们读懂团队产能，做出更聪明的调配决策，”Cooke 战略总监 Patti Stevens 说，“过去 monday 是一个我们必须去更新的平台。现在我们以它为基地开展工作。”

## monday.com 学到了什么（What monday.com learned）

For companies planning a similar rebuild, monday’s team shares five lessons they learned as they transformed their platform into an AI-first product:

对于计划进行类似重构的公司，monday 团队分享了他们在把平台转型为 AI-first 产品过程中学到的五条经验：

- The mental model is harder to change than the technology.  People naturally want to protect quality and keep improving what already works. Moving teams from "how do we responsibly improve the current product?" to "how do we responsibly rebuild it for a different future?" took longer than the technical work.

- Small teams move faster when everything is changing at the same time.  Direction, UX, technology, pricing, the trust model, and the company's own definition of good were all in motion at the same time. Layers of stakeholders would lose that much detail, but small teams with clear ownership and fast decision rights stayed close to it.

- Adoption depends on trust as much as it does on capability.  Product-market fit depends on user confidence and preparedness to let agents into how work actually gets done. Governance, permissions, transparency, and reliability determine whether agents move beyond pilot programs and into production.

- Capability needs infrastructure to match.  Agents perform at a different level when they're grounded in live project data, team history, and structured workflows, and at enterprise scale the backend has to hold. Alongside the agent layer, monday invested in monday DB so the data infrastructure could support the volume, speed, and complexity of agents operating across an organization.

- Build on what already works.  monday has always described itself as the place where people team up to drive business outcomes, and the agent-first rebuild extends that promise to a new kind of team member. People still come to monday to achieve their goals, the difference is that some of the team members working alongside them are now agents.

- 心智模式比技术更难改变。人们天然想保护质量、持续改进已经奏效的东西。让团队从“我们如何负责任地改进现有产品？”转向“我们如何负责任地为另一种未来重建它？”，所花的时间比技术工作本身更长。

- 当一切同时在变时，小团队跑得更快。方向、UX、技术、定价、信任模型，以及公司自己对“好”的定义，都在同时变动。层层利益相关者会丢失大量细节，而权责清晰、决策权快的小团队能始终贴近这些细节。

- 采用取决于信任，不亚于取决于能力。产品市场匹配（product-market fit）取决于用户的信心，以及他们是否准备好让 agent 进入工作真正完成的方式。治理、权限、透明度与可靠性，决定了 agent 能否走出试点项目、进入生产环境。

- 能力需要与之匹配的基础设施。当 agent 扎根于实时项目数据、团队历史和结构化工作流时，表现会处于另一个水平；而在企业规模下，后端必须撑得住。在 agent 层之外，monday 还投资了 monday DB，让数据基础设施能够支撑 agent 在整个组织中运作所需的容量、速度与复杂性。

- 在已经奏效的东西之上构建。monday 一直把自己描述为“人们组队驱动业务成果的地方”，agent-first 重构把这一承诺延伸到了一种新的团队成员。人们仍然来到 monday 实现自己的目标，不同的是，如今与他们并肩工作的团队成员中，有一些是 agent。

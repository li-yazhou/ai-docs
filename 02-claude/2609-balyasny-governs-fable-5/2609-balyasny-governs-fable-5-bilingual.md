# Working at the frontier：Balyasny 资产管理如何评估与治理 Claude Fable 5（中英对照）

> 原文标题：Working at the frontier: How Balyasny Asset Management evaluates and governs Claude Fable 5
> 原文链接：https://claude.com/blog/working-at-the-frontier-how-balyasny-asset-management-evaluates-and-governs-claude-fable-5
> 原文作者：Anthropic
> 发布日期：2026-09-17
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆—— 金融机构评估与治理前沿模型的一手流程，frontier 系列新篇
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

Balyasny Asset Management (BAM) is a global, multi-strategy investment firm that manages roughly $38 billion in assets and supports a team of roughly 2,000 investment professionals and staff. Charlie Flanagan, Chief AI Officer, spoke with Anthropic about how the firm evaluates new models on thousands of real financial tasks, why it built its own platform for running agents, and what changed with the launch of Claude Fable 5.

Balyasny Asset Management（BAM，Balyasny 资产管理公司）是一家全球性多策略投资机构，管理资产规模约 380 亿美元，拥有约 2,000 名投资专业人士与员工。首席 AI 官 Charlie Flanagan 与 Anthropic 谈到了公司如何在数千个真实金融任务上评估新模型、为何要自建运行 agent（智能体）的平台，以及 Claude Fable 5 的发布带来了哪些变化。

### 2026 年，前沿 AI 对 BAM 来说发生了什么变化？（How has frontier AI changed for BAM in 2026?）

2026 is the year we moved from AI systems that do search to AI systems that do work.

2026 年，我们从"做搜索的 AI 系统"迈向了"做工作的 AI 系统"。

The key change has been not just the models, but the harnesses around them, such as Claude Code. They allow the AI solutions we build to take on vastly more complex and longer-running tasks. The ability to give an AI an outcome rather than a prompt, and have it keep working until complete, has been a gamechanger.

关键的变化不仅在于模型本身，还在于围绕模型的 harness（执行框架），例如 Claude Code。它们让我们构建的 AI 方案能够承担复杂程度高得多、运行时间长得多的任务。能够给 AI 一个目标结果而非一条提示词，并让它持续工作直到完成，这彻底改变了游戏规则。

A practical example is merger-arbitrage analysis. When a deal is announced, agents now build the initial deal-analysis package. They estimate how likely the deal is to close and how long it will take, extract the key economic and legal terms, identify conditions and milestones, and flag the areas that need investor judgment.

一个实际的例子是并购套利（merger-arbitrage）分析。当一笔交易公布时，agent 现在会搭建初始的交易分析材料包：估算交易达成的概率与所需时间，提取关键的经济与法律条款，识别前提条件与里程碑，并标出需要投资者判断的领域。

A year ago, those steps were fragmented across manual research and separate tools; we did not have an agent that could reliably sustain the full multi-step workflow to a usable conclusion. That work used to take three to five days. Now it takes less than one. The agent runs for approximately 30 minutes, with human review before any material output is relied on.

一年前，这些步骤分散在人工研究和彼此独立的工具之中；我们还没有一个 agent 能够可靠地支撑完整的多步骤工作流并得出可用的结论。这项工作过去需要三到五天，现在不到一天。agent 大约运行 30 分钟，且在任何重要产出被采信之前都会经过人工审核。

We use Anthropic's frontier models, but most of the infrastructure is built in-house at BAM, including the execution harness, data access, and review controls.

我们使用 Anthropic 的前沿模型，但大部分基础设施由 BAM 自建，包括执行 harness、数据访问与审核控制。

### 在启用 Claude Fable 5 之前，你们如何评估它？（How did you evaluate Claude Fable 5 before turning it on?）

One thing we did years ago which has served us extremely well was invest in robust evaluation systems. We test new models on thousands of real-world financial tasks with verifiable outcomes, across equities, macro, and commodities, rather than relying on general benchmarks or isolated demonstrations. It has allowed us to make data-driven decisions around model choice and routing, and is something I think all enterprises should invest in.

多年前我们做过一件后来被证明极有价值的事：投资建设稳健的评估体系。我们在数千个结果可验证的真实金融任务上测试新模型，覆盖股票、宏观与大宗商品，而不是依赖通用基准或孤立的演示。这让我们能够围绕模型选择与路由做出数据驱动的决策——我认为所有企业都值得在这方面投入。

We test both the model on its own and how it performs inside our agentic environment, with the same tools, files, and requirements our users have. Can it plan the work, choose and use the right tools, find and analyze evidence, recover from errors, check its intermediate results, and produce a grounded deliverable? We also look for specific failure modes, like numerical errors, missed coverage, unsupported conclusions, and retrieval problems.

我们既测试模型本身，也测试它在我们的 agentic（智能体式）环境中的表现——使用与用户相同的工具、文件和要求。它能否规划工作、选择并使用正确的工具、查找并分析证据、从错误中恢复、检查自己的中间结果，并产出有依据的交付物？我们还会关注特定的失败模式，例如数值错误、覆盖遗漏、缺乏依据的结论以及检索问题。

On the relevant subset, Fable achieved 89.4% versus 86.1% for the prior production model, across thousands of tasks. Where it stood out most was complex planning, analysis, and agentic execution.

在相关的任务子集上，横跨数千个任务，Fable 取得了 89.4% 的成绩，而上一代生产模型为 86.1%。它最突出的地方在于复杂规划、分析与 agentic 执行。

The surprising result was a set of economics problems we have tested that we have never had a model complete successfully, until Fable. We initially treated the result as a potential evaluation issue because it represented a material step change versus every model we had tested. We reran the evaluation, independently checked the task and scoring logic, and reviewed the result with Anthropic before concluding that the improvement was real. It was a wow moment.

最令人惊讶的结果，是一组我们测了很久、从未有任何模型成功完成的经济学题目——直到 Fable 出现。起初我们把这一结果当作潜在的评估问题来处理，因为相对于我们测过的所有模型，这都是一次重大跳变。我们重新跑了一遍评估，独立核查了任务与评分逻辑，并与 Anthropic 共同复核了结果，最终确认提升是真实的。那真是一个"哇"的时刻。

Today, our investment teams use Fable as their go-to frontier model for systematic and coding work. We give them guidance on when to use Fable versus other models, based on efficiency and cost.

如今，我们的投资团队把 Fable 作为系统化（systematic）与编码工作的首选前沿模型。我们会根据效率与成本，就何时该用 Fable、何时该用其他模型给出指引。

### 面对当今的前沿模型，你们如何看待安全？（How are you thinking about safety with today's frontier models?）

We treat safety as a product and operating-model question, not as a one-time model-selection exercise. The relevant questions are not only what the model can do, but what data it can access, what tools it can use, what actions it can take, what must remain human-approved, and how we will know when something has gone wrong.

我们把安全视为一个产品与运营模式层面的问题，而不是一次性的选型练习。关键问题不仅在于模型能做什么，还在于它能访问哪些数据、能使用哪些工具、能采取哪些行动、哪些环节必须由人类批准，以及出了问题时我们将如何得知。

That means putting controls around the model rather than assuming the model itself is the control. We use approved data boundaries, least-privilege access, tool-level permissions, logging and traceability, human review for material outputs, and clear escalation paths for edge cases. We also test adversarial and failure scenarios before broadening access.

这意味着把控制措施放在模型周围，而不是假设模型本身就是控制。我们采用经批准的数据边界、最小权限访问、工具级权限、日志与可追溯性、对重要产出的人工审核，以及针对边缘情况的清晰升级路径。在扩大使用范围之前，我们还会测试对抗性与失败场景。

Those controls were a day-one priority, and security did not fundamentally change with Fable. A more capable model does not receive broader authority simply because it can reason or plan more effectively. Models can use only the tools and data sources approved for that user and task, and they cannot grant themselves more access. Investment judgment and accountability remain with people.

这些控制从第一天起就是优先事项，引入 Fable 并没有从根本上改变安全体系。一个更强大的模型不会仅仅因为它推理或规划得更有效，就获得更大的权限。模型只能使用针对该用户和该任务获批的工具与数据源，无法为自己扩充权限。投资判断与问责始终由人来承担。

### BAMAgent 处于什么位置？（Where does BAMAgent fit in?）

Looking ahead, the direction of travel is toward more capable agents that can take longer-running, multi-step actions. That makes governance more important, not less. For us, that has meant building BAMAgent, our internal platform for securely deploying agents into approved enterprise workflows. It gives agents the tools and systems they need, but only those tools and systems. We have been building it for six months now and it supports thousands of autonomous agents working 24/7.

展望未来，发展方向是能力更强的 agent，能够执行运行时间更长、步骤更多的行动。这让治理变得更加重要，而不是更不重要。对我们而言，这意味着构建 BAMAgent——我们用于把 agent 安全部署进获批企业工作流的内部平台。它为 agent 提供所需的工具与系统，但也仅限这些工具与系统。我们已经建设了六个月，目前支持数千个全天候（24/7）运行的自主 agent。

BAMAgent is the next step beyond our chat platform. Chat helps people take in and synthesize information. BAMAgent does the work: multi-step research and analysis that can run for hours or days, with agents working in parallel, and it ends in something a person can review. It can build and maintain a company research package, prepare for an earnings or macro event, or turn new evidence into financial scenarios. The agent plans the work, uses approved internal systems, runs the analysis, checks its intermediate outputs, and returns a research artifact, model, or decision-support package.

BAMAgent 是我们聊天平台之后的下一步。聊天帮助人们获取并综合信息，而 BAMAgent 负责把活干完：多步骤的研究与分析可以运行数小时甚至数天，多个 agent 并行工作，最终交付可供人审核的成果。它可以搭建并维护公司研究材料包、为财报或宏观事件做准备，或把新证据转化为金融情景。agent 会规划工作、使用获批的内部系统、执行分析、检查中间输出，最终返回一份研究产物、模型或决策支持包。

Fable is our preferred model for the planning and analysis stages. A mistake there flows through every deliverable that follows, so we want the strongest available model deciding how to break down a problem, which evidence matters, and how to reconcile conflicting signals. That is what lets the agent work like a capable coworker.

Fable 是我们在规划与分析阶段的首选模型。这些环节一旦出错，错误会传导到其后的每一份交付物，因此我们希望由可用的最强模型来决定如何拆解问题、哪些证据重要、如何调和相互矛盾的信号。正是这一点让 agent 能像一位能干的同事那样工作。

Every enterprise should be developing a strategy to move toward a hosted-agent model that allows enterprise management and enforcement while maximizing the utility of agents for users.

每家企业都应当制定战略，迈向一种托管式 agent（hosted-agent）模式：既支持企业层面的管理与强制执行，又能最大化 agent 为用户创造的效用。

### Claude Fable 5 为 BAM 带来了哪些新的可能？（What has Claude Fable 5 made possible for BAM?）

The reaction has been incredibly positive. Ultimately, people care about what this technology can unlock in their day-to-day work.

反响极其积极。归根结底，人们在意的是这项技术能在日常工作中解锁什么。

In one example, a BAMAgent ran a tax-loss harvesting analysis. It explored 90,000 database tables, found the relevant mutual fund holdings data, and built its own weighting system. After a review by our team, the result was more comprehensive than what a traditional approach would have produced.

一个例子是：某个 BAMAgent 运行了一次税损收割（tax-loss harvesting）分析。它探索了 90,000 张数据库表，找到相关的共同基金持仓数据，并自建了一套加权体系。经过我们团队审核后，其结果比传统方法所能产出的更为全面。

Separately, our Chief Economist has configured an agent workflow that reduces a recurring central-bank analysis from roughly two days to approximately 30 minutes, with the economist retaining review and judgment.

另外，我们的首席经济学家配置了一个 agent 工作流，把一项周期性的央行分析从大约两天缩短到约 30 分钟，同时由经济学家保留审核与判断权。

Fable contributes the reasoning, synthesis, and multi-step problem-solving. BAM's harness provides the workflow design, approved data and tool access, retrieval context, permissions, monitoring, and human-review controls. Both are necessary for a production-quality result.

Fable 贡献推理、综合与多步骤问题求解能力；BAM 的 harness 则提供工作流设计、获批的数据与工具访问、检索上下文、权限、监控以及人工审核控制。要得到生产级质量的成果，两者缺一不可。

### 随着模型日益强大，你们的 AI 路线图下一步是什么？（As models become increasingly powerful, what's next on your AI roadmap?）

This is the year we go from people having tools to having teammates. Much like a teammate, agents will become more useful over time as you work with them, complete more complex tasks, and start to do work proactively to help.

今年，我们将从"人们拥有工具"走向"人们拥有队友"。就像队友一样，agent 会随着你与它们协作、完成更复杂的任务，并开始主动提供帮助，而变得越来越有用。

We already have some teams running over 300 agents doing analysis over new data and information constantly. It helps the teams both be faster to insights and not miss anything.

我们已经有一些团队在运行超过 300 个 agent，持续不断地对新数据与信息进行分析。这既让团队更快获得洞察，也帮助他们不遗漏任何东西。

It also changes the question we ask. We used to build expert systems and teach people to automate the processes they already had. Now we ask whether there is a better way to reach the outcome.

这也改变了我们所提出的问题。过去我们构建专家系统，教人们把已有的流程自动化；现在我们问的是：有没有更好的方式来达成结果。

The limits are really just our own imagination. The tools, data, and models are now at a point where they can do real work for hours on end; it's up to us to continue to reimagine what is possible. It is going to be an incredibly exciting next 12 months.

真正的限制其实只是我们自己的想象力。工具、数据和模型如今已经能够连续数小时地完成实实在在的工作；接下来要靠我们不断重新想象什么是可能的。未来 12 个月将会无比令人兴奋。

Get started with Claude Fable .

开始使用 Claude Fable。

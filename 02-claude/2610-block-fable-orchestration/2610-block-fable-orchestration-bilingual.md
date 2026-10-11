# Block 如何用 Claude Fable 编排数千个 pull request（中英对照）

> **原文标题：** How Block orchestrates Claude Fable across thousands of pull requests
> **原文链接：** https://claude.com/blog/how-block-orchestrates-claude-fable-across-thousands-of-pull-requests
> **原文作者：** Bradley Axen（Head of AI Capabilities at Block）
> **发布日期：** 2026-10-08
> **翻译模型：** DeepSeek-V4.1-Flash
> **评分：** ★★★★☆ —— 一篇少见的“规模化落地账本”：Block 把 Fable 当作编排器（orchestrator）、把具体改动交给数十个更便宜的小模型，并给出在数千人可自由使用前沿模型的前提下，如何同时管住成本（自动选择器、effort level 网格化评测）与安全（双重审批、默认安全、模型内建防护）的完整工程答案。
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。术语保留英文原文并附中文释义。

---

Bradley Axen, Head of AI Capabilities at Block, on how his team evaluates new large language models, how Claude Fable 5 orchestrates company-wide code migrations, and why safeguards are a critical part of deploying frontier intelligence responsibly, at scale.

Block 的 AI 能力负责人 Bradley Axen，谈他的团队如何评估新的大语言模型（large language model）、Claude Fable 5 如何编排全公司范围的代码迁移，以及为什么在规模化地负责任部署前沿智能（frontier intelligence）时，防护措施（safeguards）是不可或缺的一环。

Block builds tools that help businesses and individuals participate in the economy, including Square for sellers and Cash App for consumers. Bradley Axen leads AI capabilities at Block, where his team builds end-to-end AI products, including Buzz, an open-source workspace for human-agent teams, and partners with the Square and Cash App teams to build customer-facing AI features. Brad spoke with Anthropic about how frontier models like Claude Opus 5 and Claude Fable 5 have transformed what it means to be an engineer, how Block applies agent orchestration patterns with Fable, and the role of safeguards when every employee has access to frontier models.

Block 打造的工具有助于企业与个人参与经济活动，包括面向卖家的 Square 和面向消费者的 Cash App。Bradley Axen 在 Block 负责 AI 能力，他的团队构建端到端的 AI 产品，其中包括 Buzz——一个面向人机协作团队（human-agent team）的开源工作空间，并与 Square 和 Cash App 团队合作打造面向客户的 AI 功能。Brad 与 Anthropic 谈了像 Claude Opus 5 和 Claude Fable 5 这样的前沿模型如何改变了“工程师”的含义、Block 如何用 Fable 落地 agent 编排（agent orchestration）模式，以及在每位员工都能使用前沿模型时防护措施所扮演的角色。

# 前沿模型在 2026 年取得了实质性进展。这项技术如何影响了你们的工作方式？（Frontier models have made meaningful progress in 2026. How has this technology impacted how you work?）

The biggest impact AI has had on our work at Block is how we build things, and you couldn't overstate how impactful that is. We saw the beginning of it in 2025 with tools like Claude Code, which at the time, required a lot more manual intervention. As models become more intelligent, design and engineering work has shifted toward designing the migrations themselves, reviewing agent behavior, and building more customer-facing features, which was always the goal.

AI 对 Block 工作最大的影响在于我们“如何构建”，这一点的影响怎么强调都不为过。我们在 2025 年借助 Claude Code 这类工具看到了开端，当时它还需要多得多的手动介入。随着模型变得更智能，设计与工程工作已转向设计迁移方案本身、审阅 agent 行为，以及构建更多面向客户的功能——而这始终是我们的目标。

The next phase is to bring these transformative technologies to our customers, through Block's products. There's just as much value AI can bring to helping a small business handle the back-of-house operations it doesn't want to spend its time on, or to helping a Cash App customer manage their finances. With Claude, we have a whole new lever in technology that can change what people expect software to do for them. Imagine if AI could respond to customer needs and build new interfaces on the fly? Increasingly, I think our products will be built on demand with AI to solve specific problems.

下一阶段是通过 Block 的产品把这些变革性技术带给我们的客户。AI 同样能创造巨大价值：帮助小企业处理它们不愿花时间的后台运营（back-of-house operations），或帮助 Cash App 客户管理财务。有了 Claude，我们手上多了一个全新的技术杠杆，可以改变人们对软件能为他们做什么的预期。想象一下，如果 AI 能响应客户需求、即时构建新界面会怎样？我越来越认为，我们的产品将越来越多地借助 AI 按需构建，以解决具体问题。

# 随着前沿智能演进，你的团队现在能做哪些几个月前还做不到的事？（As frontier intelligence evolves, what can your team do now that it couldn't do a few months ago?）

The biggest thing that has changed with Claude's newer frontier models is the scale at which we can build software. Writing code is still the core component, but now we'll do company-wide migrations with AI. We've all had that dream for a long time, and in 2025 it rarely panned out. Before Claude Fable 5, you had to have a particularly well-shaped problem for frontier models to really shine.

Claude 较新的前沿模型带来的最大变化，是我们构建软件的规模。写代码仍然是核心环节，但现在我们会用 AI 做全公司范围的迁移。这个梦想我们做了很久，而 2025 年它很少真正实现。在 Claude Fable 5 之前，你必须有一个形态特别合适的问题，前沿模型才能真正发挥出色。

In 2026, AI is a major part of how we run all large-scale code migrations. For us, a migration might mean hundreds or thousands of individual pull requests across multiple repos and millions of lines of code. In the past, humans had to coordinate that work, and models could take on one or a few PRs at a time. With Fable, we're seeing it orchestrate the work across many sessions. A human engineer steers, and Fable focuses on the higher-level design work: data models, API specs, algorithms. It then directs up to dozens of smaller, more cost-effective models like Opus or Sonnet to do the individual tasks, such as making the actual file edits and running tests.

到 2026 年，AI 已是我们运行所有大规模代码迁移的主要方式。对我们来说，一次迁移可能意味着跨多个 repo 的数百甚至数千个独立 pull request，以及数百万行代码。过去，这类工作必须由人来协调，而模型一次只能承担一个或少数几个 PR。有了 Fable，我们看到它能在众多会话之间编排工作。人类工程师负责掌舵，Fable 专注于更高层的设计工作：数据模型、API 规格、算法。随后它指挥多达数十个更小、更具成本效益的模型（如 Opus 或 Sonnet）完成具体任务，例如实际修改文件和运行测试。

The economics work out well. Early results show the orchestrator using frontier tokens for complex, upfront planning before driving a higher percentage of tokens to smaller worker models, without a drop in quality. We run the whole setup on Buzz, our open source collaboration workspace where humans and AI agents work side by side in shared channels and threads. It gives us great visibility into how the team is progressing.

这套账算得过来。早期结果显示，编排器（orchestrator）把前沿模型的 token 用在复杂的前期规划上，随后把更高比例的 token 交给更小的执行模型（worker model），而质量并未下降。整套配置运行在 Buzz 上——这是我们开源的协作工作空间，人类与 AI agent 在共享的频道（channel）和话题（thread）中并肩工作。它让我们能很好地掌握团队进展。

In any given migration, we might merge a thousand pull requests orchestrated by Fable. That lets engineers focus on the hard design decisions instead of the mechanical work.

在任何一次迁移中，我们都可能合并上千个由 Fable 编排的 pull request。这让工程师能专注于困难的设计决策，而不是机械性工作。

# 什么时候你会选择 Claude Fable 模型而不是其他方案？（When do you use Claude Fable models over other options?）

A lot of models have started to close the gap since, and Opus is very good too, but the moment Fable came out, it was a big step change in how large a problem you could take on at once. Teams at Block gravitated to that and started figuring out what it means to move up another level of abstraction, another level of scale.

此后许多模型都开始缩小差距，Opus 也非常出色，但 Fable 一问世就是一次巨大的跃变：你能一次性接手的问题规模完全不同了。Block 的各团队纷纷被它吸引，并开始琢磨再上一个抽象层级、再上一个规模层级意味着什么。

Now, a lot of our conversation is about model efficiency. That's where model orchestration comes in: Fable's frontier intelligence is most valuable when you're tackling a big problem with it, and then we look to smaller models to fill in a lot of the details on code changes. It's literally the first time I can remember since we started using LLMs for code where the answer wasn't just "use the most frontier model 100% of the time."

现在，我们很多讨论都围绕模型效率。这正是模型编排（model orchestration）的用武之地：Fable 的前沿智能在你用它攻克大问题时最有价值，之后我们再借助更小的模型去填补代码改动中的大量细节。自从我们开始把 LLM 用于代码以来，这是我记忆中第一次答案不再是“百分之百始终使用最强的前沿模型”。

Model orchestration is a big part of how we build things today, and it means we need new tools. Some tools are very single-conversation focused, and the interface doesn't keep up with you having 45 agents working with you all the time. That's another place Buzz comes in. We have multi-agent workflows in Buzz, and we really think the future is multiple people talking to many agents, with the organization picking the efficient models for routine tasks and frontier models for bigger problems.

模型编排是今天我们构建方式的重要一环，这也意味着我们需要新工具。有些工具高度聚焦于单次对话，当你同时有 45 个 agent 在与你协作时，它的界面就跟不上了。这又是 Buzz 的用武之地。我们在 Buzz 中有多 agent 工作流（multi-agent workflow），我们真心认为未来是多人同时与众多 agent 对话，由组织为日常任务挑选高效的模型、为更大的问题挑选前沿模型。

# 你交给 Fable 的最难的问题是什么？（What's the hardest problem you've thrown at Fable?）

The first is large-scale migrations: multiple-thousand-PR migrations where Fable orchestrated the whole system that did the work.

第一个是大规模迁移：数千个 PR 量级的迁移，由 Fable 编排完成整个干活的系统。

The second is taking a large codebase, something in the neighborhood of a million lines of code, and making sure it's not growing tech debt every day. We are shipping more PRs than ever before. People are shipping a lot more features and a lot more changes, so our systems accumulate tech debt faster than ever, and we've been missing the corresponding thing that speeds up our ability to maintain them. That's finally happening. We have agents that do daily or weekly passes across a whole codebase asking, "How's it going in here? What do we need to clean up and consolidate?"

第二个是拿下一个大型代码库——大约一百万行代码的量级——并确保它的技术债（tech debt）不会每天都在增长。我们现在合并的 PR 比以往任何时候都多，人们交付的功能和改动也多了很多，因此我们的系统积累技术债的速度前所未有；而我们也一直缺少与之配套、能加快维护能力的手段。这件事终于开始发生了。我们有 agent 会每天或每周对整个代码库做一轮巡检，问：“这里情况怎么样？有哪些需要清理和整合？”

# 在广泛开放访问的前提下，你们如何推动前沿模型的高效使用？（With broad access, how do you drive efficient use of frontier models?）

We give anyone in a technical role full access to the most frontier models, because a big part of our strategy is to always be working at the edge of what's possible.

我们让任何技术岗位的人都能完全使用最前沿的模型，因为我们策略的很大一部分就是始终在可能性的边缘工作。

Using Fable to fix a typo in a README is a very bad use of resources. So we needed better systems for suggesting which model to use for given tasks. To accomplish this, we've been building an auto-selector that helps employees choose which model to use, when. Our multi-agent systems powered by a mix of larger and smaller frontier models help too.

用 Fable 去修 README 里的一个错别字，是对资源极其糟糕的使用。因此我们需要更好的系统来建议特定任务该用哪个模型。为此，我们一直在构建一个自动选择器（auto-selector），帮助员工判断何时该用哪个模型。由更大与更小的前沿模型混合驱动的多 agent 系统对此也有帮助。

A big part of our goal as a company is to operate efficiently by default, and that's the skill we're building now. We've got a lot of great models to choose from. If everyone gets good at picking the right model for the right task, our cost effectiveness goes a lot further without slowing us down.

作为一家公司，我们的目标很大一部分是默认就高效运转，这正是我们当下在培养的能力。我们有很多优秀模型可选。如果每个人都能熟练地为合适的任务挑选合适的模型，我们的成本效益就能提升一大截，而不会拖慢节奏。

# 你们会建议一些具体的优化策略吗，比如 effort level？（Do you advise specific optimization strategies, like effort levels?）

Even as someone who works with LLMs all day, I almost feel like a model at xhigh effort is a different model than the same model at medium. I don't think of effort levels as a spectrum you walk along. Fable on low might just be better than another model on xhigh. So I think of it as a bigger grid we need to optimize against. Every single use case we benchmark, we check not just every model but every effort level of every model.

即便我整天和 LLM 打交道，我也几乎觉得同一个模型在 xhigh effort 下和在 medium 下是不同的模型。我不把 effort level 看作一条可以依次走过的光谱。Fable 在 low 档位下可能就比另一个模型在 xhigh 档位下更好。所以我把它看作一个更大的网格（grid），需要在其上做优化。对我们做基准测试（benchmark）的每一个用例，我们不仅检查每个模型，还检查每个模型的每一个 effort level。

As someone working centrally on how we do AI, I want recommendations that are really clear, such as: "Use this model on high as the default for this kind of task." We have to make the tools do that automatically as much as possible, and then let experts explore when they want to.

作为一个在公司层面负责 AI 做法的人，我希望建议足够明确，比如：“这类任务的默认选择是把这个模型用在 high 档位。”我们必须尽可能让工具自动做到这一点，然后在专家想要探索时留出空间。

# 在当今的前沿模型上，你如何考虑引入防护措施？（How are you thinking about incorporating safeguards with today's frontier models?）

Some things are so critical that you need guarantees, and we're not ready to let LLMs loose on our codebase. We always leave merges to main and deploys to production up to people, along with related changes like feature flags. Agents can make code changes, but those changes have to pass our security checks, and then two humans have to approve the deploy. That dual-approval layer makes it impossible for an LLM to push those kinds of changes on its own.

有些事至关重要，需要的是保证，我们还没准备好让 LLM 在我们的代码库上自由行动。合并到 main 分支、部署到生产环境，以及 feature flag 之类的相关变更，我们始终交由人来完成。Agent 可以修改代码，但这些改动必须通过我们的安全检查，然后还要有两个人批准才能部署。这一双重审批（dual-approval）层让 LLM 无法自行推送这类变更。

At a lower level, it's surprising how many things an employee can do on their laptop if they're not paying attention, things you assume humans won't do because they know not to. That pushes us in interesting directions. I think a lot of the future of work is remote workstations, because they're easier to secure (and they help with other problems LLMs have, like filling up disk space). A big part of our security story is building systems that are secure by default: a network allow list instead of a deny list, and things like that, which stop the worst kinds of problems.

在更低的层面上，如果员工不留神，他们能在自己的笔记本上做出多少事，实在令人意外——那些你本以为人因为知道不该做就不会去做的事。这把我们推向了有意思的方向。我认为未来工作的很大一部分是远程工作站（remote workstation），因为它们更容易加固安全（而且还能缓解 LLM 的另一些麻烦，比如把磁盘空间占满）。我们安全体系的重要一环，是构建默认即安全（secure by default）的系统：用网络允许列表（allow list）而不是拒绝列表（deny list），诸如此类，从而挡住最糟糕的那类问题。

We also try to get as much as possible out of the model loop. A lot of our security guardrails are really cheap, efficient checks that run without the model needing to be involved. We build those into the interfaces people use to do work. Sometimes it's even a regex. Increasingly, smaller classifiers are part of the picture too.

我们还尽量把尽可能多的检查移出模型回路（model loop）。我们的许多安全护栏（guardrail）其实都是非常廉价高效的检查，不需要模型参与就能运行。我们把它们内建到人们干活所用的界面里。有时甚至只是一条正则表达式（regex）。越来越常见的是，更小的分类器（classifier）也被纳入其中。

Then in the day to day, there's a whole class of things you want to enable to make people productive, where blocking isn't worth it. We intentionally let the model open PRs, review code, and query our logging systems and analytical databases, all to help people move faster.

而在日常工作中，还有一整类事情是你希望放开、以提升人们生产力的，对它们加以阻断并不值得。我们有意让模型去开 PR、审阅代码、查询我们的日志系统和分析数据库，都是为了帮助人们更快推进。

That's where the model's built-in safeguards are really critical. By that, I mean the way Fable and other frontier models are tuned to not take part in dangerous activities, plus Anthropic's own safety classifiers. In our testing, even if someone asks Claude to "hack around" our dual-approval system, it refuses to try to bypass it. It's something we benchmark, too.

这正是模型内建防护措施至关重要的地方。我指的是 Fable 及其他前沿模型被调校为不参与危险活动的方式，以及 Anthropic 自己的安全分类器（safety classifier）。在我们的测试中，即便有人让 Claude 去“绕过”我们的双重审批系统，它也会拒绝尝试。这也是我们会做基准测试的一项。

# 对其他使用前沿模型的工程团队，你有什么建议？（Any advice for other engineering teams working with frontier models?）

We have thousands of people working with AI who are pushing the frontier of what it can do. They're learning what works as individuals, and they're improving the systems around them. I've got a bunch of local skills on my laptop that make it possible for me to work in our Square web front end, for example, even though I'm not a front-end developer by trade. Organizational maturity comes when all of this context and organizational knowledge is remembered at a system level, not an individual level.

我们有数千人在使用 AI，不断拓展它能做到的事。他们以个人身份摸索出哪些做法有效，并改进身边的系统。比如，我的笔记本上装了一堆本地 skills，让我能在 Square 的 Web 前端里干活，尽管我并不是科班的前端开发者。当所有这些上下文和组织知识是在系统层面、而不是个人层面被记住时，组织才算真正成熟。

Our goal is to document everything we're learning from people doing the work and put it into a system where everyone benefits, so every time you run an agent, you know it has the best context available to contribute to your work.

我们的目标是，把从一线做事的人身上学到的一切都记录下来，放进一个让所有人受益的系统里；这样每次你运行 agent 时，都能确信它拥有可用的最佳上下文来为你的工作做出贡献。

Get started with Claude Fable 5.1 today.

今天就开始使用 Claude Fable 5.1。

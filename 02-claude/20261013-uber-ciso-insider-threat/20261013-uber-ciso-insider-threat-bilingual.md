# Uber 的 CISO 谈如何为 AI 建立内部威胁项目(中英对照)

> **原文标题:** Uber's CISO on building an insider threat program for AI
> **原文链接:** https://claude.com/resources/articles/uber-s-ciso-on-building-an-insider-threat-program-for-ai
> **原文作者:** Anthropic(页面未署名,分类:Perspectives;受访者:Uber 首席信息安全官 Philip Martin)
> **发布日期:** 2026-10-13
> **翻译模型：** DeepSeek-V4.1-Flash
> **评分:** ★★★☆☆ —— Uber CISO Philip Martin 谈为 AI 建内部威胁项目:agent 监控与可观测性的落地经验
> 排版:每段英文原文在前,中文翻译紧随其后。默认收录正文主体。术语保留英文并附中文释义。

---

Uber runs one of the world's largest real-time marketplaces, with over 40 million trips and delivery orders a day. Philip Martin, Uber's Chief Information Security Officer (CISO), leads the team that protects Uber's data and the riders, drivers, and couriers who use the platform in the real world. His team started working with Claude on threat modeling before Claude Code existed, assembling context by hand. Now, with Claude Fable, Claude gathers that context itself from code, design docs, and ticket history.

Uber 运营着全球最大的实时交易平台之一,每天处理超过 4000 万次行程与配送订单。Uber 首席信息安全官(Chief Information Security Officer,CISO)Philip Martin 领导的团队,负责保护 Uber 的数据,以及在这个平台上真实世界里使用它的乘客(rider)、司机(driver)和配送员(courier)。他的团队早在 Claude Code 出现之前就开始与 Claude 合作进行威胁建模(threat modeling),当时还得靠人工汇集上下文。如今借助 Claude Fable,Claude 能自己从代码、设计文档和工单历史(ticket history)中汇集这些上下文。

**Video:** Embedded interview video — "Uber's CISO, Philip Martin, on building an insider threat program for AI" (video not collected / 视频未收录)

**视频:** 嵌入式访谈视频 ——"Uber's CISO, Philip Martin, on building an insider threat program for AI"(视频未收录)

Martin thinks AI agents need the same kind of insider threat program companies already run for their employees. We spoke with him about that idea, why he thinks so much about accidental misuse, and how Uber's open-source monitoring tools work alongside Enterprise Frontier Safeguards.

Martin 认为,AI 智能体(AI agent)需要一套与企业早已为员工建立的内部威胁项目(insider threat program)同样的机制。我们与他聊了这个想法、他为何如此关注意外误用(accidental misuse),以及 Uber 的开源监控工具如何与 Enterprise Frontier Safeguards 协同工作。

## 前沿模型正在如何改变你们安全团队的工作?(How are frontier models changing your security team's work?)

The really big difference between prior models and today's frontier models is their ability to incorporate additional context over long time horizons, and as a result, generate really specific outputs. When we're doing security reviews or threat modeling for a given service application, we can let the models consume a lot more context about the service, whether that's the code itself, design documents, issues in Jira, or historical context on what the service is supposed to do and what it interacts with.

以往模型与当今前沿模型之间最大的区别,在于后者能够在很长的时间跨度内吸收额外的上下文,并因此生成非常具体的输出。当我们对某个服务应用做安全审查(security review)或威胁建模时,我们可以让模型吸收关于该服务多得多的上下文——无论是代码本身、设计文档、Jira 中的 issue,还是关于该服务应当做什么、与什么交互的历史背景信息。

I see the same thing emerging with teams of agents. I can imagine a world in the not-too-distant future where each service has an agent associated with it that understands that service very well, and those agents can talk to each other, the same way the services might in production, about the threat model: this is my threat model, this is where my data comes from. That team-of-agents effect will take us to the next level of efficacy.

我同样看到这种趋势正出现在智能体团队(teams of agents)上。我可以想象在不久的将来,每个服务都配有一个与之关联、非常了解该服务的智能体,而这些智能体可以像生产环境中的服务那样彼此对话,交流威胁模型:"这是我的威胁模型,这是我的数据来源。"这种"智能体团队"效应将把我们的效能带到下一个层级。

## Uber 与我们的 Enterprise Frontier Safeguards 团队合作。这对 Uber 为什么重要?(Uber worked with our team on Enterprise Frontier Safeguards. Why was this important for Uber?)

One thing Anthropic and Uber have in common is a bone-deep focus on safety, albeit in very different contexts. When Uber thinks about safety, we think about rider safety, protecting personal data. Anthropic wants to prevent the misuse of its frontier models, but Uber needs to protect our customer data, our trade secrets, and any other proprietary information. Enterprise Frontier Safeguards allow us to strike this balance: the safeguards are in place, but our data is not accessible to Anthropic without our permission.

Anthropic 与 Uber 有一个共同点,那就是对安全有着刻入骨髓的重视,尽管各自所处的语境非常不同。Uber 谈安全,想的是乘客安全、保护个人数据;Anthropic 想防止其前沿模型被滥用,而 Uber 需要保护我们的客户数据、商业秘密(trade secret)以及任何其他专有信息(proprietary information)。Enterprise Frontier Safeguards 让我们得以达成这种平衡:安全保障措施到位,但未经我们许可,Anthropic 无法访问我们的数据。

It gives us control of our data and control of the risk mitigation and the actions we decide to take after a safeguard is triggered. I think we share a responsibility to make sure these models are used safely and effectively in the world.

它让我们既能掌控自己的数据,也能掌控风险缓解(risk mitigation)措施,以及在某个安全保障被触发后我们决定采取的行动。我认为我们共同肩负着一份责任:确保这些模型在世界上被安全、有效地使用。

When your team walked us through the design, it was clear that you heard our feedback. You put us in the driver's seat. This allows us to protect our data while still being responsible stewards of the capabilities of these models.

当你们的团队向我们讲解设计方案时,可以明显看出你们听取了我们的反馈。你们让我们坐上了驾驶座,掌握了主动权。这让我们既能保护自己的数据,又能成为这些模型能力的负责任的管理者(steward)。

## Enterprise Frontier Safeguards 与你们已经在 Uber 的 AI 部署上运行的安全监控如何配合?(How does Enterprise Frontier Safeguards fit with the safety monitoring you already run on AI deployments at Uber?)

We've done a ton of work since the very beginning on how we deploy AI safely within Uber. We recently open-sourced a product called AI Detection and Response (ADR), designed to gather real-time telemetry and take real-time action in the agent harnesses we deploy.

从一开始,我们就在"如何在 Uber 内部安全地部署 AI"这件事上做了大量工作。我们最近开源了一个名为 AI Detection and Response(ADR,AI 检测与响应)的产品,旨在在我们部署的智能体运行框架(agent harness)中收集实时遥测数据(telemetry)并采取实时行动。

It complements Anthropic's safeguards, because we now have access to enhanced data to detect when an agent is acting outside its bounds and respond in real time. We have our own view of what's going on, plus output from Anthropic's classifier, and we can integrate both into our overall detection environment, much like a traditional cybersecurity one. It's all about assembling context effectively to reach a decision as fast, and with as much certainty, as possible.

它与 Anthropic 的安全保障形成互补,因为我们现在能获取更丰富的数据,用以检测智能体何时越界行动,并实时做出响应。我们既有自己对现状的观察,也有 Anthropic 分类器(classifier)的输出,可以把两者整合进我们整体的检测环境,就像传统的网络安全检测环境那样。关键在于有效地汇集上下文,以便尽可能快地、尽可能有把握地做出决策。

I think we'll see Enterprise Frontier Safeguards feed into ADR as a data source. Together with our LLM gateway, which builds a perimeter and controls how AI is used within Uber, those make up our overall AI security architecture.

我认为我们会看到 Enterprise Frontier Safeguards 作为一种数据源接入 ADR。再加上我们的 LLM 网关(gateway)——它建立起一道边界(perimeter)、管控 AI 在 Uber 内部的使用方式——这些共同构成了我们整体的 AI 安全架构。

## 在保护 AI 系统方面,你认为防御方下一步应优先聚焦在哪里?(Where do you think defenders should prioritize their focus next when it comes to securing AI systems?)

Number one is real-time detection and intervention: getting as close as we can to the human entering a prompt in a harness, to be able to tell them, "You probably don't want to do that," or "The tool this model is about to call, the place it's about to connect to, is probably not something you want to happen."

第一是实时检测与干预(real-time detection and intervention):尽可能贴近那个正在运行框架里输入提示词(prompt)的人,从而能够告诉他:"你大概不会想这么做",或者"这个模型即将调用的工具、即将连接的地方,大概不是你想要发生的。"

Number two is better understanding emerging patterns. We look at trends in model behavior and detect new behaviors we didn't know were possible. That becomes a cycle. We look at the data, in many cases with the assistance of LLMs, discover these behaviors as they appear, and build safeguards against them back into the real-time platform. Then new behaviors emerge, because constraints create new behaviors, and we find those too.

第二是更好地理解新出现的模式(emerging patterns)。我们观察模型行为的趋势,并检测出我们此前不知道可能存在的新的行为。这就形成了一个循环:我们查看数据(很多时候借助 LLM 的协助),在行为出现时发现它们,再把针对它们的安全保障措施构建回实时平台。接着新的行为又会出现——因为约束会催生新的行为——我们再把这些也找出来。

## 对于那些对前沿模型所需的监控心存犹豫的领导者,你会说些什么?(What would you tell other leaders who are hesitant about the monitoring frontier models require?)

AI adoption is a business imperative. It's coming whether we want it or not, and it's going to bring real benefits to businesses and people around the world. But there's also real potential for harm, and the stakes rise as models get more capable and take on more complex, autonomous work.

采用 AI 是一项业务上的必然要求(business imperative)。无论我们想不想要,它都会到来,并将给世界各地的企业和人们带来实实在在的好处。但同时也存在实实在在的潜在危害,而且随着模型能力更强、承担更复杂、更自主的工作,风险也在上升。

Every company already runs an insider threat program for its people. Nobody argues about that. We accept the sometimes onerous data retention, and we monitor things we'd rather not monitor, because we know humans occasionally act outside the bounds of their role, for any number of reasons.

每家公司都已经在为员工运行一套内部威胁项目。这一点没有人会去争论。我们接受有时颇为繁重的数据留存(data retention)要求,也监控一些我们并不情愿监控的东西,因为我们知道,人类偶尔会因为各种各样的原因做出超出自身职责范围的行为。

Frontier models introduce the same risk. These models may be directed to act outside the constraints we expect them to operate within. So we need the equivalent capability for AI. That insider threat program for models is going to be essential to rolling these systems out safely and securely.

前沿模型带来了同样的风险。这些模型可能被指示去做超出我们预期其运行边界的行为。因此,我们需要为 AI 配备对等的能力。那套面向模型的内部威胁项目,对于安全、稳妥地推广这些系统而言将不可或缺。

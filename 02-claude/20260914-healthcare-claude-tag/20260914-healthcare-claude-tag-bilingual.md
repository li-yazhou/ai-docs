# 医疗机构如何使用 Claude Tag（中英对照）

> 原文标题：How healthcare organizations use Claude Tag
> 原文链接：https://claude.com/blog/how-healthcare-organizations-use-claude-tag
> 原文作者：Anthropic
> 发布日期：2026-09-14
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆—— 医疗行业部署与治理 Claude Tag 的案例合集
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

Healthcare organizations are using Claude Tag (beta), which brings Claude into Slack as a teammate, to help them triage production alerts, maintain internal tools, and answer questions about payer rules. While Claude Tag isn't yet covered by Anthropic's Business Associate Agreement, several healthcare organizations are using it today in channels and with connectors that never touch protected health information (PHI). Mention @Claude in a channel and it reads the thread, uses the tools you've connected, does the work, and reports back. It remembers what happens in each channel, so long-running work never needs re-explaining, and it can watch a channel and step in on its own when you let it.

医疗机构正在使用 Claude Tag（beta 版；一项把 Claude 引入 Slack、可像队友一样 @ 提及的功能）来帮助自己分诊生产环境告警、维护内部工具，并解答支付方规则（payer rules）相关的问题。虽然 Claude Tag 尚未纳入 Anthropic 的商业伙伴协议（Business Associate Agreement, BAA），但如今已有多家医疗机构在使用它——只在不会接触受保护健康信息（PHI）的频道中使用，也只连接不涉及 PHI 的数据源。在频道里 @Claude，它就会阅读整个讨论串，调用你已连接的工具，完成工作并汇报结果。它记得每个频道里发生过什么，因此长期推进的工作无需反复解释；在你允许的情况下，它还能持续关注某个频道，并在需要时主动介入。

Admins decide where Claude Tag works and what it can reach, so healthcare teams can keep Claude Tag out of channels and systems that hold PHI:

管理员可以决定 Claude Tag 在哪里工作、能访问到什么，因此医疗团队可以把 Claude Tag 挡在存有 PHI 的频道和系统之外：

- Claude Tag can be off by default and enabled only in approved channels, with DMs disabled and connectors scoped per channel.
- Claude Tag doesn't read all of Slack – instead, it only sees what a workspace member sees. While it can read the public channels of the workspace and search them by keyword, it doesn't have access to private channels it hasn't been invited to.
- Access bundles let a team connect data sources like its codebase and issue tracker in one channel while the EHR, clinical systems, and patient communications are inaccessible. Learn more about Claude Tag's agent identity access model and its full architecture in the security and data handling docs, and you can read more about best practices for healthcare organizations, here.

- Claude Tag 可以默认关闭，仅在获批的频道中启用，同时禁用私聊（DM），并按频道逐一限定连接器范围。
- Claude Tag 并不会读取整个 Slack——它只能看到工作区成员能看到的内容。虽然它可以读取工作区的公开频道并按关键词搜索，但无法访问未被邀请加入的私有频道。
- 访问权限包（access bundles）让团队可以在一个频道里连接代码库、工单系统等数据源，而电子健康档案（EHR）、临床系统和患者沟通渠道则完全无法访问。想进一步了解 Claude Tag 的代理身份访问模型与完整架构，可参阅安全与数据处理文档；关于医疗机构的最佳实践，可在这里阅读更多。

Enterprise organizations that activate Claude Tag and link it to GitHub receive $25,000 in Claude Tag credit ($2,500 for Team organizations with 10+ seats); note that these credits expire October 1, 2026. See the credit details, here.

激活 Claude Tag 并将其关联到 GitHub 的企业版组织可获得 25,000 美元的 Claude Tag 额度（拥有 10 个及以上席位的 Team 版组织为 2,500 美元）；请注意，这些额度将于 2026 年 10 月 1 日到期。额度详情可在这里查看。

Here's how Insight Health, Tennr, and Medallion are using Claude Tag to build human-agent teams today.

下面来看 Insight Health、Tennr 和 Medallion 如今是如何用 Claude Tag 搭建"人类 + 智能体"协作团队的。

##### 在 Insight Health 运行事件响应（Running incident response at Insight Health）

Insight Health builds MagicDocs, an AI referral coordinator for specialty medical practices. MagicDocs reads inbound patient documents (faxes, referrals, prior-authorization requests, labs, and medical records), extracts the clinical details, matches each document to the right patient, and writes structured data into the EHR. The company serves 1,100+ practices across 56 specialties.

Insight Health 开发的产品是 MagicDocs——一款面向专科医疗诊所的 AI 转诊协调员。MagicDocs 读取传入的患者文件（传真、转诊单、预授权申请、检验报告和病历），提取其中的临床细节，把每份文件匹配到正确的患者，并将结构化数据写入 EHR。该公司服务超过 1,100 家诊所，覆盖 56 个专科。

As Insight Health's customer base has grown, production alert triage has eaten into time its lean engineering team would rather spend on higher-value work. So for the past three months, the company has run Claude Tag in its PHI-free engineering and support channels, where it investigates production alerts, files and de-duplicates tickets, reviews PRs, and carries context from one thread to the next.

随着 Insight Health 客户规模的扩大，生产环境告警的分诊占用了这支精简工程团队本想投入更高价值工作的时间。于是在过去三个月里，该公司在与 PHI 隔离的工程和支持频道中运行 Claude Tag，由它调查生产告警、提交并去重工单、评审 PR，并在不同讨论串之间延续上下文。

In their high-volume production alerts channel, they've paired Claude Tag with a second agent, Zeus (built by Insight Health on the Claude Agent SDK), to take an incident from first notification to a tested fix. The two agents have deliberately different access: Claude Tag sees the codebase and Linear, so it knows the code patterns and ticket history; Zeus runs on the company's BAA-covered Claude API organization, so it can query production data, masking PHI before the data reaches Slack. When an alert arrives, the agents investigate it–querying production data, checking it against the code and recent deploys, and comparing it to past tickets–all while reporting on their progress in the Slack thread. Once they identify the root cause, they open a draft PR and monitor the tests, and finally an engineer reviews and merges. Claude Tag's channel memory is valuable here: it recognizes a new complaint as a known issue with a fix pending, recalls investigations from weeks earlier, and follows standing instructions unprompted.

在他们消息量很大的生产告警频道里，团队将 Claude Tag 与第二个智能体 Zeus（由 Insight Health 基于 Claude Agent SDK 构建）配对，让一次事件从首次通知一路推进到经过测试的修复。两个智能体的权限经过了刻意区分：Claude Tag 能看到代码库和 Linear，因此了解代码模式和历史工单；Zeus 则运行在公司受 BAA 覆盖的 Claude API 组织上，因此可以查询生产数据，并在数据到达 Slack 之前先对 PHI 做脱敏处理。告警一到，两个智能体便展开调查——查询生产数据、对照代码和近期部署进行核对、并与过往工单做比较——同时在 Slack 讨论串中汇报进展。一旦找到根因，它们就会打开一个草稿 PR 并盯住测试运行，最后由工程师评审并合并。Claude Tag 的频道记忆在这里非常关键：它能认出新的投诉其实是某个已有修复在途的已知问题，能回忆起几周前的调查过程，还会无需提醒地遵循既定指令。

"Everyone in the Slack channel sees both agents' reasoning and how they divide the work," said Saran Siva, Insight Health's co-founder and CTO. "That transparency builds trust and teaches the team how to get the most value out of the agents." Since Claude Tag went live, 97% of alerts in Insight Health's critical alert channel have closed without an engineer having to step in, freeing the team to focus on core product work.

"Slack 频道里的每个人都能看到两个智能体的推理过程，以及它们如何分工，" Insight Health 联合创始人兼 CTO Saran Siva 说，"这种透明度建立起了信任，也让团队学会如何从智能体身上获得最大价值。" 自 Claude Tag 上线以来，Insight Health 严重告警频道中 97% 的告警无需工程师介入即可关闭，团队得以专注于核心产品工作。

Beyond incidents, the Insight Health team uses Claude Tag for hiring, vendor negotiation prep, contract reviews (checking terms against call transcripts), and general business operations.

除事件处理之外，Insight Health 团队还用 Claude Tag 做招聘、供应商谈判准备、合同评审（将条款与通话记录逐一核对）以及日常业务运营。

##### 在 Tennr 维护内部工具（Maintaining internal tooling at Tennr）

Tennr is a patient orchestration platform that helps providers get patients into the right care setting faster by automating the intake, documentation, authorization, and scheduling work that otherwise stalls care. Claude Tag lives in its internal tooling, and helps non-technical teams set up and maintain custom internal applications.

Tennr 是一个患者调度编排平台，通过自动化原本会拖慢诊疗进程的接诊、文书、授权和预约工作，帮助医疗服务机构更快地把患者送入合适的诊疗场景。Claude Tag 就驻扎在它的内部工具体系里，帮助非技术团队搭建和维护定制的内部应用。

At fast-growing companies like Tennr, internal tools die as fast as they are built because nobody has the bandwidth to maintain them. In July 2026, Tennr took a different approach as it built internal tools. It launched recruiting.tennr.com, an internal offer presentation portal built in Claude Code, and made Claude Tag its primary maintainer through a dedicated Slack channel. Now the people who actually use the tool (recruiters, People team members, hiring managers, and RevOps) @-mention Claude with requests in plain language English, and Claude ships the code change, deploys it, and reports back. They can iterate on the tool directly, without pulling engineers off product work.

在 Tennr 这类快速扩张的公司里，内部工具的消亡速度和建成的速度一样快，因为没人有精力去维护它们。2026 年 7 月，Tennr 在构建内部工具时换了一种思路：它上线了 recruiting.tennr.com——一个用 Claude Code 构建的内部 offer（录用通知）展示门户——并通过一个专门的 Slack 频道让 Claude Tag 担任它的主要维护者。现在，真正使用这个工具的人（招聘人员、People 团队成员、用人经理和 RevOps）用平实的英文 @ 提及 Claude 提出需求，Claude 便完成代码修改、部署上线并汇报结果。他们可以直接对这个工具进行迭代，而不必把工程师从产品工作中拉出来。

In roughly a month, the team shipped 15+ tickets this way: a benefits deep-dive section built from an uploaded PDF one-pager, a "Sign your offer here" banner linking to Dropbox Sign, target bonus fields, and self-service admin controls. When a publicly exposed copy API was flagged, Claude locked it down the same day. When an Ashby import bug kept resetting equity on live offers, Claude fixed the bug and propagated the corrected values. When a separate bug briefly showed candidates variable comp incorrectly, Claude posted a channel-wide notice with the affected window and remediation without being asked. It also wrote the onboarding documentation and handles permission management for the tool.

大约一个月里，团队用这种方式交付了 15 张以上的工单：根据一份上传的 PDF 单页材料搭建的福利详解版块、链接到 Dropbox Sign 的"在此签署你的 offer"横幅、目标奖金字段，以及自助式管理控制项。当一个暴露在公网上的 copy API 被标记出来时，Claude 当天就把它锁死了。当 Ashby 导入的一个 bug 反复重置已生效 offer 中的股权数据时，Claude 修复了 bug，并把修正后的值同步到了所有相关记录。当另一个 bug 短暂地错误显示了候选人的浮动薪酬时，Claude 无人吩咐就主动在频道里发布了全员公告，说明受影响的时间窗口和补救措施。它还编写了上手文档，并负责这个工具的权限管理。

The team taught Claude its own ops conventions in-channel, including an emoji status legend, and a ticket format with numbered tickets, requester, commit link, screenshots, and a live test link. Claude adopted them going forward. Recruiters were filing what amounted to engineering tickets in natural language, sometimes just a screenshot, and getting production changes back within the hour. "Claude Tag is what makes internal tooling viable at all," said Abe Griffiths, Tennr's VP of Business Operations and Strategy. "With Claude as the steward of the tool via a Slack channel, the people who actually use the tool can iterate on it directly, without pulling engineers off product work."

团队在频道里教会了 Claude 自己的运营规范，包括一套 emoji 状态图例，以及一种工单格式：工单编号、请求人、commit 链接、截图和可用的测试链接。Claude 此后一直沿用这些规范。招聘人员用自然语言——有时只是一张截图——提交的请求其实相当于工程工单，一小时内就能拿到上线后的改动。"Claude Tag 是让内部工具真正可行的关键，" Tennr 业务运营与战略副总裁 Abe Griffiths 说，"由 Claude 通过一个 Slack 频道来当这个工具的管家，真正使用工具的人就能直接对它进行迭代，而不必把工程师从产品工作中拉出来。"

Rollout was quick because the groundwork existed. Tennr only allows PHI in a limited set of private channels, so adding Claude Tag broadly didn't introduce a new data problem. What made Claude Tag easy to say yes to was per-channel scoping.

推广之所以迅速，是因为前期基础早已具备。Tennr 只允许在有限的几个私有频道中出现 PHI，因此大范围引入 Claude Tag 并不会带来新的数据问题。而真正让 Claude Tag 易于获批的，是按频道划分的权限范围（per-channel scoping）。

"In the offer tool channel, we want Claude to be proactive: fix problems, modify code, skip PR reviews, and ship," Griffiths said. "That posture would obviously be wrong for, say, a Product or Eng channel where Claude's role is closer to info gathering or keeping us organized. Being able to draw those lines channel by channel meant we could give Claude real autonomy where it's low risk without granting it everywhere."

"在 offer 工具频道里，我们希望 Claude 主动出击：修复问题、修改代码、跳过 PR 评审、直接上线，" Griffiths 说，"这种姿态如果放在比如产品或工程频道里显然就不合适了——在那里 Claude 的角色更接近信息收集，或者帮我们把事情理得井井有条。能够逐频道划出这些界线，意味着我们可以在低风险的地方给 Claude 真正的自主权，而不必到处放权。"

##### 在 Medallion 积累支付方专业知识（Accumulating payer expertise at Medallion）

Medallion automates provider credentialing, licensing, and payer enrollment for healthcare organizations. Much of that work depends on arcane, undocumented rules: which payers require a minimum number of credentialed providers before a group can enroll, state-specific regulations, and dozens of similar questions engineers need answered before they can codify a process into the product. Historically, those answers lived with a small group of in-house healthcare domain experts, creating an ongoing human bottleneck for core product development decisions.

Medallion 为医疗机构自动化处理医疗从业者资质认证、执业许可办理和支付方注册。这些工作很大程度上依赖一些晦涩且未成文的规则：哪些支付方要求一个团体在注册前必须拥有最低数量的已认证从业者、各州特有的监管规定，以及几十个类似的问题——工程师必须先弄清这些，才能把流程固化到产品里。过去，这些答案只掌握在少数几位公司内部的医疗领域专家手中，成为核心产品开发决策上持续存在的人力瓶颈。

Claude Tag now sits in that loop and breaks the knowledge silo. When an engineer asks a payer-rules question in Slack, Claude Tag answers from past expert responses, historical data, and unstructured internal resources. The expertise accumulates in the channel instead of in one person's head.

如今，Claude Tag 进入了这个环节，打破了知识孤岛。当工程师在 Slack 中提出支付方规则方面的问题时，Claude Tag 会基于过往的专家答复、历史数据和非结构化的内部资源来作答。专业知识在频道中不断积累，而不是留在某个人的脑子里。

"When it isn't confident, it tags in the right expert, and their response becomes the basis for future answers on related topics," said CTO Armaan Sarkar.

"当它没有把握时，就会把对应的专家拉进来，而专家的回复会成为今后回答相关主题的依据，" CTO Armaan Sarkar 说。

What made them comfortable rolling out Claude Tag? Sarkar points to picking the right PHI-free workstreams, expert review, and automated validation. Claude Tag operates at the policy level: the questions are about payer rules and process, not individual patients or providers. It doesn't answer alone: experts provide oversight and corrections, and every exchange happens in a Slack channel anyone at the company can audit. And the outputs get checked downstream, since Medallion's systems use these policies to take actions that are audited and validated on their own.

是什么让他们能放心地推广 Claude Tag？Sarkar 归因于三点：选对无 PHI 的工作流、专家复核，以及自动化校验。Claude Tag 工作在政策层面：问题都关于支付方规则和流程，而不涉及具体的患者或从业者。它不会独自作答：专家提供监督和纠正，每一次交互都发生在公司里任何人都可以审计的 Slack 频道中。而且输出结果在下游还会被再次检查，因为 Medallion 的系统会用这些政策去执行那些本身就经过审计和验证的操作。

##### 上手指南（Getting started）

The teams above all started in the same place: engineering, product, ops, or recruiting channels, with one or two connectors, working in open threads rather than DMs so the whole team could review and pick up context.

上面这些团队的起步方式都一样：在工程、产品、运营或招聘频道中，先连上一两个连接器，在公开讨论串而非私聊中工作，让整个团队都能查看并接续上下文。

Review the Claude Tag best practices for healthcare organizations, here, then start with one channel. Turn Claude Tag on for an alert or support-engineering channel, connect GitHub, and let the team work with it for two weeks before widening the allowlist.

先在这里查阅面向医疗机构的 Claude Tag 最佳实践，然后从一个频道开始。在某个告警或支持工程频道中启用 Claude Tag，连接 GitHub，让团队与它协作两周，之后再扩大白名单范围。

Get started with Claude Tag.

立即上手 Claude Tag。

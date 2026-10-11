# 把 Claude Mythos 5 的网络安全能力带给更多防守者（中英对照）

> 原文标题：Bringing the cybersecurity capabilities of Claude Mythos 5 to more defenders
> 原文链接：https://claude.com/blog/bringing-claude-mythos-5-to-more-defenders
> 原文作者：Anthropic
> 发布日期：2026-08-21
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，值得一看）--Project Glasswing 扩围与前沿模型安全部署进展
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

*We're sharing an update on our efforts to help more teams use frontier capabilities for cyber defense.*[*Claude Mythos 5*](https://www.anthropic.com/news/claude-fable-5-mythos-5)*is now available in*[*Claude Security*](https://claude.com/product/claude-security)*, and coming soon to partners' cyber defense tools. We're also launching a $35M fund to help secure open-source software and sharing plans to expand our*[*Cyber Verification Program*](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet)*.*

*我们在此分享一项进展：帮助更多团队使用前沿能力进行网络防御。[Claude Mythos 5](https://www.anthropic.com/news/claude-fable-5-mythos-5) 现已在 [Claude Security](https://claude.com/product/claude-security) 中可用，并将很快进入合作伙伴的网络防御工具。我们还将设立一支 3,500 万美元的基金来帮助保护开源软件的安全，并分享扩展 [Cyber Verification Program（网络验证计划）](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet)的计划。*

In April, we launched [Project Glasswing](https://www.anthropic.com/glasswing) to put our most capable frontier model, Claude Mythos Preview (and its successor, Claude Mythos 5), in the hands of a small group of organizations securing the world’s most critical software. This gave defenders a window of time to find and fix vulnerabilities ahead of models with similar capabilities becoming generally available or reaching malicious actors.

今年 4 月，我们启动了 [Project Glasswing](https://www.anthropic.com/glasswing)，把我们最强大的前沿模型 Claude Mythos Preview（及其后继者 Claude Mythos 5）交到一小群保护全球最关键软件的组织手中。这给了防守者一段时间窗口，在这些具备类似能力的模型普遍可用或落入恶意行为者手中之前，先找到并修复漏洞。

Our goal has always been to expand Mythos-level defense to as many defenders as we safely can. To do that, we've been working on [safety classifiers](https://www.anthropic.com/research/next-generation-constitutional-classifiers) and safeguards that let us expand access to Mythos-class models without putting their offensive cyber capabilities in the wrong hands. [Claude Fable 5](https://www.anthropic.com/news/claude-fable-5-mythos-5) was the first step: it made the model broadly available while blocking dual-use cyber work.

我们的目标始终是：在力所能及的安全范围内，把 Mythos 级别的防御能力扩展给尽可能多的防守者。为此，我们一直在开发[安全分类器](https://www.anthropic.com/research/next-generation-constitutional-classifiers)（safety classifiers）与保障措施（safeguards），使我们能够在不让 Mythos 级模型的进攻性网络能力落入不法之手的前提下扩大访问。[Claude Fable 5](https://www.anthropic.com/news/claude-fable-5-mythos-5) 是第一步：它让模型广泛可用，同时屏蔽两用（dual-use）网络工作。

Today, we’re taking the next steps. The riskiest behavior occurs when a user has direct access to a model, where a malicious actor can try to steer it toward harmful uses. But if users can only receive specific outputs, such as a patch for a vulnerability or a security alert, that risk is much lower. The changes we’re announcing give users greater access to the defensive results, while maintaining appropriate guardrails around direct access to the model:

今天，我们迈出下一步。最危险的行为发生在用户能够直接访问模型的时候，恶意行为者可以试图把模型引向有害用途。但如果用户只能接收特定的输出，比如一个漏洞补丁或一条安全警报，风险就要低得多。我们此次宣布的调整，让用户能更大程度地获取防御性成果，同时围绕对模型的直接访问维持适当的护栏：

- Claude Mythos 5 integration into the tools defenders rely on.  We’re working with our cybersecurity technology and services partners to integrate Claude Mythos 5 into the products and services defenders already use to secure their software.

- Claude Security scans can now run on Claude Mythos 5.  Customers on Claude Enterprise plans can now run our most capable model in Claude Security, using it to scan their codebases for security vulnerabilities and suggest patches.

- $35 million in credits for open-source security.  Our new Defender Advantage Fund (0xDAF) will provide $35 million in credits to organizations working to patch vulnerabilities in open-source projects, automate parts of the process of scanning and patching open-source software, and experiment with new security approaches.

- Expanding our Cyber Verification Program.  The program already gives vetted defenders reduced safeguards on Opus and Sonnet models. In the coming weeks, we will expand this program to include broader dual-use capabilities on Opus and Sonnet, with Mythos-class access to follow.

- Claude Mythos 5 集成到防守者赖以工作的工具中。我们正在与网络安全技术与服务合作伙伴合作，把 Claude Mythos 5 集成进防守者已经在用来保护其软件的产品与服务中。

- Claude Security 扫描现可在 Claude Mythos 5 上运行。Claude Enterprise 计划的客户现在可以在 Claude Security 中运行我们最强大的模型，用它扫描代码库中的安全漏洞并提出补丁建议。

- 3,500 万美元的开源安全额度。我们新设立的 Defender Advantage Fund（0xDAF）将向致力于修补开源项目漏洞、自动化开源软件扫描与修补流程的部分环节、并尝试新安全方法的组织提供 3,500 万美元的额度。

- 扩展我们的 Cyber Verification Program。该计划已向经过审查的防守者提供 Opus 和 Sonnet 模型上的减免护栏（reduced safeguards）。未来几周内，我们将扩展该计划，纳入 Opus 和 Sonnet 上更广泛的两用能力，随后开放 Mythos 级别的访问。

Our aim remains to help organizations adapt to the pace and demands of cybersecurity as AI models become increasingly powerful. We will continue to develop safeguards, access programs, and community support to make our most capable models safely available to a wide range of people and organizations.

我们的目标不变：在 AI 模型日益强大的同时，帮助组织适应网络安全的节奏与要求。我们将继续开发保障措施、访问计划与社区支持，让我们最强大的模型能够安全地提供给广泛的人群与组织。

## 将 Mythos 集成到现有的网络防御工具中（Integrating Mythos into existing cyberdefensive tools）

The teams defending hospitals, utilities, financial systems, and the software supply chain already rely on a suite of products and services for security operations, incident response, threat intelligence, and detection engineering. The fastest way to make frontier capabilities available to those defenders is to integrate Mythos-class models into the tools they already run.

保护医院、公用事业、金融系统和软件供应链的团队，早已依赖一整套产品与服务来进行安全运营（security operations）、事件响应、威胁情报和检测工程（detection engineering）。让前沿能力触达这些防守者的最快方式，就是把 Mythos 级模型集成进他们已经在运行的工具中。

Many of our partners have already [built cyber products on Claude Opus](https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity) that help security teams triage alerts, identify threats, and remediate vulnerabilities faster. We’re now working with these partners and more to build Claude Mythos 5 into their products and services, so they can deliver Mythos-level defensive outcomes to their customers.

我们的许多合作伙伴已经[基于 Claude Opus 构建了网络产品](https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity)，帮助安全团队更快地分诊警报、识别威胁、修复漏洞。我们现在正与这些合作伙伴以及更多伙伴合作，把 Claude Mythos 5 构建进他们的产品与服务，让他们能够向自己的客户交付 Mythos 级别的防御成果。

When an end user uses one of these products, they’re not interacting with Mythos directly. Instead, they work through a purpose-built interface that runs Mythos in the background for a defined task and only receive the specific artifact the product is intended to provide. For example, a tool to remediate vulnerabilities might provide a list of suggested patches as its output. This output would be generated by Mythos, but the user would not have a way to prompt the model to, say, develop an exploit for a vulnerability. We and our partners also have abuse prevention measures in place to verify the model stays within its intended scope.

当最终用户使用这类产品时，他们并不直接与 Mythos 交互。而是通过一个为特定目的构建的界面工作：该界面在后台为某个明确的任务运行 Mythos，用户只会收到该产品旨在提供的特定产物（artifact）。例如，一个修复漏洞的工具可能以建议补丁列表作为输出。这个输出由 Mythos 生成，但用户没有办法提示模型去做诸如为某个漏洞开发漏洞利用代码之类的事情。我们和合作伙伴还部署了防滥用措施，以验证模型始终停留在其预定范围之内。

We're early in this work and expect it to expand over time. If you build security products or services and want to bring Claude Mythos 5 to your customers, you can [register your interest here](https://claude.com/form/mythos-cyber-partner).

这项工作尚处早期，我们预计它会随时间扩展。如果你构建安全产品或服务，并希望把 Claude Mythos 5 带给你的客户，可以[在此登记意向](https://claude.com/form/mythos-cyber-partner)。

## 面向 Enterprise 客户，Claude Security 现可搭配 Claude Mythos 5 使用（Making Claude Security available with Claude Mythos 5 for Enterprise customers）

Starting today, [Claude Security](https://claude.com/product/claude-security) scans now run on Claude Mythos 5. Claude Security scans codebases for vulnerabilities and suggests patches for human review; it’s currently in public beta for Claude Enterprise customers, and scans with Mythos 5 are billed as standard token usage under your existing plan, with no separate add-on.

从今天起，[Claude Security](https://claude.com/product/claude-security) 的扫描现在运行在 Claude Mythos 5 上。Claude Security 扫描代码库中的漏洞并给出供人审阅的补丁建议；它目前面向 Claude Enterprise 客户处于公开测试（public beta）阶段，使用 Mythos 5 的扫描按你现有计划下的标准 token 用量计费，无需单独的附加组件。

Enterprise admins can enable Claude Security in the [admin console](http://claude.ai/admin-settings/claude-code). From [claude.ai/security](http://claude.ai/security), users can select a repository to scan using Claude Mythos 5. Claude then scans the codebase for vulnerabilities, and returns each finding with a [CWE](https://cwe.mitre.org/) (Common Weakness Enumeration) category, confidence and severity ratings, and a suggested fix.

Enterprise 管理员可以在[管理控制台](http://claude.ai/admin-settings/claude-code)中启用 Claude Security。在 [claude.ai/security](http://claude.ai/security)，用户可以选择一个代码库用 Claude Mythos 5 进行扫描。Claude 随后扫描代码库中的漏洞，并为每项发现返回 [CWE](https://cwe.mitre.org/)（Common Weakness Enumeration，通用缺陷枚举）类别、置信度与严重性评级，以及建议的修复方案。

Users can then open Claude Code on the web to implement the fix. Interactive patching uses the models your organization has access to in Claude Code. The Mythos scan itself does not extend Mythos access to other surfaces. Every patch must be reviewed and approved by a human before it can be implemented.

用户随后可以打开网页版 Claude Code 来实施修复。交互式打补丁使用的是你的组织在 Claude Code 中有权访问的模型。Mythos 扫描本身不会把 Mythos 的访问扩展到其他界面。每个补丁都必须经过人工审阅和批准才能实施。

Claude Security uses Mythos 5 to scan code you own, and returns detailed findings rather than raw outputs without exposing the model itself. This means defenders can access the capabilities of Claude Mythos 5 without the model becoming accessible to those who might misuse it.

Claude Security 用 Mythos 5 扫描你拥有的代码，返回的是详细的发现结果而非原始输出，并且不会暴露模型本身。这意味着防守者可以使用 Claude Mythos 5 的能力，而模型本身不会被可能滥用它的人接触到。

For more about Claude Security, see our [guide to getting started](https://claude.com/resources/tutorials/getting-started-with-claude-security).

想了解更多关于 Claude Security 的信息，请参阅我们的[入门指南](https://claude.com/resources/tutorials/getting-started-with-claude-security)。

## 启动 Defender Advantage Fund，保护开源软件安全（Launching the Defender Advantage Fund to secure open-source software）

Some of the world’s most widely used programs run on open-source software. Yet these projects are often maintained by volunteers or nonprofit foundations, who may lack the resources or personnel to comprehensively defend their projects against attack. Through Project Glasswing, we made $4M in direct donations to open-source security organizations, provided credits to the open-source security foundations in the program, helped scan and patch widely used projects, and support coordinated vulnerability-fixing efforts like [Akrites](https://akrites.org/) and [Gold Eagle](https://www.whitehouse.gov/releases/2026/07/white-house-launches-gold-eagle-initiative-for-unprecedented-cybersecurity-vulnerability-coordination/).

世界上一些使用最广泛的程序运行在开源软件之上。然而这些项目往往由志愿者或非营利基金会维护，他们可能缺乏资源或人手来全面防御其项目免受攻击。通过 Project Glasswing，我们向开源安全组织直接捐赠了 400 万美元，向该计划中的开源安全基金会提供额度，帮助扫描和修补被广泛使用的项目，并支持 [Akrites](https://akrites.org/) 和 [Gold Eagle](https://www.whitehouse.gov/releases/2026/07/white-house-launches-gold-eagle-initiative-for-unprecedented-cybersecurity-vulnerability-coordination/) 这类协同漏洞修复行动。

Our new Defender Advantage Fund (0xDAF) builds on that work with $35 million in Claude credits for organizations helping open-source maintainers secure their software. Grants will focus on three areas: patching live vulnerabilities in widely used projects, automating scanning and patching in ways other projects can replicate, and helping projects pursue more ambitious security approaches that make them resistant to whole classes of attack.

我们新设立的 Defender Advantage Fund（0xDAF）在这一工作基础上，再投入 3,500 万美元的 Claude 额度，资助那些帮助开源维护者保护其软件安全的组织。资助将聚焦三个方向：修补被广泛使用项目中的现存漏洞、以其他项目可以复制的方式自动化扫描与修补，以及帮助项目采取更有雄心的安全方法，使其能够抵御整类攻击。

We're starting with a small number of larger, pilot grants to learn what works and scales best. We will share details on initial recipients in the coming weeks.

我们将从少量较大规模的试点资助开始，以了解什么最有效、最可扩展。我们将在未来几周公布首批受助者的详情。

## 扩展我们的 Cyber Verification Program（Expanding our Cyber Verification Program）

To date, our Cyber Verification Program has provided organizations with access to dual-use capabilities when using Claude Opus and Sonnet models. Organizations in the program experience reduced safeguards, minimizing interruptions for accepted teams doing legitimate cybersecurity work on systems they’re authorized to protect.

迄今为止，我们的 Cyber Verification Program 已向组织提供在使用 Claude Opus 和 Sonnet 模型时访问两用能力的途径。该计划中的组织体验到的是减免后的护栏，最大限度地减少对获准团队在其被授权保护的系统上开展合法网络安全工作的干扰。

Over the coming weeks, we are evolving the program to expand safeguarded access to Claude Mythos. As part of this, access to defensive capabilities like vulnerability triaging and validation will expand to Mythos-class models, and cyber defenders will see reduced blocks on Claude Opus and Sonnet-class models. Additionally, we are continuing to expand access to Claude Mythos through Project Glasswing in collaboration with our partners in the U.S. Government, focused on protectors of critically important infrastructure that meet strict security control requirements.

未来几周，我们将演进该计划，扩展到 Claude Mythos 的带护栏访问。作为其中的一部分，漏洞分诊与验证等防御能力的访问将扩展到 Mythos 级模型，网络防守者在 Claude Opus 和 Sonnet 级模型上遇到的拦截也会减少。此外，我们正继续与美国政府的合作伙伴协作，通过 Project Glasswing 扩大对 Claude Mythos 的访问，重点面向满足严格安全控制要求、保护至关重要基础设施的机构。

We'll share more details about the Cyber Verification Program expansion in the coming weeks. In the meantime, we encourage all security teams performing legitimate cybersecurity work to apply for the program for reduced safeguards on Claude Opus and Sonnet models. If you are already enrolled and accepted, no action is needed; we’ll reach out with updates.

我们将在未来几周分享关于 Cyber Verification Program 扩展的更多细节。与此同时，我们鼓励所有开展合法网络安全工作的安全团队申请该计划，以在 Claude Opus 和 Sonnet 模型上获得减免的护栏。如果你已报名并被接纳，则无需任何操作；我们会主动联系并提供更新。

## 下一步（What’s next）

These initiatives are a continuation of our efforts to make the defensive capabilities of frontier models available to more people and organizations, and to support the open-source community in hardening their projects against attack. We will continue to work with government partners, organizations, open-source maintainers, and the broader industry to build the resilient cyber infrastructure today’s highly capable AI models demand.

这些举措是我们既有努力的延续：让前沿模型的防御能力惠及更多人与组织，并支持开源社区加固其项目以抵御攻击。我们将继续与政府合作伙伴、组织、开源维护者以及更广泛的行业合作，建设当今高度强大的 AI 模型所需要的弹性网络基础设施。

- Apply for the  Cyber Verification Program .

- Register your interest  in building cyber products and offerings with Mythos.

- Claude Security is available in public beta for Enterprise customers. Admins can enable Claude Security in the  admin console . For a full walkthrough, see our  guide to getting started .

- 申请 Cyber Verification Program。

- 登记构建基于 Mythos 的网络产品与服务的意向。

- Claude Security 面向 Enterprise 客户处于公开测试阶段。管理员可以在管理控制台中启用 Claude Security。完整操作演示请参阅我们的入门指南。

# 与 NVIDIA 合作,让企业对自己的 AI 智能体拥有更多控制权(中英对照)

> **原文标题:** Giving companies more control over their AI agents, with NVIDIA
> **原文链接:** https://claude.com/blog/giving-companies-more-control-over-their-ai-agents-with-nvidia
> **原文作者:** Anthropic(页面未署名,分类:Agents / Product)
> **发布日期:** 2026-09-28
> **翻译模型:** GLM-5.3-Flash
> **评分:** ★★★☆☆ —— 与 NVIDIA 合作的企业级智能体控制与治理公告:凭据保管、策略强制执行、可证明的权限边界等主题重要,但属合作公告、技术细节有限
> 排版:每段英文原文在前,中文翻译紧随其后。默认收录正文主体。术语保留英文并附中文释义。

---

NVIDIA today announced the Open Agent Safety Platform, an open software platform and reference system design for strengthening AI security. Anthropic has collaborated with NVIDIA to bring additional layers of security and control to the agent stack.

NVIDIA 今日发布了 Open Agent Safety Platform(开放智能体安全平台),一个用于强化 AI 安全的开放软件平台与参考系统设计(reference system design)。Anthropic 与 NVIDIA 合作,为智能体技术栈(agent stack)带来额外的安全与控制层。

Claude Managed Agents, a suite of composable APIs for building and deploying production-grade agents at scale, holds the credentials an agent needs in a vault so the agent never sees them. Open source NVIDIA OpenShell software is designed to control what the agent can execute and reach while it works. Customers who are using Managed Agents with OpenShell can limit what an agent can do, review what the agent did and confirm that the limits are in place.

Claude Managed Agents 是一套用于大规模构建和部署生产级智能体的可组合 API(composable API),它把智能体所需的凭据(credential)保存在一个保险库(vault)中,使智能体自身永远看不到它们。开源的 NVIDIA OpenShell 软件则用于控制智能体在工作时能执行什么、能触达什么。同时使用 Managed Agents 与 OpenShell 的客户可以限制智能体能做什么、审查智能体做了什么,并确认这些限制确实到位。

Companies are moving from using AI to answer questions to deploying agents that handle complex work across business units, use proprietary data and take actions on behalf of users. As models improve, agents find more uses and get more access. The more access an agent has, the more its company needs to control and check what it does.

企业正从"用 AI 回答问题"走向部署智能体:让它们处理跨越业务部门的复杂工作、使用专有数据、并代表用户采取行动。随着模型不断改进,智能体的用途越来越多、获得的访问权限也越来越大。智能体拥有的访问权限越多,企业就越需要管控并核查它的所作所为。

## 分层防护(Protection in layers)

Protection starts with safeguards inside the model. Managed Agents and NVIDIA Open Shell add limits that sit outside the model and apply to what the agent does. Each layer is designed to enforce its limits independently, so protection doesn't depend on any single layer. The layers are modular, so companies can adopt the ones that fit their setup.

防护始于模型内部的安全保障。Managed Agents 与 NVIDIA Open Shell 则在模型之外加上限制,作用于智能体的行为。每一层都被设计为独立执行自己的限制,因此防护不依赖于任何单一层。各层是模块化的,企业可以只采用适合自身架构的那几层。

## Claude Managed Agents 负责完成工作并保管凭据(Claude Managed Agents does the work and holds the credentials)

With Managed Agents, the agent loop runs on a separate server from the sandbox, the isolated environment where the work happens. Credentials, meaning passwords and access keys, are held in a separate vault, so the agent never sees them.

在 Managed Agents 中,智能体循环(agent loop)运行在与沙箱(sandbox)分开的服务器上,沙箱是工作实际发生的隔离环境。凭据——即密码和访问密钥(access key)——被保存在一个独立的保险库中,智能体永远看不到它们。

Managed Agents also provides audit trails, which record what each agent did, and integration with a company's existing access controls. Companies can bring their own sandbox setup and choose where and how it runs.

Managed Agents 还提供审计轨迹(audit trail),记录每个智能体做了什么,并可与企业现有的访问控制集成。企业可以自带沙箱配置,自行选择沙箱在哪里运行、如何运行。

## NVIDIA OpenShell 设定智能体可触达的范围(NVIDIA OpenShell sets what an agent can reach)

OpenShell is open source secure runtime software from NVIDIA. It governs and monitors all AI agent behavior and enforces policies for every action. OpenShell blocks everything unless a rule allows it. It checks each tool an agent tries to use and applies rules to the files, network connections and data the agent accesses. The rules are enforced outside the agent, and OpenShell logs every decision it allows or blocks.

OpenShell 是 NVIDIA 的开源安全运行时(secure runtime)软件。它治理并监控 AI 智能体的所有行为,对每一个动作执行策略(policy enforcement)。OpenShell 默认阻断一切,除非某条规则明确允许。它会检查智能体尝试使用的每个工具,并对智能体访问的文件、网络连接和数据施加规则。规则在智能体之外强制执行,OpenShell 会记录它允许或阻断的每一个决定。

Teams can start with narrow permissions, review the log, and use Claude to tighten the rules toward the least access a task needs. OpenShell's policy prover then uses mathematical proof to confirm what the agent can reach under the rules the team wrote.

团队可以从小范围权限起步,查看日志,并借助 Claude 把规则逐步收紧到任务所需的最小访问(least access)。随后,OpenShell 的策略证明器(policy prover)会用数学证明来确认:在团队编写的规则之下,智能体究竟能触达什么。

## Claude Managed Agents 包含哪些能力(What Claude Managed Agents includes)

- Production-grade agents with secure sandboxing, authentication, and tool execution handled for you.
- Long-running sessions that operate autonomously for hours, with progress and outputs that persist even through disconnections.
- Multi-agent orchestration, where agents can spin up and direct other agents to parallelize complex work.
- Trusted governance, giving agents access to real systems with scoped permissions, identity management, and execution tracing built in.

- 生产级智能体:安全沙箱、身份认证(authentication)和工具执行都为你处理妥当。
- 长时间运行的会话:可自主运行数小时,进度与输出即使在中断连接后也会保留。
- 多智能体编排(multi-agent orchestration):智能体可以启动并调度其他智能体,把复杂工作并行化。
- 可信治理(trusted governance):让智能体以限定范围的权限(scoped permissions)访问真实系统,并内置身份管理与执行追踪(execution tracing)。

## 团队如何使用 Managed Agents(How teams use Managed Agents)

- Notion lets teams hand work to Claude inside their workspace. Engineers use it to ship code, and other employees use it to produce websites and presentations. Dozens of tasks can run in parallel while the team works on the results together.
- Rakuten runs specialist agents across engineering, product, sales, marketing, and finance, each deployed within a week.
- Asana built AI Teammates, agents that work alongside people in Asana projects, take on tasks and draft deliverables. Using Managed Agents, the team added advanced features faster than it could have otherwise.

- Notion 让团队在工作区内部把工作交给 Claude。工程师用它发布代码,其他员工用它制作网站和演示文稿。几十个任务可以并行运行,团队则共同处理这些结果。
- Rakuten(乐天)在工程、产品、销售、市场和财务各部门运行专家智能体,每一个都在一周之内完成部署。
- Asana 构建了 AI Teammates——在 Asana 项目中与人并肩工作的智能体,它们承接任务、起草交付物。借助 Managed Agents,团队以比原本更快的速度添加了高级功能。

## 可用性(Availability)

Managed Agents is available today. It can operate in a sandbox you control either running on your own infrastructure, or with a managed provider. NVIDIA OpenShell is open source under the Apache 2.0 license and available on GitHub and NVIDIA's developer resources page.

Managed Agents 现已可用。它可以在由你掌控的沙箱中运行:既可以跑在你自己的基础设施上,也可以使用托管服务提供商。NVIDIA OpenShell 以 Apache 2.0 许可证开源,可在 GitHub 与 NVIDIA 的开发者资源页面获取。

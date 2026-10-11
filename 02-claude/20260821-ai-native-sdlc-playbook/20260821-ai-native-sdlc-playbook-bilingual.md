# AI 原生 SDLC 实战手册（中英对照）

> 原文标题：The AI-Native SDLC playbook
> 原文链接：https://claude.com/blog/the-ai-native-sdlc-playbook
> 原文作者：Louis Claxton
> 发布日期：2026-08-21
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）--官方六阶段 AI 原生 SDLC 全景手册，方法论成体系、可直接落地
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

## 代码不再是瓶颈（Code is no longer the bottleneck）

Organizations have started using AI to write code at a speed unthinkable one year ago, yet the processes around the code haven't changed at the same pace.

各组织已经开始用 AI 以一年前难以想象的速度编写代码，然而围绕代码的流程却没有以同样的速度演进。

Many engineering teams still have the same approval gates, reviews, handoffs, and policies, stalling productivity gains made by using agentic coding solutions like [Claude Code](https://claude.com/product/claude-code).

许多工程团队仍然沿用同样的审批关卡、评审、交接和制度，使得使用 [Claude Code](https://claude.com/product/claude-code) 这类代理式编码（agentic coding）方案本可带来的生产力提升被卡住。

The software development lifecycle (SDLC) is the process that takes software from idea to production. Most organizations run some version of the same six stages, covering planning, design, building, testing, deploying, and maintaining software. Traditionally, each stage is a discrete phase owned by a different role. Product managers write requirements, technical architects turn them into designs, engineers build the designs, QA teams at regulated enterprises verify it, releases teams ship it, and operations monitors what is running. Work moves between the phases through documents, tickets, and sign-offs.

软件开发生命周期（SDLC）是把软件从想法带到生产环境的过程。大多数组织运行的都是同样六个阶段的某种版本，涵盖软件的规划、设计、构建、测试、部署与维护。传统上，每个阶段是由不同角色负责的离散环节：产品经理撰写需求，技术架构师把需求变成设计，工程师实现设计，受监管企业的 QA 团队进行验证，发布团队负责上线，运维团队监控运行中的系统。工作在各个阶段之间通过文档、工单和签核流转。

The traditional software development lifecycle (SDLC) is process-heavy to ensure accountability and control at each step. However, the traditional SDLC was designed to maximize efficiency in an era where the most time-consuming and expensive stage was writing and implementing code, which is no longer the case. PRDs, estimation rituals, and product security reviews all existed to force alignment during what could be weeks, months, or quarters of development work.

传统软件开发生命周期（SDLC）流程繁重，为的是确保每一步都有问责与控制。然而，传统 SDLC 是在一个以撰写和实现代码为最耗时、最昂贵阶段的年代里，为最大化效率而设计的，如今情况已不再如此。PRD、估算仪式和产品安全评审，都是为了在可能长达数周、数月或数个季度的开发工作中强制达成一致而存在的。

The traditional SDLC also features controls that assume every step is performed by humans. The organizations generating the most value have rebuilt their process around what agentic AI can now do, while ensuring that humans stay in the loop. In this guide, we walk through several of our Applied AI team's best practices for integrating Claude internally across each stage of the SDLC to accelerate development and make processes run faster, inspired by working with our customers.

传统 SDLC 的种种控制还建立在每个步骤都由人类执行这一假设之上。创造价值最多的组织已经围绕代理式 AI（agentic AI）现在能做到的事情重建了流程，同时确保人类始终在环（in the loop）。在本指南中，我们将结合与客户合作的实践经验，介绍我们的 Applied AI 团队在 SDLC 各阶段内部集成 Claude 以加速开发、让流程跑得更快的若干最佳实践。

When code is no longer the bottleneck and the build phase runs faster than the traditional SDLC allows for, three things become true:

当代码不再是瓶颈、构建阶段的运行速度超过传统 SDLC 所允许的速度时，有三件事会成为现实：

- The bottleneck moves to the steps to the left and right of the build phase. This is mainly plan, review/test, and deploy, which still run at human speed.

- 瓶颈转移到构建阶段左右两侧的环节，主要是规划（plan）、评审/测试（review/test）和部署（deploy），它们仍以人类速度运行。

- The controls stop matching reality and become intractable. Reviewing each line by hand made sense when a person had written it, but it can't keep up once agents write most of the diff.

- 控制措施不再符合现实，变得难以执行。逐行人工审查在代码由人编写时是合理的，但一旦 diff 的大部分由代理写出，它就跟不上了。

- Governance costs increase because exceptions still route through meetings and committees that meet weekly or monthly.

- 治理成本上升，因为例外情况仍要经过每周或每月才开一次的会议和委员会。

![](images/img-01.png)

**Caption:** Build is no longer the constraint — the human-speed steps around it are. Human-speed stages keep their length while build collapses to hours.

**图注：** 构建不再是约束--围绕它的人类速度环节才是。人类速度的阶段保持原有时长，而构建阶段则坍缩至数小时。

Let's use a security bottleneck as an example. Security teams are sized for human output, so when agents multiply code output, either the review queue builds or code ships under-reviewed. A regulated organization can't accept either outcome, so its security and policy checks have to keep pace with the agents.

以安全瓶颈为例。安全团队的规模是按人类产出配置的，因此当代理让代码产出成倍增长时，要么审查队列不断堆积，要么代码在审查不足的情况下发布。受监管的组织两者都无法接受，所以它的安全与政策检查必须跟上代理的速度。

To better realize the productivity gains of and secure agentic AI, the traditional SDLC lifecycle requires the same level of transformation as the implementation phase has undergone.

为了更好地实现代理式 AI 的生产力收益并保障其安全，传统 SDLC 生命周期需要经历与实现阶段同等程度的转型。

## 什么是 AI 原生 SDLC？（What is an AI-native SDLC?）

The AI-native SDLC is a reimagined process that combines the old control objectives with new enforcement. Instead of a linear flow, the process becomes a loop, and AI is embedded at each point. The AI-native SDLC promotes automated handover and triggering of subsequent plays, helping to address the manual and clunky nature of handoff between the phases of the traditional SDLC.

AI 原生 SDLC 是一个重新构想的过程，它把旧的控制目标与新的执行方式结合起来。流程不再是线性流，而是变成一个循环，AI 嵌入在每一个环节。AI 原生 SDLC 促进自动交接以及后续打法（play）的自动触发，有助于解决传统 SDLC 各阶段之间交接手动而笨拙的问题。

You'll also hear this shift called the agentic SDLC, the AI SDLC, or simply agentic software development — the labels differ, but they describe the same thing.

你还会听到这种转变被称为代理式 SDLC（agentic SDLC）、AI SDLC，或者干脆叫代理式软件开发（agentic software development）--名称各异，但描述的是同一回事。

![](images/img-02.png)

### AI 原生 SDLC 六个阶段的转变（The shifts across the six stages of an AI-native SDLC）

The table below highlights the ends of the spectrum between traditional SDLC and AI-native SDLC, supported by Claude. Most organizations sit somewhere between the two columns.

下表突出了由 Claude 支撑的传统 SDLC 与 AI 原生 SDLC 这两个光谱端点。大多数组织都处在两列之间的某个位置。

| Stage | Traditional SDLC | AI-native SDLC |
|---|---|---|
| Plan | Requirements gathered by committee, distilled through workshops and sign-offs, written up by hand | Claude synthesizes pain points straight from the sources and captures them within `intent.md` which is human readable and machine actionable |
| Design | Spec written by analysts, parsed by designers | Requirements and design compressed into one working session with an agent, guided by standards encoded as skills, versioned in git |
| Build | Tests and code are handwritten and documentation is written after the main development happens | Tests and code are generated by AI and institutional knowledge is maintained as versioned machine-readable `CLAUDE.md` files and skills |
| Test | QA gates at stage boundaries | Continuous evals woven through implementation |
| Deploy | Humans review every line of code and governance occurs in review cycles, often inconsistently | Layers of agentic review with human review reserved for regulated and critical code. Governance is enforced as the AI acts, with hooks as approval gates |
| Maintain | Humans watch production for bugs | Agents monitor live deployments. Any breached control band is diagnosed and written back into the loop as a new `intent.md` |

| 阶段 | 传统 SDLC | AI 原生 SDLC |
|---|---|---|
| Plan | 由委员会收集需求，经过工作坊和签核逐步提炼，再手工撰写成文 | Claude 直接从源头综合痛点，并将其捕获到 `intent.md` 中，既可供人阅读，也可供机器执行 |
| Design | 规格由分析师撰写，再由设计师解读 | 需求与设计被压缩进与代理的一次工作会话中，以编码为 Skills 的标准为指引，并在 git 中版本化 |
| Build | 测试与代码由手工编写，文档在主要开发完成之后才补写 | 测试与代码由 AI 生成，组织知识以版本化的、机器可读的 `CLAUDE.md` 文件和 Skills 的形式维护 |
| Test | 在阶段边界设置 QA 关卡 | 持续评估（evals）贯穿实现过程 |
| Deploy | 人工逐行审查代码，治理发生在审查周期中，且往往执行不一致 | 多层代理式审查，人工审查仅保留给受监管和关键代码。治理在 AI 行动时即被强制执行，Hooks 充当审批关卡 |
| Maintain | 人工监视生产环境中的缺陷 | 代理监控线上部署。任何被突破的控制带（control band）都会被诊断，并作为新的 `intent.md` 写回循环 |

The thread running through the right-hand column is the committed artifact. Each stage ends by writing one to version control (including `intent.md`, `spec.md`, `plan.md`, the diff and its tests, the PR with its review findings, and the incident record) and the next stage begins by reading it. For the early stages, .md files are the predominant artifact because a product owner and an agent can both read and act on the same file. From Build onward, the artifact is code and its records. The chain of commits is also the audit trail: who asked for what, what the agent produced, and who approved it.

贯穿右列的主线是"已提交的工件（committed artifact）"。每个阶段都以向版本控制写入一个工件而结束（包括 `intent.md`、`spec.md`、`plan.md`、diff 及其测试、连同审查发现的 PR，以及事件记录），而下一个阶段则以读取该工件开始。在早期阶段，.md 文件是主要的工件形式，因为产品负责人和代理可以读取同一个文件并据以行动。从 Build 阶段开始，工件变为代码及其记录。这一连串提交同时也是审计轨迹（audit trail）：谁提出了什么请求、代理产出了什么、以及谁批准了它。

Humans remain accountable for every decision that requires judgment. In the agentic SDLC world, the human attention shifts along with the artifacts that must be reviewed.

每一项需要判断力的决策仍由人类负责。在代理式 SDLC 的世界里，人类的注意力随着必须审查的工件而转移。

Every stage commits an artifact the next stage can read. Together, the intent, the spec, the plan, the diff and the review findings are the audit trail.

每个阶段都提交一个下一个阶段可以读取的工件。意图、规格、计划、diff 和审查发现共同构成审计轨迹。

## 打法（Plays）

The plays are the core of the playbook and are grouped into six non-linear stages (Plan, Design, Build, Test, Deploy, Maintain), which together cover the complete lifecycle.

这些打法（plays）是整本实战手册的核心，被归入六个非线性的阶段（Plan、Design、Build、Test、Deploy、Maintain），它们共同覆盖了完整的生命周期。

Each play covers:

每个打法涵盖以下内容：

- What changes;

- 有什么变化；

- Getting started;

- 上手准备；

- Concrete steps for implementation;

- 具体实施步骤；

- Governance considerations; and

- 治理考量；以及

- How you measure whether it worked.

- 如何衡量它是否奏效。

The steps are modular and organizations may choose to prioritize transforming different stages at different times based on their unique needs. Each play names its dependencies under "Prerequisites," which the dependency graph further illustrates.

这些步骤是模块化的，组织可以根据自身独特需求，选择在不同时间优先转型不同的阶段。每个打法都在"Prerequisites（前置条件）"一项下列出其依赖，依赖关系图进一步加以说明。

A stage ends by committing an artifact with the commit initiating the next stage. An accepted `intent.md` triggers the requirements and design pass, an approved `spec.md` triggers plan mode, a merged PR triggers the pipeline, and a breached control band in production writes the next `intent.md` and so the loop continues.

一个阶段以提交一个工件而结束，而这次提交会启动下一个阶段。一份被接受的 `intent.md` 触发需求与设计环节，一份获得批准的 `spec.md` 触发 plan mode，一个被合并的 PR 触发流水线，而生产环境中被突破的控制带（control band）则写出下一份 `intent.md`，循环由此持续下去。

First, you prompt each step by hand with the end state being a loop in which each accepted artifact fires the next gate. Human attention concentrates at the gates, reviewing what the agent flagged rather than starting each stage from scratch.

起初，你为每一步手工输入提示（prompt），而最终形态则是一个循环：每一份被接受的工件都会触发下一道关卡。人类的注意力集中在各道关卡上，审查代理标记出的内容，而不是每个阶段都从零开始。

![](images/img-03.png)

**Caption:** The plays are listed with stage; the arrows give the order to adopt them in. The two are not the same. Start with any clay play — nothing points into it, so it needs nothing first. For any other play, the arrows pointing into it are the plays to adopt before it.

**图注：** 图中按所属阶段列出了各项打法；箭头给出了采纳它们的先后顺序。两者并不相同。可以从任意一个"黏土打法（clay play）"开始--没有箭头指向它，因此它不需要任何前置。对于其他任何打法，指向它的箭头所对应的打法就是应在它之前采纳的打法。

## 规划（Plan）

Ideas stop waiting for someone to write them up. Intent is captured once, in the originator's own words, as a version-controlled artifact the next stage can act on.

想法不再需要等待某人来把它们写下来。意图只需用发起人自己的话捕获一次，就成为下一个阶段可以据以行动的、受版本控制的工件。

### 以 intent.md 捕获（Capture as intent.md）

The `intent.md`, which kicks off the software development process can enter through different routes. A person has an idea, a ticket is filed, or an incident is surfaced via an alert (see Stage 6: Maintenance).

启动软件开发流程的 `intent.md` 可以通过不同路径进入流程：某人有了想法、提交了一个工单，或者某个事件通过告警浮出水面（见 Stage 6: Maintenance）。

When a person has an idea, they brainstorm with Claude and produce a markdown proto-spec. In the traditional SDLC, the same person must then convince a member of the product team to write the idea up with them or on their behalf.

当一个人有了想法时，他与 Claude 一起头脑风暴，产出一个 markdown 原型规格（proto-spec）。在传统 SDLC 中，这个人随后还必须说服产品团队的一位成员与他一起撰写、或代他撰写这个想法。

The proto-spec generated by Claude is human readable, version-controlled, and immediately consumable by the next stage. The proto-spec is saved as an `intent.md`.

Claude 生成的原型规格人类可读、受版本控制，并且可以被下一个阶段立即使用。原型规格保存为一份 `intent.md`。

Regardless of whether the intent originates from an event trigger or an agent, the same steps apply: the product owner reviews and corrects the agent-written `intent.md` before it is committed.

无论意图源自事件触发还是代理，步骤都相同：在提交之前，由产品负责人审查并修正代理写好的 `intent.md`。

**Traditional:** An idea passes through backlog entries, user stories, story points, and refinement meetings before anyone can act on it. Ownership transfers at each handoff, so what reaches engineering is several steps removed from what the originator meant.

**传统方式：** 一个想法要先经过待办条目（backlog entry）、用户故事、故事点和精化会议，才有人能据以行动。每次交接都会转移所有权，因此最终到达工程团队的东西，已经与发起人的本意隔了好几层。

**AI-native:** The originator brainstorms with Claude and writes the result down as `intent.md`, a proto-spec in the originator's own terms. The artifact contains what is wanted, why, and under which constraints. Repeat processes are encoded via skills.

**AI 原生方式：** 发起人与 Claude 一起头脑风暴，并把结果以他自己的术语写成 `intent.md`--一份原型规格（proto-spec）。该工件写明想要什么、为什么，以及在哪些约束之下。重复性的流程则通过 Skills 编码固化。

**Getting started（上手准备）**

**Prerequisites:** None.

**前置条件：** 无。

**Infrastructure:** Claude access for people who are not engineers (claude.ai or Cowork); an agreed `intent.md` template; a shared, version-controlled home for intent that the product owner watches. For a single product the simplest home is an `intent/` folder in the product repo. This setup keeps the artifact chain next to the code derived from it. A dedicated intent repo is only worth the overhead when intent spans many repositories, and in a monorepo it is a directory. The Stage 3: Build sidebar covers how this home relates to a Jira or requirements tool that already holds the record.

**基础设施：** 为非工程师人员提供 Claude 访问权限（claude.ai 或 Cowork）；一份商定好的 `intent.md` 模板；一个共享的、受版本控制的 intent 存放地（intent home），由产品负责人关注。对单个产品而言，最简单的存放地就是产品仓库里的一个 `intent/` 文件夹。这种设置让工件链与由它派生的代码放在一起。只有当 intent 跨越多个仓库时，专门的 intent 仓库才值得这份开销；而在 monorepo 中它只是一个目录。Stage 3: Build 的边栏会讲到这个存放地与已经保存记录的 Jira 或需求工具之间的关系。

Setting this up is a one-time task for the platform or engineering team. A technical team member needs to stand up the intent home and decide who can write to it, since many contributors will come from across the organization.

这套设置是平台团队或工程团队的一次性任务。需要一名技术团队成员搭建起 intent home 并决定谁可以写入，因为许多贡献者将来自整个组织。

Once the repository exists, contributors without git experience don't need to use git directly. Instead a connector to the version-control system (e.g. GitHub) lets Claude commit markdown files on their behalf from claude.ai or Cowork.

仓库建好之后，没有 git 经验的贡献者无需直接使用 git。取而代之的是，通过一个连接到版本控制系统（例如 GitHub）的连接器，Claude 可以在 claude.ai 或 Cowork 中代他们提交 markdown 文件。

#### 如何执行（How to execute it）

1. The originator describes the problem to Claude in their own words. The originator may describe what they cannot do today, who is affected by the idea, what better looks like, or what is out of scope. No formal language is required.

1. 发起人用自己的话向 Claude 描述问题。他可以描述今天做不到什么、这个想法影响到谁、更好的样子是什么，或者什么不在范围内。不需要任何正式的语言。

2. Brainstorm until the idea is concrete. Claude asks the questions an analyst would ask: scope, users, constraints, and what success looks like.

2. 头脑风暴，直到想法变得具体。Claude 会像分析师那样提问：范围、用户、约束，以及成功是什么样子。

3. Ask Claude to write the result as `intent.md` using the organization's template, which can be encoded as a skill set up by a technical team member and signed off by a lead. This can cover the problem, proposed outcome, affected users and systems, constraints, and open questions.

3. 请 Claude 按照组织的模板把结果写成 `intent.md`；该模板可以编码为一个 Skill，由技术团队成员搭建、由负责人签核。内容可以涵盖问题、期望的结果、受影响的用户与系统、约束以及待解问题。

4. The originator corrects anything Claude misunderstood.

4. 发起人纠正 Claude 理解错的地方。

5. Commit `intent.md` to the shared home. Author and timestamp join the record, and the product owner picks the idea up from there.

5. 把 `intent.md` 提交到共享的存放地。作者和时间戳随之进入记录，产品负责人从这里接手这个想法。

```markdown
# Intent: claims status self-service
Author: J. Ortiz (claims operations). Status: draft.

## Problem
Customers phone the contact center to ask where their claim is.
Handlers spend roughly a third of call time on status-only queries.

## Proposed outcome
Customers see claim status, next step and expected date in the portal.

## Affected users and systems
Claims handlers, portal team, claims-core API.

## Constraints
No new PII in the portal session. Existing authentication only.

## Open questions
Do third-party loss adjusters need access too?
```

#### 治理考量（Governance considerations）

The evidence is the committed `intent.md`, which lists the author, the timestamp and the full revision history. It's logged in the git history of the intent home. The product owner approves, and the accept or reject decision that sends the intent into Stage 2: Design is recorded as the merge or the closing review.

证据就是已提交的 `intent.md`，其中列出了作者、时间戳和完整的修订历史。它记录在 intent home 的 git 历史中。由产品负责人审批，而将意图送入 Stage 2: Design 的接受或拒绝决定，则以合并或关闭审查的形式被记录下来。

**如何衡量（How to measure it）**

**领先指标（Leading indicator）**

Time from first conversation to a committed `intent.md`, read from git history on the intent home, which records author and time stamp. The expectation is to fall from a multi-week elicitation and refinement cycle to hours.

从第一次对话到提交一份 `intent.md` 的时间，可从 intent home 的 git 历史中读取，其中记录了作者和时间戳。预期是从长达数周的需求引导与精化循环缩短到几小时。

**滞后指标（Lagging indicator）**

The survival rate, or the share of `intent.md` files that the product owner accepts into Stage 2: Design rather than closes. The accept or reject decision is recorded as the merge of the artifact or the closed review. Additionally, the number of changes made to the `intent.md` that are made after the first `spec.md` commit for the same change.

存活率，即产品负责人接受进入 Stage 2: Design（而非关闭）的 `intent.md` 文件所占比例。接受或拒绝的决定记录为工件的合并或被关闭的审查。此外，还有同一变更中在第一份 `spec.md` 提交之后才对 `intent.md` 做出的修改数量。

## 设计（Design）

Requirements and design collapse into one session. Policy is applied while the spec is written, not discovered in a review weeks later.

需求与设计坍缩为一次会话。政策在撰写规格的同时即被应用，而不是数周后的审查中才被发现。

### 需求与设计（Requirements and design）

Once approved by the product owner, Claude takes the accepted `intent.md` and produces a requirements and design spec. This is guided by the organization's [skills](https://code.claude.com/docs/en/skills) for brand, security, compliance, and UX.

获得产品负责人批准后，Claude 拿这份被接受的 `intent.md`，产出一份需求与设计规格。这一过程由组织针对品牌、安全、合规与 UX 的 [skills](https://code.claude.com/docs/en/skills) 来指引。

The product owner reviews that spec, but doesn't write it. The goal of this process is to create a spec the engineering team can plan against, with flagged areas of concern.

产品负责人审查这份规格，但并不亲自撰写。这个流程的目标是产出一份工程团队可以据以制定计划的规格，并标出需要关注的领域。

Front-end work is the clearest example. Once the `intent.md` is accepted, the product owner mocks the design up in [Claude Design](https://claude.com/product/design) (beta) from the `intent.md`, iterates on the mock, and then exports it to Claude Code to build.

前端工作是最清晰的例子。`intent.md` 被接受后，产品负责人基于它在 [Claude Design](https://claude.com/product/design)（beta）中把设计做成模型（mock），在模型上迭代，然后导出到 Claude Code 中进行构建。

**Traditional:** Requirements and design are separate phases run by separate teams. Analysts formalize the idea into requirements and designers then parse those back into a design. The separation exists for accountability, but it is slow and lossy.

**传统方式：** 需求与设计是由不同团队运行的两个独立阶段。分析师把想法正式化为需求，设计师再把这些需求解读回一个设计。这种分离是为了问责，但它既慢又有损。

**AI-native:** Both phases happen in a single prompted session. Claude takes `intent.md` and produces a requirements and design spec, constrained by the organization's skills, with areas of concern flagged.

**AI 原生方式：** 两个阶段都在一次提示驱动的会话中完成。Claude 拿到 `intent.md` 后产出一份需求与设计规格，受组织 Skills 的约束，并标出需要关注的领域。

**Getting started（上手准备）**

**Prerequisites:** Write an `intent.md` file, with brand, security, compliance, and UX policies written as skills.

**前置条件：** 写好一份 `intent.md` 文件，并把品牌、安全、合规与 UX 政策写成 Skills。

**Infrastructure:** A product owner with Claude access. No engineering skill is required.

**基础设施：** 一位有 Claude 访问权限的产品负责人。不需要任何工程技能。

#### 如何执行（How to execute it）

1. The product owner opens a session with the organization's skills available and attaches the `intent.md`.

1. 产品负责人在一个加载了组织 Skills 的会话中，附上这份 `intent.md`。

2. The product owners prompt points at the `intent.md`, names the constraints, and demands flagged concerns. Run it by hand at first, then codify it as an organization-level slash command. From there make the acceptance of `intent.md` in the intent home the trigger, with a non-interactive job that fires on the merge, run the pass with the organization's skills loaded, and commit `spec.md` as a pull request (the CI/CD play in Stage 5: Deploy covers the plumbing). From that point the product owner's first involvement is the review.

2. 产品负责人的提示指向这份 `intent.md`，点明各项约束，并要求标出值得关注的点。一开始先手工运行，然后把它固化为一个组织级的斜杠命令（slash command）。再往后，把 intent home 中 `intent.md` 被接受设为触发器：一个在合并时触发的非交互作业，加载组织的 Skills 运行这一环节，并以拉取请求（pull request）的形式提交 `spec.md`（Stage 5: Deploy 的 CI/CD 打法涵盖了这些管道工作）。从那时起，产品负责人的第一次介入就是审查。

3. The same product owner reviews the spec against the idea. Does the spec solve the stated problem, and are the open questions from `intent.md` answered or carried forward?

3. 由同一位产品负责人对照最初的想法审查这份规格：规格是否解决了所陈述的问题？`intent.md` 中的待解问题是得到了回答，还是被带到了后面？

4. Work through the flagged concerns first as they are the points an analyst would have escalated. The product owner resolves each one with its policy owner before engineering sees the spec.

4. 先逐一处理被标出的关注点，因为它们正是分析师原本会上报的问题。在工程团队看到规格之前，产品负责人要与对应的政策负责人逐条解决这些问题。

5. Commit `spec.md` alongside `intent.md`. The file pair records what was asked for and what was decided.

5. 把 `spec.md` 与 `intent.md` 一起提交。这对文件记录了要的是什么、以及决定了什么。

6. The product owner decides whether the spec and intent progress to build, consulting a technical lead for anything the organization classes as higher risk. A human team mate always makes this call, and accepting the spec is what starts the plan mode play in Stage 3: Build.

6. 由产品负责人决定规格与意图是否推进到构建阶段；凡属组织认定为较高风险的内容，需咨询技术负责人。这个决定始终由人类队友做出，而接受这份规格正是启动 Stage 3: Build 中 plan mode 打法的动作。

#### 实际样例（What it looks like (the prompt)）

```markdown
Read the attached intent.md and produce a requirements and design spec for integrating it into our existing codebase. Apply the skills available to you so the plan conforms to our brand guidelines, security policies and UX standards. Document the spec fully as spec.md, ready to hand to the engineering team. Describe clearly any areas of concern, especially where you cannot satisfy contradicting policies.
```

#### 治理考量（Governance considerations）

Instead of being discovered in a review weeks later, the live policy is read and applied while the spec is written. The organization's skills are applied as constraints on the spec. The spec, the prompt that produced it, and the skill versions in force are all logged in version control. The product owner signs off the spec, and routes flagged concerns to the named policy owners.

现行政策不再是在数周后的审查中才被发现，而是在撰写规格的同时就被读取并应用。组织的 Skills 作为对规格的约束被施加。规格、产生它的提示词以及当时生效的 Skill 版本，全部记录在版本控制中。产品负责人对规格签核，并把被标出的关注点转交给指定的政策负责人。

**如何衡量（How to measure it）**

**领先指标（Leading indicator）**

Elapsed time between the `intent.md` commit and the `spec.md` commit for the same change (two git timestamps), compared with the old requirements-plus-design cycle.

同一变更中，`intent.md` 提交与 `spec.md` 提交之间经过的时间（两个 git 时间戳），并与旧的需求加设计周期相比较。

**滞后指标（Lagging indicator）**

Requirements rework after build starts. Count `spec.md` commits dated after the first `plan.md` commit for the same change. Git log will give this directly.

构建开始后的需求返工。统计同一变更中日期晚于第一份 `plan.md` 提交的 `spec.md` 提交数量。git log 可以直接给出这个数字。

## 构建（Build）

Nothing is implemented without an accepted plan. Institutional knowledge becomes files the agent reads, and the guardrails run as code rather than as habits.

没有获得接受的计划，就什么都不实现。组织知识（institutional knowledge）变成代理会读取的文件，护栏以代码而非习惯的方式运行。

### 以 Claude Code plan mode 作为默认起点（Claude Code plan mode as the default starting point）

Engineers start Claude Code sessions in [plan mode](https://code.claude.com/docs/en/permission-modes), give Claude the approved `spec.md` from Stage 2: Design, and let it interview them, iterating on the plan until the engineer is happy with it.

工程师以 [plan mode](https://code.claude.com/docs/en/permission-modes) 启动 Claude Code 会话，把来自 Stage 2: Design 的已批准 `spec.md` 交给 Claude，让它反过来采访自己，并在计划上迭代，直到工程师满意为止。

**Traditional:** An engineer reads the design and starts writing code. How the change will be made, down to which files and which tests, stays in the engineer's head or at best a ticket comment. Nobody else can review it. The first thing a reviewer sees is the finished diff, and by then rework is slow.

**传统方式：** 工程师读完设计就开始写代码。这个改动会怎么做--具体到改哪些文件、写哪些测试--只留在工程师脑子里，顶多写进一条工单评论。别人无从审查。审查者看到的第一样东西就是完成的 diff，而到那时返工已经很慢了。

**AI-native:** Work starts with a written plan that Claude produces in plan mode, where it can read the codebase without changing anything. The engineer corrects the plan before code is written, and the approved version is committed as `plan.md` for later stages to check against.

**AI 原生方式：** 工作从一份书面计划开始，它由 Claude 在 plan mode 下产出--在该模式下 Claude 可以读取代码库而不做任何更改。工程师在写代码之前先修正计划，获得批准的版本作为 `plan.md` 提交，供后续阶段对照检查。

**Getting started（上手准备）**

**Prerequisites:** The intent artifact (`intent.md` or `spec.md`) if one exists, and the `CLAUDE.md` file helps.

**前置条件：** 意图工件（如果存在的话是 `intent.md` 或 `spec.md`），另外 `CLAUDE.md` 文件会有所帮助。

**Infrastructure:** Claude Code with access to the repository.

**基础设施：** 可访问仓库的 Claude Code。

#### 如何执行（How to execute it）

1. The engineer starts the session in plan mode with Claude.

1. 工程师以 plan mode 与 Claude 开启会话。

2. The engineer gives Claude the `intent.md` and the `spec.md` and asks for an implementation plan that names the files that change, the order of the work, and the tests that prove it.

2. 工程师把 `intent.md` 和 `spec.md` 交给 Claude，请它给出一份实现计划，写明会改动的文件、工作的先后顺序，以及证明改动正确的测试。

3. Interrogate the plan by asking what the change could break, which step is most risky, and what other options Claude chose not to do.

3. 拷问这份计划：这个改动可能破坏什么、哪一步风险最大、以及 Claude 放弃了哪些其他方案。

4. Iterate until an engineer who has never seen the conversation could implement the change from the plan alone.

4. 持续迭代，直到一位从未见过这场对话的工程师也能仅凭这份计划实现该改动。

5. Commit the approved plan as `plan.md`. The plan joins the audit trail, and the PR review play (Stage 5: Deploy) checks the eventual diff against it.

5. 把批准的计划作为 `plan.md` 提交。计划由此进入审计轨迹，PR 审查打法（Stage 5: Deploy）会对照它检查最终的 diff。

6. Accept the plan and let Claude implement. With a solid plan, the implementation is often a single pass.

6. 接受计划并让 Claude 去实现。有了扎实的计划，实现往往一轮就能完成。

7. When implementation departs from the plan, update `plan.md` in the same commit. Consider using a hook to enforce synchronization between the two.

7. 当实现偏离计划时，在同一次提交中更新 `plan.md`。可以考虑使用一个 Hook 来强制两者保持同步。

#### 实际样例（What it looks like (plan.md)）

```markdown
# Plan: claims status self-service (from intent.md 2026-06-02)

## Files that change
portal/src/claims/StatusPanel.tsx (new), claims-api/routes/status.py,
claims-api/tests/test_status.py

## Order of work
1. Add the status endpoint behind existing auth.
2. Panel against the endpoint.
3. Wire into the portal nav.

## Risks
The claims-core API rate-limits at 50 rps; the panel must cache.

## Proof
test_status.py covers the four claim states; screenshot matches the
approved mock.
```

#### 治理考量（Governance considerations）

Design review happens before any code is generated, when changing course is still a matter of editing a document. Plan mode enforces this itself, since Claude cannot edit files until the engineer accepts the plan. The plan and its revisions are logged along with who accepted it. Routine changes are approved by the engineer, and anything the organization classes as higher risk goes to a tech lead or architect.

设计审查发生在任何代码生成之前，此时改变方向不过是编辑一份文档而已。plan mode 本身就在强制这一点，因为在工程师接受计划之前，Claude 无法编辑文件。计划及其修订都会连同批准人一起被记录。常规改动由工程师批准，凡属组织认定为较高风险的内容则交给技术负责人或架构师。

**如何衡量（How to measure it）**

**领先指标（Leading indicator）**

Share of changes that merge from the first implementation pass, and time from plan approval to merged PR with the required data within the PR metadata.

从第一轮实现就成功合并的改动所占比例，以及从计划批准到 PR 合并的时间（所需数据包含在 PR 元数据中）。

**滞后指标（Lagging indicator）**

Rework cycles per change, again from the PR metadata, and how often the merged diff still matches the committed `plan.md`.

每个改动的返工次数（同样来自 PR 元数据），以及合并的 diff 与已提交 `plan.md` 仍然一致的比例。

### Claude Code 的 auto 模式（Claude Code on auto mode）

Claude Code can also run in auto mode, where the engineer approves the plan and, once happy and iterated upon, Claude applies each change without a per-edit prompt. As the guardrails from the later plays mature (a tuned `CLAUDE.md`, skills that encode policy, hooks that block unsafe actions, and a test suite Claude can run), auto-accept becomes the default for routine work: a tight `spec.md`, a small blast radius, and code the tests already cover.

Claude Code 也可以在 auto 模式（auto mode）下运行：工程师批准计划，并在满意、充分迭代之后，让 Claude 无需每次编辑都提示即可应用每一处更改。随着后续打法中的护栏走向成熟（调优过的 `CLAUDE.md`、把政策编码进去的 Skills、阻止不安全操作的 Hooks，以及 Claude 能够运行的测试套件），自动接受（auto-accept）成为常规工作的默认方式：一份严密的 `spec.md`、一个很小的爆炸半径（blast radius），以及测试已经覆盖的代码。

The shift is now away from the user watching the agent make the edits and reviewing actions, towards the review of artifacts after longer autonomous sessions. Auto-accept mode further enables parallelism across individuals and the team when used with worktrees and is fundamental to running the SDLC autonomously and closing the loop as described in Stage 6: Maintenance.

转变的方向是：不再是用户盯着代理做编辑、逐一审查每个动作，而是在更长的自主会话结束后审查工件。配合 worktree 使用时，auto-accept 模式进一步让个人与团队层面的并行成为可能，并且是自主运行 SDLC、按 Stage 6: Maintenance 所述闭合循环的基础。

**边栏（Sidebar）**

### 遗留系统与事实来源（Legacy systems and the source of truth）

Applies to every artifact the process produces.

适用于该流程产出的每一个工件。

Existing SDLC processes likely already track artifacts, just not in markdown files. Work items may be in Jira, requirements in a tool with regulatory traceability built in, designs in Figma, and change approvals with a change board. Those systems are hard to displace because auditors and regulators already accept them and other teams depend on them, so the AI-native SDLC has to fit around what exists.

现有的 SDLC 流程很可能已经在跟踪工件，只是没有用 markdown 文件。工作项可能放在 Jira 里，需求放在一个内置监管追溯能力的工具里，设计放在 Figma 里，变更审批则经由变更委员会。这些系统难以取代，因为审计者和监管者已经认可它们、其他团队也依赖它们，所以 AI 原生 SDLC 必须迁就现有环境。

When transitioning to the AI-native SDLC, for every artifact the process produces, name one system as the source of truth, with everything else holding a copy or a link to the original. The configurations below can be set up to have one source of truth, with the choice differing per artifact:

在向 AI 原生 SDLC 过渡时，对流程产出的每一个工件，都指定一个系统作为事实来源（source of truth），其余一切都只保存副本或指向原件的链接。可以把下面的几种配置设置为只有一个事实来源，具体选择因工件而异：

**The repo as the source of truth.**

The markdown artifacts are the authoritative record and the legacy system references files within commits. This can be one of the cleanest configurations for engineering-led organizations, as all records live in one tool with one timestamp authority.

**以仓库作为事实来源。**

markdown 工件就是权威记录，遗留系统在提交中引用这些文件。对工程主导的组织而言，这可能是最干净的配置之一，因为所有记录都保存在同一个工具里，拥有唯一的时间戳权威。

**The legacy system as the source of truth.**

Jira, ServiceNow, or the requirements tool holds the authoritative record and the markdown artifacts are working copies. Claude reads the record at the start of the session and writes the outcome back through an `MCP` connector in the same session that produced the spec or the plan.

**以遗留系统作为事实来源。**

由 Jira、ServiceNow 或需求工具保存权威记录，markdown 工件则是工作副本。Claude 在会话开始时读取记录，并在产出规格或计划的同一个会话中，通过 `MCP` 连接器把结果写回。

**Linkage as the minimum bar.**

All artifacts note the record ID and all legacy records contain the commit SHA of the markdown file. Linkage is a good place to start when transitioning to the AI-native SDLC, accepting that there are two sources of truth.

**以互链（linkage）作为最低标准。**

所有工件都注明记录 ID，所有遗留记录都包含 markdown 文件的提交 SHA。在向 AI 原生 SDLC 过渡时，互链是一个很好的起点，前提是接受存在两个事实来源。

Both the legacy system and the markdown-first system can coexist, so long as there is a link between the two or one is declared the source of truth.

遗留系统与 markdown 优先的系统可以共存，只要两者之间有链接，或者宣布其中一方为事实来源。

### CLAUDE.md 文件（The CLAUDE.md）

[`CLAUDE.md`](https://code.claude.com/docs/en/memory) gives Claude the context a new joiner would need, covering conventions, commands, architecture, and the mistakes the team sees most often. Knowledge that used to sit in people's heads and on wikis becomes a file the agent reads at the start of every session, maintained by the whole team and iterated on whenever a mistake is made.

[`CLAUDE.md`](https://code.claude.com/docs/en/memory) 为 Claude 提供新成员入职时需要了解的背景，涵盖约定、命令、架构，以及团队最常犯的错误。过去存在人们脑子里和 wiki 上的知识，变成了代理在每次会话开始时都会读取的文件，由整个团队维护，并且每当出现错误时都持续迭代。

**Getting started（上手准备）**

**Prerequisites:** None.

**前置条件：** 无。

**Infrastructure:** A repo, Claude Code installed, and one engineer who knows the codebase well.

**基础设施：** 一个仓库、安装好的 Claude Code，以及一位非常熟悉代码库的工程师。

#### 如何执行（How to execute it）

1. Run `/init` in the repo. Claude generates a starting `CLAUDE.md` from what it finds.

1. 在仓库中运行 `/init`。Claude 会根据它所发现的内容生成一份初始 `CLAUDE.md`。

2. Cut the generated file down to what a new joiner would need on day one. Keep the build, test and lint commands, the conventions that matter, and the things Claude keeps getting wrong.

2. 把生成的文件精简到新成员第一天所需的程度。保留构建、测试和 lint 命令、重要的约定，以及 Claude 总是搞错的东西。

3. Check `CLAUDE.md` into git at the repo root so the whole team shares one version and changes are reviewed like code.

3. 把 `CLAUDE.md` 提交到仓库根目录的 git 中，这样整个团队共享同一个版本，改动也像代码一样被审查。

4. A working rule helps here. When Claude makes a mistake twice, the correction goes into `CLAUDE.md`.

4. 这里有一条实用的经验法则：当 Claude 犯同一个错两次时，就把纠正写进 `CLAUDE.md`。

5. Keep it under a page, because Claude reads all of it at the start of a session and anything stale is taking up context for no benefit.

5. 把它控制在一页以内，因为 Claude 会在会话开始时读取它的全部内容，任何过时的内容都在白白占用上下文。

#### 实际样例（What it looks like (CLAUDE.md)）

```javascript
# Payments service

## Commands
- Build: make build
- Test: make test (unit), make itest (integration, needs docker)
- Lint: make lint (runs in CI; fix before pushing)

## Conventions
- Java 21, Spring Boot 3. No new Lombok.
- Money is always BigDecimal, never double.
- Every endpoint needs an integration test in src/itest.

## Architecture
- api/ holds REST controllers, core/ holds domain logic,
  adapters/ talks to external systems.
- Kafka events are defined in schemas/; never edit generated classes.

## Things Claude gets wrong
- Do not bump dependency versions; the platform team owns them.
- The legacy v1/ package is frozen; changes go in v2/.
```

#### 治理考量（Governance considerations）

`CLAUDE.md` is version controlled, so the instructions the agent works to are reviewable and auditable. Team conventions are applied through the file, changes to it are logged in git history, and code owners approve those changes in PR review.

`CLAUDE.md` 受版本控制，因此代理遵循的指令是可审查、可审计的。团队约定通过该文件来落实，对它的更改会记录在 git 历史中，并由代码所有者（code owners）在 PR 审查中批准。

**如何衡量（How to measure it）**

**领先指标（Leading indicator）**

How often Claude repeats a mistake `CLAUDE.md` should have caught. The corrections or changes to the `CLAUDE.md` should be tracked within the git history.

Claude 重复犯本应被 `CLAUDE.md` 拦住的错误的频率。对 `CLAUDE.md` 的纠正或更改应在 git 历史中跟踪。

**滞后指标（Lagging indicator）**

Time to first merged PR for a new member of the team from PR history.

从 PR 历史读取的新成员首次合并 PR 所需的时间。

### 用 Skills 承载组织知识（Skills as institutional knowledge）

Skills are how an organization makes its institutional knowledge operational. The instructions are explicit, version-controlled, applied broadly, and updated centrally when policy changes. The rule of thumb: write a skill for institutional knowledge that must be applied consistently; don't write a skill for components that belong in `CLAUDE.md` or a prompt.

Skills 是组织让自身组织知识落地运转的方式。指令是明确的、受版本控制的、被广泛应用的，并在政策变化时集中更新。经验法则是：必须被一致执行的组织知识，就写成 Skill；属于 `CLAUDE.md` 或提示词的内容，就不要写成 Skill。

**Getting started（上手准备）**

**Prerequisites:** None required. Having a `CLAUDE.md` helps, because it keeps the agent's working knowledge in the repo, but a skill does not depend on it.

**前置条件：** 无硬性要求。有一份 `CLAUDE.md` 会有帮助，因为它把代理的工作知识保存在仓库里，但 Skill 并不依赖它。

**Infrastructure:** One policy with a named owner and a written source of truth.

**基础设施：** 一条有明确负责人和成文事实来源的政策。

#### 如何执行（How to execute it）

1. Pick one piece of knowledge that is enforced inconsistently today. This could be a security standard, an API design convention, or a brand rule.

1. 挑一条如今执行得不一致的知识。它可以是一条安全标准、一个 API 设计约定，或一条品牌规则。

2. Write it as a skill, a folder containing a `SKILL.md` whose frontmatter says when it triggers and whose body says what to do. An engineer writes it from the policy owner's source of truth, using Claude to help.

2. 把它写成一个 Skill：一个包含 `SKILL.md` 的文件夹，其 frontmatter 说明何时触发，正文说明要做什么。由一名工程师依据政策负责人的事实来源来撰写，并借助 Claude 协助。

3. Put the skill in the repo at `.claude/skills/<name>/` so it ships with the code, or distribute it organization-wide through a `plugin`.

3. 把 Skill 放进仓库的 `.claude/skills/<name>/` 目录，让它随代码一起发布；或者通过一个 `plugin` 在整个组织范围内分发。

4. Test that the skill triggers. Ask Claude to do the relevant task in different ways and confirm the skill loads each time.

4. 测试 Skill 是否会触发。用不同方式让 Claude 执行相关任务，确认每次 Skill 都会加载。

5. When the policy changes, change the skill and have the policy owner sign off the change.

5. 当政策变化时，同步修改 Skill，并由政策负责人签核这一更改。

6. Engineers pick up the new version automatically in their next session.

6. 工程师在下一次会话中自动获得新版本。

#### 实际样例（What it looks like (.claude/skills/secure-api-review/SKILL.md)）

```markdown
---
name: secure-api-review
description: Apply the API security standard. Use whenever creating or
  modifying an external-facing endpoint, reviewing API code, or
  generating an OpenAPI spec.
---
# Secure API review

When you create or change an API endpoint:
1. Authentication: every endpoint requires the gateway JWT;
   no anonymous routes outside /health.
2. Input validation: validate request bodies against the OpenAPI
   schema and reject unknown fields.
3. Audit: every state-changing endpoint emits an audit event with
   actor, action, entity and timestamp.
4. Data classification: fields tagged pii in the schema must never
   appear in logs or error messages.

Run scripts/check-endpoints.sh and include its output in your summary.
```

#### 治理考量（Governance considerations）

A skill is a control, though an advisory one. It makes Claude likely to apply the policy while the code is written, and nothing forces a session to comply with it. A policy that must always hold needs something deterministic behind the skill, such as a hook that blocks the action or a review pass that re-checks the policy at the PR. The skill makes violations rare and the hook makes them close to impossible. Skill invocations are logged in session traces, and the policy owner reviews skill changes like code.

Skill 是一种控制手段，不过是建议性的。它让 Claude 在编写代码时倾向于应用政策，但没有任何东西强制会话遵守它。必须始终成立的政策需要在 Skill 背后再加上确定性的机制，比如一个阻止该操作的 Hook，或在 PR 处复查该政策的审查环节。Skill 让违规变得罕见，Hook 则让违规几乎不可能发生。Skill 的调用会记录在会话轨迹（session traces）中，政策负责人像审查代码一样审查 Skill 的更改。

**如何衡量（How to measure it）**

**领先指标（Leading indicator）**

Time from the policy owner approving a policy change to the updated skill merging, taken from the PR on the skill folder.

从政策负责人批准政策变更，到更新后的 Skill 合并所需的时间，取自 Skill 文件夹上的 PR。

**滞后指标（Lagging indicator）**

PR reviews findings that cite the policy, which should fall towards zero once the skill is applying the policy while the code is written. Where the findings don't fall towards zero, either the skill isn't triggering or its text has drifted from the official policy.

PR 审查中引用该政策的发现（findings），一旦 Skill 在编写代码时就应用了政策，这一数字应趋近于零。如果发现没有趋近于零，要么是 Skill 没有触发，要么是其文本已偏离官方政策。

### 用 Hooks 做构建期护栏（Hooks as build-time guardrails）

A skill is an advisory control while a [hook](https://code.claude.com/docs/en/hooks) is the deterministic layer behind it. Most of Claude's actions are file edits and shell commands during implementation, so the build phase is where hooks can end up firing most often.

Skill 是建议性的控制，而 [hook](https://code.claude.com/docs/en/hooks) 是它背后的确定性层。在实现阶段，Claude 的大多数动作是文件编辑和 shell 命令，因此构建阶段是 Hooks 最常触发的地方。

Build-phase hooks can:

构建期的 Hooks 可以：

- Block edits to protected paths such as generated classes or a frozen package;

- 阻止对受保护路径（如生成的类或已冻结的包）的编辑；

- Run the formatter and linter after file edits so drift never accumulates;

- 在文件编辑之后运行格式化器和 linter，使漂移从不累积；

- Keep credentials out of the diff.

- 把凭据挡在 diff 之外。

Back any skill whose policy has to hold without exception. A hook runs on each action that matches it, so build-phase hooks should be fast and scoped to the file that changed. Heavier checks such as the full test suite belong at the commit or the PR.

凡是政策必须无条件成立的 Skill，都要有 Hook 在背后支撑。Hook 会在每个匹配的动作上运行，因此构建期的 Hook 应当快速，并把作用范围限定在被更改的文件上。更重的检查（例如完整测试套件）应放在提交或 PR 环节。

A hook that asks a human for approval belongs with the gates in Stage 5: Deploy, because an approval prompt during the build puts a person back on the critical path of all the sessions running in parallel.

需要向人类请求批准的 Hook 应归入 Stage 5: Deploy 的关卡，因为构建期间的批准提示会把一个人重新放回所有并行会话的关键路径上。

### 并行会话与子代理（Parallel sessions and subagents）

One engineer can drive several streams of work at once.

一名工程师可以同时推进多条工作流。

A parallel session is another full Claude Code instance, working a separate task in its own [git worktree](https://code.claude.com/docs/en/worktrees). Each independent session knows nothing about the others, and the engineer steering them is the only thing they share.

并行会话是另一个完整的 Claude Code 实例，在自己的 [git worktree](https://code.claude.com/docs/en/worktrees) 中处理一个独立的任务。每个独立会话对其他会话一无所知，唯一共享的是驾驭它们的工程师。

A [subagent](https://code.claude.com/docs/en/sub-agents) runs inside a single session as a scoped helper with its own context window and tool limits and suits jobs that recur in multiple tasks such as verifying the app runs as expected.

[子代理（subagent）](https://code.claude.com/docs/en/sub-agents)在单个会话内运行，是一个拥有自己上下文窗口和工具限制的、作用范围受限的助手，适合在多个任务中反复出现的工作，例如验证应用按预期运行。

Parallel sessions raise the number of tasks an engineer can have in flight, while subagents keep each session focused on its own task. The engineer's job is steering and reviewing all of them.

并行会话提高了工程师可以同时推进的任务数量，而子代理让每个会话专注于自己的任务。工程师的职责是驾驭并审查所有这些会话。

**Traditional:** One engineer works one task at a time and spends a significant portion of their day or week on builds, tests and reviewers. Switching between tasks while waiting is possible, but the context switch is tiring enough that few people choose to.

**传统方式：** 一名工程师一次只做一项任务，每天或每周有相当大的一部分时间耗在构建、测试和评审上。等待时切换到别的任务是可行的，但上下文切换实在太累，很少有人愿意这么做。

**AI-native:** One engineer runs several Claude sessions at once, each in its own worktree on its own task. Repeated jobs become subagents with their own context and tool limits. The engineer's job shifts to orchestrating, and eventually, to building and monitoring loops.

**AI 原生方式：** 一名工程师同时运行多个 Claude 会话，每个会话在自己的 worktree 中处理自己的任务。重复性的工作变成拥有自己上下文和工具限制的子代理。工程师的职责转向编排，并最终转向搭建和监控循环。

**Getting started（上手准备）**

**Prerequisites:** The `CLAUDE.md`, since all sessions read the file. The feedback loop (Stage 4: Test) also helps here, because less supervision from the engineer is needed when a session can verify its own work.

**前置条件：** `CLAUDE.md`，因为所有会话都会读取该文件。反馈循环（Stage 4: Test）在这里也有帮助，因为当一个会话能够验证自身的工作时，所需的工程师监督就更少。

**Infrastructure:** A git repository, since isolation comes from worktrees and permission settings tuned so sessions are not waiting on approval prompts for commands the organization considers safe.

**基础设施：** 一个 git 仓库，因为隔离来自 worktree；还要调好权限设置，让会话不会因组织认为安全的命令而卡在批准提示上。

#### 如何执行（How to execute it）

1. The engineer splits the work into tasks that touch different files, using the plan from the plan mode play (Stage 3: Build) to see where the work is independent. Tasks that share files run in a single session, one after another.

1. 工程师把工作拆成触及不同文件的任务，借助 plan mode 打法（Stage 3: Build）产出的计划来判断哪些工作彼此独立。共享文件的任务在单个会话中依次运行。

2. Each parallel task gets its own worktree, for example `claude --worktree feature-auth` in one terminal and `claude --worktree fix-rate-limit` in another. A worktree is a separate checkout on its own branch, which stops sessions colliding on files.

2. 每个并行任务都有自己的 worktree，例如在一个终端运行 `claude --worktree feature-auth`，在另一个终端运行 `claude --worktree fix-rate-limit`。worktree 是独立分支上的独立检出，可以避免会话在文件上互相冲突。

3. Two or three sessions is a sensible starting point. The practical ceiling is how many streams one person can review properly, so add sessions only while review is keeping up.

3. 两三个会话是合理的起点。实际上限是一个人能够认真审查的流的数量，因此只在审查跟得上的情况下继续增加会话。

4. Turn repeated jobs into subagents, as defined in markdown files in `.claude/agents/`, each with a name, a description of when to use it, and the tools it may touch. Examples include a code simplifier that strips needless complexity after the main agent finishes, a verifier that runs the app and checks behavior, a researcher that explores the codebase and reports back without flooding the main context. Check the definitions into git so the whole team shares them.

4. 把重复性的工作变成子代理，以 `.claude/agents/` 目录中的 markdown 文件定义，每个子代理有名字、一段说明何时使用它的描述，以及它可以使用的工具。例如：在主代理完成后剔除不必要复杂度的代码简化器（code simplifier）、运行应用并检查行为的验证器（verifier）、探索代码库并汇报结果而不淹没主上下文的研究员（researcher）。把这些定义提交进 git，让整个团队共享。

#### 实际样例（What it looks like (.claude/agents/verifier.md)）

```javascript
---
name: verifier
description: Runs the app and checks the change works before the session
  reports done
tools: Bash, Read
---
Start the app with make run. Exercise the changed behavior and the two
nearest neighboring flows. Report what you ran, what you saw, and any
behavior that does not match plan.md. Do not fix anything; report only.
```

#### 治理考量（Governance considerations）

More sessions means more output, so the controls have to come from configuration in the repo. Hooks and permission settings there apply to all sessions, and what a session does is logged and attributed to the engineer who ran it.

会话越多意味着产出越多，因此控制必须来自仓库中的配置。仓库里的 Hooks 和权限设置对所有会话生效，会话所做的事情会被记录，并归因到运行它的工程师名下。

**如何衡量（How to measure it）**

**领先指标（Leading indicator）**

Concurrent sessions per engineer while review quality holds, counted from the OpenTelemetry export, and the share of the day spent steering rather than waiting.

在审查质量保持的前提下每位工程师的并发会话数（从 OpenTelemetry 导出数据统计），以及一天中用于驾驭而非等待的时间占比。

**滞后指标（Lagging indicator）**

Changes merged per engineer per week read alongside the rework rate as determined per the PR history.

结合由 PR 历史确定的返工率，一起解读每位工程师每周合并的改动数量。
## 测试（Test）

Every session checks its own work before a human sees it, and the configuration that steers the agent gets regression-tested like the code it writes.

每个会话在人类看到结果之前会先检查自己的工作，而引导代理行为的那些配置，也要像它写出的代码一样接受回归测试。

### 给 Claude 一个反馈回路（Give Claude a feedback loop）

Always give Claude a way to verify its own work, whether tests, a build, or a screenshot diff. A session checks its own work and fixes its own mistakes before an engineer sees them.

始终给 Claude 一种验证自身工作的手段，无论是测试、构建，还是截图对比。会话会在工程师看到之前自查工作、修正自己的错误。

The feedback loop should not be confused with a verifier subagent (Stage 3: Build). The feedback loop runs through the whole task as many times as the work. The verifier subagent, on the other hand, is one way to package the final check by running a fresh context window once the session believes the work is done. This way the verdict is not colored by the assumptions that produced the code.

不要把反馈回路与验证器子代理（verifier subagent，见第 3 阶段：构建）混为一谈。反馈回路贯穿整个任务，随工作需要运行任意多次；而验证器子代理是在会话认为工作已经完成时，用一个全新的上下文窗口来打包执行最终检查的一种方式。这样，判定结论就不会被产出代码的那些预设假设所左右。

**Traditional:** The signal that code works arrives late. CI minutes later, a tester days later, production weeks later. With an agent producing the code, a late signal means a person has to check all of its output, and that person becomes the bottleneck.

**传统方式：** 代码能跑的信号来得很迟：CI 上是几分钟之后，测试人员是几天之后，生产环境是几周之后。当代码由代理生成时，迟到的信号意味着必须有一个人去检查它的全部产出，而这个人就成了瓶颈。

**AI-native:** The session is given a way to check its own work before a person sees it. Run the tests, run the build, take the screenshot. Claude iterates until the check passes, so what reaches the engineer has already passed it. Setting the loop up falls to the engineer running the session, and the steps below are written for them.

**AI 原生方式：** 会话在人看到结果之前就获得了自查的手段：跑测试、跑构建、截图。Claude 不断迭代，直到检查通过，因此到达工程师手里的东西已经通过了检查。搭建这个回路是运行会话的工程师的职责，下面的步骤就是为他们写的。

**快速上手（Getting started）**

**Prerequisites:** None.

**前置条件：** 无。

**Infrastructure:** A test suite and a build that run locally with one command each. For the UI work, a way for Claude to see the result is crucial, either a browser tool or a screenshot utility wired in via MCP.

**基础设施：** 一套本地各用一条命令即可运行的测试套件和构建。对于 UI 工作而言，让 Claude 能"看到"结果至关重要，可以是通过 MCP 接入的浏览器工具，也可以是截图工具。

#### 如何执行（How to execute it）

1. If checking the work today takes a sequence of commands and some environment knowledge, wrap it in a single target such as "make test" or "npm test" that exits non-zero on failure.

2. In the `CLAUDE.md`'s Commands section, list each command with an example of a healthy output.

3. State a target and make it quantifiable so Claude can check the work without asking you, for example: "All tests in test_status.py pass," "the screenshot matches the attached mock," or "the endpoint returns 200 with the new field".

4. For bug fixes, write the failing test first. Ask Claude to reproduce the bug as a test, run it, and confirm it fails for the reason you expect. Commit that test. Only then ask Claude to make it pass without editing the test, with the test-file hook from the final step enforcing the restriction. A test that existed before the fix, and that the agent couldn't rewrite, is proof the bug is gone.

5. For UI work, close the loop with a visual check. Give Claude a browser or screenshot tool, give it the mock, and let it iterate. Implement, screenshot, compare, and adjust. Two or three rounds is normal, and the result should improve with each one.

6. Make verification part of "done." Instruction lives in `CLAUDE.md`. Run the tests before reporting a task complete, and show the output.

7. Finally, the loop itself needs protecting, because an agent fixing code must not be able to weaken the check on that code. A hook that blocks edits to test files during a fix task does this. The alternative is to check the diff in review and reject any change that touches a test.

1. 如果今天检查工作需要一串命令和一些环境知识，就把它封装成一个单一目标，例如 "make test" 或 "npm test"，失败时以非零状态码退出。

2. 在 `CLAUDE.md` 的 Commands 一节中，为每条命令附上一个健康输出的示例。

3. 给出目标并让它可量化，使 Claude 无须来问你就能自查工作，例如："test_status.py 中的所有测试通过"、"截图与附带的设计稿一致"，或"端点返回 200 且带有新字段"。

4. 修复缺陷时，先写失败的测试。让 Claude 把这个缺陷复现为一条测试，运行它，并确认它失败的原因与你预期的一致。提交这条测试。然后才让 Claude 在不编辑该测试的前提下使其通过，并由最后一步中的测试文件 hook 来强制这一约束。一条在修复之前就已存在、且代理无法改写的测试，就是缺陷已被消除的证明。

5. 对于 UI 工作，用视觉检查来闭合回路。给 Claude 一个浏览器或截图工具，把设计稿交给它，让它自行迭代：实现、截图、对比、调整。两三轮是常态，而且结果应当一轮比一轮好。

6. 把验证变成"完成"定义的一部分。指令写在 `CLAUDE.md` 里：在报告任务完成之前先运行测试，并展示输出。

7. 最后，回路本身也需要保护，因为修复代码的代理绝不能削弱针对这段代码的检查。一个在修复任务期间阻止编辑测试文件的 hook 可以做到这一点。另一种做法是在评审中检查 diff，拒绝任何触及测试的改动。

#### 实际样子（What it looks like (CLAUDE.md verification block)）

```javascript
## Verifying your work

- Build: make build (must finish with "Build succeeded")
- Test: make test (all green; never skip or delete a failing test)
- Lint: make lint (zero warnings)

Run all three before reporting any task complete, and paste the output.
If a test fails, fix the code, not the test.
```

**治理考量（Governance considerations）**

**What is enforced:** Verification before a task is reported done, and the block on the agent editing test files during a fix, both implemented as hooks where the organization wants them guaranteed.

**强制执行的内容：** 在报告任务完成之前必须先完成验证，以及修复期间禁止代理编辑测试文件；凡组织要求保证执行之处，二者均以 hook 实现。

**What the evidence is:** The literal output of "make test," the build log, or the screenshot diff that Claude ran and pasted, so the evidence comes from the toolchain.

**证据是什么：** Claude 运行并粘贴的 "make test" 原始输出、构建日志或截图对比，因此证据直接来自工具链本身。

**Where it is logged:** In the session transcript, which the OpenTelemetry export forwards to the organization's observability stack, and in the PR's check run, where the reviewer and any later auditor can both see it.

**记录在哪里：** 会话记录（OpenTelemetry 导出会将其转发到组织的可观测性栈），以及 PR 的 check run（评审者与后续审计者都能看到）。

**Who approves:** The code owner reviewing the PR, who can concentrate on intent and risk because the mechanical evidence is already attached.

**由谁批准：** 评审该 PR 的代码所有者（code owner）；由于机械性的证据已经附上，他们可以专注于意图与风险。

**如何衡量（How to measure it）**

**Leading indicator:** First-pass CI success rate for agent-written changes, which the CI system already supports.

**领先指标（Leading indicator）：** 代理所写变更的 CI 一次通过率，CI 系统本身已支持这项统计。

**Lagging indicator:** Review time per PR (from the PR metadata), which should fall once the tests catch what reviewers used to catch, and the change failure rate from an incident tracker.

**滞后指标（Lagging indicator）：** 每个 PR 的评审时间（来自 PR 元数据；一旦测试能接住评审者过去要抓的问题，它就应下降），以及来自事件跟踪器（incident tracker）的变更失败率。

### CI 中的持续评估（Continuous evals in CI）

Evals are the AI-native equivalent of stage-gate QA. In practice that means a suite that runs whenever the agent's configuration changes. When a new model is swapped in or a prompt is rewritten, the eval suite says whether the agent still does the work to the same standard.

评估（evals）相当于阶段关卡（stage-gate）式 QA 的 AI 原生版本。在实践中，这意味着一套只要代理的配置发生变化就会运行的套件。当换入新模型或重写某条提示词时，评估套件会说明代理是否仍能以同样的标准完成工作。

The evals should be seen as a live suite. As models improve, cases that once discriminated stop doing so and new ones must be added that arise from ongoing monitoring.

应当把评估视为一套不断演进的"活的"套件。随着模型进步，曾经有区分度的用例会逐渐失效，必须根据持续监控中暴露的问题补充新用例。

Depending on the use case, some teams may prefer to run these evals offline on a set cadence rather than on every change. The steps below are for continuous evaluations.

视用例而定，一些团队可能更愿意按固定节奏离线运行这些评估，而不是每次变更都跑。下面的步骤针对的是持续评估。

**快速上手（Getting started）**

**Prerequisites:** The `CLAUDE.md` and feedback loop (Stage 4: Test).

**前置条件：** `CLAUDE.md` 与反馈回路（第 4 阶段：测试）。

**Infrastructure:** CI that can run Claude Code non-interactively, and an API key with budget for eval runs.

**基础设施：** 能够非交互式运行 Claude Code 的 CI，以及一个为评估运行留有预算的 API 密钥。

#### 如何执行（How to execute it）

1. The platform engineer collects 20 to 50 real tasks from recent work with its expected/accepted outcome.

2. Write each task as an eval, meaning the prompt plus the checks that define acceptable (tests pass, lint clean, behavior unchanged, policy followed).

3. The suite runs non-interactively in CI on a schedule and on any change to `CLAUDE.md`, skills or hooks, since that configuration steers the agent and deserves the regression testing that code gets.

4. Gate configuration changes on the results. A skill change that drops the pass rate gets reviewed before it merges.

5. Each production incident gets an eval, written by the team that owned the incident, and stays in the suite as a regression test.

1. 平台工程师从近期工作中收集 20 到 50 个真实任务，连同各自预期/可接受的结果。

2. 把每个任务写成一条评估（eval）：提示词，加上定义"可接受"的检查项（测试通过、lint 干净、行为不变、策略得到遵循）。

3. 该套件在 CI 中以非交互方式按计划运行，并在 `CLAUDE.md`、skills 或 hooks 发生任何变更时运行，因为这些配置引导着代理的行为，理应获得与代码同等的回归测试。

4. 用评估结果为配置变更把关：导致通过率下降的 skill 变更必须先经过评审才能合并。

5. 每起生产事故都会转化为一条评估，由负责该事故的团队撰写，并作为回归测试留在套件中。

#### 实际样子（What it looks like (.github/workflows/agent-evals.yml)）

```yaml
name: Agent evals
on:
  pull_request:
    paths: ['CLAUDE.md', '.claude/**']
  schedule:
    - cron: '0 2 * * *'
jobs:
  evals:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install -g @anthropic-ai/claude-code
      - name: Run eval suite
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          for eval in evals/*.json; do
            claude -p "$(jq -r '.prompt' $eval)" \
              --allowedTools "Read,Edit,Bash(make test)" \
              --output-format json > result.json
            ./evals/check.sh "$eval" result.json
          done
```

#### 治理考量（Governance considerations）

Evals give QA a gate that keeps up with agent output. The pass-rate threshold is enforced as a merge check, runs are logged so results can be compared over time, and the team that owns the configuration change approves it.

评估为 QA 提供了一个能跟上代理产出速度的门禁。通过率阈值作为合并检查强制执行；每次运行都有日志记录，结果可以跨时间比较；配置变更由其归属团队批准。

**如何衡量（How to measure it）**

**Leading indicator:** The eval pass rate over time, reported by the suite on every run, and how long a production incident takes to become a permanent eval.

**领先指标（Leading indicator）：** 评估通过率随时间的变化（套件每次运行都会报告），以及一起生产事故需要多久才能固化为一条永久评估。

**Lagging indicator:** Regressions caught in CI compared with regressions found in production derived from the incident tracker.

**滞后指标（Lagging indicator）：** 在 CI 中拦下的回归与在生产中发现的回归之比，数据来自事件跟踪器。

## 部署（Deploy）

Review runs in both directions, and governance is enforced as the agent acts. The agent does everything up to the production gate and nothing past it.

评审双向进行，治理在代理行动的同时得到执行。代理会做完生产门禁之前的所有事，绝不越过它半步。

### PR 评审回路中的 AI（AI in the PR review loop）

Claude both gives and receives reviews. It reviews incoming PRs against the organization's policies and addresses review comments on its own PRs. This allows engineers to focus on behavior in their PR review, which boils down to judging intent and risk.

Claude 既做评审也接受评审。它依据组织的策略评审收到的 PR，并处理自己 PR 上的评审意见。这让工程师在 PR 评审中可以聚焦于行为本身，而这归根结底就是判断意图与风险。

**Traditional:** Review capacity was planned around human output. A PR waits for a reviewer to read all of it, review quality varies with the reviewer's load, and the author chases while the backlog grows.

**传统方式：** 评审产能围绕人类的产出规划。一个 PR 要等评审者通读全部内容，评审质量随评审者的负载起伏，作者在一旁催促，积压却越堆越高。

**AI-native:** All PRs get an identical set of review passes, with findings ranked by severity. Human attention moves up a level, to whether the change does what the plan intended and whether the risk is acceptable.

**AI 原生方式：** 所有 PR 都得到同一套评审环节（review passes），发现的问题按严重程度排序。人的注意力上移一层，去关注变更是否实现了计划意图、风险是否可以接受。

**快速上手（Getting started）**

**Prerequisites:** An updated `CLAUDE.md` file from Stage 3: Build; skills if the review passes enforce written policies, defined subagents.

**前置条件：** 来自"第 3 阶段：构建"的更新版 `CLAUDE.md` 文件；如果各评审环节要执行成文策略，还需要 skills，以及已定义的 subagents。

**Infrastructure:** A repo with the Claude integration installed, either the managed Code Review (research preview) service enabled by an admin or the claude-code-action running in your own CI, with model calls through AWS Bedrock, Google Vertex or Microsoft Foundry where needed (the CI/CD play covers the deployment options). Branch protection policies that require a code owner's approval are also worthwhile.

**基础设施：** 一个装好 Claude 集成的仓库：要么使用由管理员启用的托管 Code Review（research preview）服务，要么在你自己的 CI 中运行 claude-code-action，并在需要时通过 AWS Bedrock、Google Vertex 或 Microsoft Foundry 调用模型（部署选项见 CI/CD 打法）。要求代码所有者批准的分支保护策略同样值得配置。

#### 如何执行（How to execute it）

1. The managed Code Review service is the fastest start. An admin enables it and selects repositories. Run the review in your own CI with the claude-code-action when you need control of the pipeline or want API calls routed through your own cloud agreement (the CI/CD play covers that plumbing).

2. The tech lead writes the review policy as `REVIEW.md` at the repo root, divided into the passes the organization cares about: bugs and logical errors; security and vulnerabilities; compliance against the spec (`spec.md` from the requirements play), the implementation plan (`plan.md` from the plan mode play) and design principles. `REVIEW.md` also defines what counts as Important as opposed to a Nit, and what to skip.

3. The tech lead sets the human threshold. Findings do not approve or block a PR on their own, and branch protection still requires approval from a code owner. A platform engineer who wants to gate merges on findings can read the severity counts that the check run publishes as a machine-readable tally.

4. When a reviewer or the author tags `@claude` on a review comment, Claude addresses the comment and pushes the fix. The PR thread records both the request and the change. This fix loop runs through the claude-code-action. In the managed service, commenting `@claude review` requests a fresh review instead. For PRs Claude opened, go further and let Claude babysit the PR to merge. Teams wrap the loop in a custom slash command that sweeps the unresolved review comments and failing checks on the PR, addresses them and pushes the fixes, until the PR is green and waiting only on code owner approval.

5. Review findings feed back into `CLAUDE.md`. When a review flags a mistake for the second time, the correction goes into `CLAUDE.md` as part of that review, and because review reads `CLAUDE.md` the mistake is caught from the next PR onwards. Review also flags when a change has made `CLAUDE.md` outdated.

6. Once a month the tech lead tunes the setup by rating findings so the reviewer improves and by capping Nit volume in `REVIEW.md`. Generated paths and anything CI already enforces are excluded.

1. 托管 Code Review 服务是上手最快的方式：由管理员启用并选择仓库。当你需要掌控流水线，或希望 API 调用走你自己的云协议时，就用 claude-code-action 在自己的 CI 中运行评审（相关管道搭建见 CI/CD 打法）。

2. 技术负责人把评审策略写成仓库根目录下的 `REVIEW.md`，按组织关心的环节划分：缺陷与逻辑错误；安全与漏洞；对照规格（来自需求打法的 `spec.md`）、实施计划（来自 plan mode 打法的 `plan.md`）和设计原则做合规检查。`REVIEW.md` 还定义了什么算 Important（重要问题）而非 Nit（琐碎问题），以及哪些应当跳过。

3. 技术负责人设定人工介入的阈值。评审发现本身既不批准也不阻塞 PR，分支保护仍然要求代码所有者的批准。想以评审发现作为合并门禁的平台工程师，可以读取 check run 以机器可读清单形式发布的严重度计数。

4. 当评审者或作者在评审评论中 @claude 时，Claude 会处理该评论并推送修复。PR 讨论串同时记录了请求与改动。这个修复回路通过 claude-code-action 运行。在托管服务中，评论 `@claude review` 则会请求一次全新的评审。对于 Claude 自己打开的 PR，还可以更进一步，让 Claude 保姆式跟进直到合并。团队可以把这个回路包进一个自定义斜杠命令：扫清 PR 上未解决的评审评论和失败的检查，逐项处理并推送修复，直到 PR 变绿、只欠代码所有者的批准。

5. 评审发现会回流到 `CLAUDE.md`。当评审第二次标记同一个错误时，更正就作为该次评审的一部分写入 `CLAUDE.md`；由于评审会读取 `CLAUDE.md`，从下一个 PR 起这个错误就会被抓住。评审还会在某次变更使 `CLAUDE.md` 过时时发出提示。

6. 每月一次，技术负责人通过给评审发现评分来促使评审者改进，并在 `REVIEW.md` 中为 Nit 数量设置上限，以此调优整套配置。生成的路径以及 CI 已经强制的内容被排除在外。

#### 实际样子（What it looks like (REVIEW.md)）

```markdown
# Review instructions

## Passes
Run three passes and tag each finding with its pass:
- Bugs: logic errors, broken edge cases, subtle regressions
- Security: injection risks, authentication gaps, PII in logs
- Compliance: the change matches spec.md, plan.md and our design principles

## What Important means here
Reserve Important for findings that would break behavior, leak data
or breach a policy. Style and naming are nits.

## Cap the nits
Report at most five nits per review; summarize the rest as a count.

## Do not report
Generated files under src/gen/ and anything CI already enforces.
```

#### 治理考量（Governance considerations）

Separation of duties is preserved, because the agent that wrote the code has no way to approve it. The review policy in `REVIEW.md` is applied to all PRs, and findings, fixes, ratings and approvals are logged in the PR history, so the PR is the audit record. Approval comes from a human through branch protection, informed by the findings.

职责分离得以保留，因为写代码的代理无权批准它。`REVIEW.md` 中的评审策略适用于所有 PR，发现、修复、评分与批准都记录在 PR 历史中，PR 本身就是审计记录。批准由人通过分支保护作出，并以评审发现作为依据。

For how these controls compose at production scale, see [securing an AI-native SDLC at Anthropic](https://claude.com/blog/how-anthropic-secures-its-ai-native-software-development-lifecycle).

关于这些控制在生产规模下如何组合，参见 [Anthropic 如何保护其 AI 原生 SDLC](https://claude.com/blog/how-anthropic-secures-its-ai-native-software-development-lifecycle)。

**如何衡量（How to measure it）**

**Leading indicator:** Time to first review, which should fall to minutes, and the share of review comments resolved without a human touching the branch with data stored directly on Git.

**领先指标（Leading indicator）：** 首次评审的等待时间，应缩短到分钟级；以及无须人工触碰分支即获解决的评审评论占比，数据直接存储在 Git 上。

**Lagging indicator:** Defects and vulnerabilities caught before merge set against those escaping to production, from the PR history and the incident tracker.

**滞后指标（Lagging indicator）：** 合并前被拦下的缺陷与漏洞，对比逃逸到生产环境的部分；数据来自 PR 历史与事件跟踪器。

### 作为审批门的 Hooks（Hooks as approval gates）

The build phase used hooks as guardrails, allowing or blocking actions with no human involved (Stage 3: Build). A hook can also ask, pausing the action until a specific person approves, which is what release gating needs.

构建阶段把 hooks 当作护栏使用，在没有任何人参与的情况下允许或阻止操作（第 3 阶段：构建）。hook 还可以"询问"：暂停操作，直到特定的人批准，而这正是发布门禁所需要的。

The play sits in Stage 5: Deploy because the release gate is the clearest case, but hooks are not deploy-specific: they run wherever Claude acts. For example, hooks can block edits to migrations and infra without a change ticket during Stage 3: Build, and stop the agent editing test files during a fix task in Stage 4: Test.

这个打法放在第 5 阶段：部署，因为发布门是最典型的场景，但 hooks 并非部署专属：Claude 行动之处它都可以运行。例如，hooks 可以在第 3 阶段：构建中阻止没有变更工单却修改 migrations 和 infra 的行为，也可以在第 4 阶段：测试中阻止代理在修复任务期间编辑测试文件。

**快速上手（Getting started）**

**Prerequisites:** None.

**前置条件：** 无。

**Infrastructure:** A written list of the approvals the change process requires.

**基础设施：** 一份书面清单，列明变更流程所需的各项审批。

#### 如何执行（How to execute it）

1. Engineering leadership, with change management and compliance, lists the human approval gates that must survive, such as change management sign-off, release authorization, and edits to protected paths.

2. The platform engineer expresses each gate as a hook, a script that runs before Claude acts that can allow, ask, or block.

3. Team hooks go in `.claude/settings.json` in git, and non-negotiable hooks go in managed settings owned by the platform or IT admin, where individual engineers cannot switch them off.

4. A block should explain itself, so when a hook stops an action the reason and the route to approval appear in Claude's output.

1. 工程管理层会同变更管理与合规部门，列出必须保留的人工审批门，例如变更管理签核、发布授权，以及对受保护路径的修改。

2. 平台工程师把每个门表达为一个 hook：一段在 Claude 行动之前运行的脚本，可以放行、询问或阻止。

3. 团队级 hooks 放入 git 中的 `.claude/settings.json`；不可协商的 hooks 放入由平台或 IT 管理员掌管的 managed settings，个别工程师无法将其关闭。

4. 阻止应当自带解释：当 hook 拦下某个操作时，原因和通往审批的路径会出现在 Claude 的输出里。

#### 实际样子（What it looks like (.claude/settings.json)）

```json
{
    "hooks": {
      "PreToolUse": [
        {
          "matcher": "Bash",
          "hooks": [
            { "type": "command",
              "command": "${CLAUDE_PROJECT_DIR}/.claude/hooks/production-gate.sh" }
          ]
        }
      ]
    }
}
```

#### 审批门脚本本身（And the gate itself (.claude/hooks/production-gate.sh)）

```bash
#!/bin/bash
# Production deploys require a named release authorization
cmd=$(jq -r '.tool_input.command' < /dev/stdin)
if [[ "$cmd" == *"deploy"* && "$cmd" == *"production"* ]]; then
   if [ -z "$RELEASE_APPROVAL" ]; then
     echo "Production deploys need a release authorization." >&2
     exit 2 # exit 2 blocks the action; the message goes to Claude
   fi
fi
exit 0
```

#### 治理考量（Governance considerations）

Hooks are the approval gates. The gate condition is enforced every time, for everyone. Allow and block decisions are logged with a timestamp. The gate also defines what counts as approval, whether that's an approved change ticket or the release manager's sign-off.

hooks 就是审批门。门禁条件对所有人、每一次都强制执行。放行与阻止的决定都带时间戳记录在案。这个门还定义了什么算作批准：无论那是已批准的变更工单，还是发布管理者的签核。

**实战示例（Worked example）**

### 受监管企业的托管设置（Managed settings for a regulated enterprise）

Deployed by the platform team via MDM or the admin console; engineers cannot edit or override any of it.

由平台团队通过 MDM 或管理控制台部署；工程师不能编辑或覆盖其中任何一项。

```json
{
  "permissions": {
     "deny": [
        "Read(.env*)", "Read(./secrets/**)",
        "WebFetch", "Bash(curl *)", "Bash(wget *)"
     ],
     "allow": [
        "Bash(git *)", "Bash(make build)",
        "Bash(make test)", "Bash(make lint)"
     ],
     "disableBypassPermissionsMode": "disable"
  },
  "allowManagedPermissionRulesOnly": true,
  "sandbox": {
     "enabled": true,
     "failIfUnavailable": true,
     "allowUnsandboxedCommands": false,
     "network": { "allowedDomains": ["git.internal.example.com",
"registry.npmjs.org"] },
     "credentials": {
        "files": [
          { "path": "~/.ssh", "mode": "deny" },
          { "path": "~/.aws/credentials", "mode": "deny" }
        ],
        "envVars": [ { "name": "GITHUB_TOKEN", "mode": "deny" } ]
     }
  },
  "allowManagedHooksOnly": true,
  "disableSideloadFlags": true,
  "allowManagedMcpServersOnly": true,
  "strictKnownMarketplaces": [
     { "source": "github", "repo": "example-corp/approved-plugins" }
  ],
  "requiredMinimumVersion": "2.1.193"
}
```

**每项配置在控制层面换来了什么（What each line buys, in control terms）**

`permissions.deny` keeps secrets out of the agent's context and blocks arbitrary network egress through tools;

`permissions.deny` 把密钥挡在代理的上下文之外，并阻止通过工具进行的任意网络外联（egress）；

`permissions.allow` pre-approves the safe inner loop so the deny list doesn't turn into prompt fatigue.

`permissions.allow` 预先放行安全的内循环操作，避免 deny 列表演变成提示疲劳（prompt fatigue）。

`disableBypassPermissionsMode` plus `allowManagedPermissionRulesOnly` means no engineer, project file or command-line flag can widen the rules.

`disableBypassPermissionsMode` 加上 `allowManagedPermissionRulesOnly`，意味着任何工程师、任何项目文件或任何命令行旗标都无法放宽这些规则。

`sandbox` closes the gap permissions cannot. A tool-level deny on WebFetch doesn't stop a shell command reaching the network; the OS-level domain allowlist blocks egress outright.

`sandbox` 弥补了权限覆盖不到的缺口。在工具层面 deny 掉 WebFetch 并不能阻止 shell 命令访问网络；操作系统级的域名白名单则直接切断外联。

`failIfUnavailable` and `allowUnsandboxedCommands` make the sandbox a gate: Claude Code refuses to start when the sandbox cannot initialize, and a command that fails inside the sandbox cannot be retried outside it.

`failIfUnavailable` 与 `allowUnsandboxedCommands` 让沙箱成为一道门：沙箱无法初始化时 Claude Code 拒绝启动；在沙箱内失败的命令，也不能转到沙箱外重试。

`credentials` closes the gap the deny rules leave open. `permissions.deny` governs Claude's file tools, but a sandboxed shell command could still read `~/.ssh` or `~/.aws/credentials` by default; this block denies those reads and strips the named secrets from the environment of every sandboxed command.

`credentials` 堵上了 deny 规则留下的缺口。`permissions.deny` 管的是 Claude 的文件工具，但默认情况下，沙箱中的 shell 命令仍可能读到 `~/.ssh` 或 `~/.aws/credentials`；这一配置块拒绝这些读取，并把列出的密钥从每条沙箱命令的环境中剥离。

`allowManagedHooksOnly` means the approval gates from this play are the only hooks that run; nothing local can add to or replace them.

`allowManagedHooksOnly` 意味着本打法的审批门是仅有的会运行的 hooks；本地任何东西都无法增补或替换它们。

`disableSideloadFlags` and `strictKnownMarketplaces` mean every skill, agent, hook and MCP server on an engineer's machine arrived through the organization's approved plugin marketplace, never from a home directory.

`disableSideloadFlags` 与 `strictKnownMarketplaces` 意味着工程师机器上的每一个 skill、agent、hook 和 MCP 服务器都是经由组织批准的插件市场安装的，绝不会来自个人主目录。

`allowManagedMcpServersOnly` makes the agent's tool surface an allowlist owned by the platform team.

`allowManagedMcpServersOnly` 让代理的工具面成为一份由平台团队掌控的白名单。

`requiredMinimumVersion` refuses to start on a version below the approved floor, so the controls are enforced by a build the organization has actually assessed.

`requiredMinimumVersion` 拒绝在低于批准下限的版本上启动，因此这些控制由组织真正评估过的版本来落实。

code.claude.com/docs/en/settings

**如何衡量（How to measure it (for the hooks themselves)）**

**Leading indicator:** Time spent waiting on each approval gate. Every hook decision is written to the OpenTelemetry export with a timestamp and an allow or block verdict, so the wait is visible per gate.

**领先指标（Leading indicator）：** 在每个审批门上花费的等待时间。每个 hook 决定都会连同时间戳和放行/阻止的裁决写入 OpenTelemetry 导出，因此每个门的等待时间都清晰可见。

**Lagging indicator:** Gate violations reaching production before and after hooks from the incident tracker.

**滞后指标（Lagging indicator）：** 引入 hooks 前后抵达生产环境的门禁违规数量，数据来自事件跟踪器。

### CI/CD 集成与部署（CI/CD integration and deployment）

Run Claude Code non-interactively inside the CI/CD pipeline, sandbox the execution so long-running agents run safely, expose deployment through MCP integrations, and rehearse the rollback paths before the agent ever needs them.

在 CI/CD 流水线中非交互式地运行 Claude Code，为执行加上沙箱（sandbox）使长时间运行的代理安全作业，通过 MCP 集成暴露部署能力，并在代理真正需要之前就演练好回滚路径。

**Traditional:** Pipelines run deterministic scripts, and anything that needs judgment waits for a human. For example, triaging the flaky test, writing the changelog, or working out why the build broke. Deployment and rollback are runbooks a human follows under pressure.

**传统方式：** 流水线运行确定性脚本，任何需要判断的事都要等人。例如分诊（triage）不稳定的测试（flaky test）、撰写变更日志（changelog）、排查构建失败的原因。部署与回滚是人在压力下照着执行的运行手册（runbook）。

**AI-native:** Claude runs non-interactively inside the pipeline for the judgment steps, in a sandbox with scoped credentials. Deployment tooling is exposed to the agent through MCP, so the workflow that wrote and tested the change can also ship it and roll it back, inside gates the organization defines per environment.

**AI 原生方式：** Claude 在流水线内以非交互方式承担需要判断的步骤，运行在带有最小范围凭据（scoped credentials）的沙箱中。部署工具通过 MCP 暴露给代理，因此编写并测试了该变更的那个工作流，也能发布它、回滚它，而这一切都发生在组织为每个环境定义的门禁之内。

**快速上手（Getting started）**

**Prerequisites:** Claude in the PR review loop and hooks as approval gates, because the gates must exist before automation accelerates anything through them.

**前置条件：** PR 评审回路中的 Claude，以及作为审批门的 hooks：这些门必须先存在，自动化才能加速任何流经它们的东西。

**Infrastructure:** A CI platform with the claude-code-action installed, or any runner that can call `claude -p`; model access through the API, or Bedrock, Foundry, or Vertex where traffic must stay on the organization's cloud agreement; MCP servers for the deployment targets; a sandbox profile for agent jobs with no standing production credentials.

**基础设施：** 装有 claude-code-action 的 CI 平台，或任何能调用 `claude -p` 的 runner；模型访问经由 API，或在流量必须留在组织云协议之内时经由 Bedrock、Foundry 或 Vertex；面向各部署目标的 MCP 服务器；以及一个面向代理作业、不持有常驻生产凭据的沙箱配置（sandbox profile）。

#### 如何执行（How to execute it）

1. The platform engineer starts with read-only judgment steps. Use `claude -p` in a pipeline job to triage a failed build, summarize a flaky test, or draft the changelog.

2. Add write steps behind the existing gates for jobs like fixing lint, updating generated docs, or addressing review comments via the `@claude` mentions. Anything the agent writes arrives as a PR through branch protection, and the agent has no route to push to main.

3. Execution is sandboxed. Agent jobs run in containers under a network policy with short-lived scoped tokens, and hold no production credentials by default.

4. Expose deployment through MCP. Deploy, status, and rollback become tools, scoped per environment, so the agent's deployment powers are an allowlist rather than a shell script with credentials.

5. Tier the autonomy by environment. In development, the agent deploys freely. In production, the agent prepares the release and the release manager authorizes it, and a hook enforces the production gate. Staging sits somewhere in the middle.

6. Rollback should be the most rehearsed path in the pipeline, a single command that the agent can run and that is exercised regularly in staging. The closing the loop play (Stage 6: Maintenance) calls this rollback when a control band is breached, so it has to be proven in advance.

1. 平台工程师从只读的判断类步骤开始。在流水线作业中用 `claude -p` 来分诊失败的构建、总结不稳定的测试或起草变更日志。

2. 在现有门禁之后加入写入类步骤，用于修复 lint、更新生成的文档，或通过 @claude 提及处理评审评论。代理写下的任何东西都以 PR 形式、经分支保护到达，代理没有任何直推 main 的途径。

3. 执行处于沙箱之中。代理作业在容器内、网络策略之下运行，使用短时效的最小范围令牌（scoped tokens），默认不持有任何生产凭据。

4. 通过 MCP 暴露部署能力。部署、状态查询与回滚都成为工具，并按环境划定范围，这样代理的部署权限就是一份白名单，而不是一个带着凭据的 shell 脚本。

5. 按环境划分自治级别。在开发环境中，代理可以自由部署；在生产环境中，代理准备发布、由发布管理者授权放行，并由一个 hook 强制执行生产门禁。预发布（staging）介于两者之间。

6. 回滚应当是流水线中演练得最充分的路径：一条代理就能执行的单一命令，并在预发布环境中定期演练。闭环打法（第 6 阶段：维护）会在控制带（control band）被突破时调用这个回滚，所以它必须事先经过验证。

#### 实际样子（What it looks like (pipeline step)）

```markdown
- name: Triage failed build
  if: failure()
  run: >
    claude -p "Read the build log at out/build.log. Identify the most
    likely cause, say whether the failure looks flaky or real, and write a
    three-line summary for the PR thread." >> triage.md
```

#### 治理考量（Governance considerations）

The governing principle is that the agent may act up to the production gate and cannot pass it. The controls below enforce this principle.

治理原则是：代理可以一直行动到生产门禁为止，但不能越过它。下列控制负责落实这一原则。

- Branch protection turns anything the agent writes into a PR, with no direct path to main.

- The production deploy hook blocks the release until a named release manager authorizes it. Each non-interactive run acts under the agent's own identity, so the pipeline log separates what the agent did from what the engineer who triggered it did.

- Per-environment permission tiers set how much the agent may do on the way to the gate.

- 分支保护把代理写下的任何东西都变成 PR，没有直通 main 的路径。

- 生产部署 hook 会阻止发布，直到指定姓名的发布管理者（release manager）授权为止。每次非交互运行都以代理自己的身份行事，因此流水线日志能把代理做的事与触发它的工程师做的事区分开。

- 按环境划分的权限层级决定了代理在通往门禁的路上能做多少事。

**如何衡量（How to measure it）**

**Leading indicator:** The share of pipeline failures triaged without paging a human taken from the CI/CD pipeline logs.

**领先指标（Leading indicator）：** 无须呼叫人工即完成分诊的流水线失败占比，数据取自 CI/CD 流水线日志。

**Lagging indicator:** DevOps Research and Assessment (DORA) measures, which the CI system and deployment tooling already emit.

**滞后指标（Lagging indicator）：** DevOps Research and Assessment（DORA）度量，CI 系统与部署工具本身已经在输出这些数据。

## 维护（Maintain）

The loop closes. A trigger invokes Claude with no person in the invocation path, and what it finds re-enters the pipeline as `intent.md`.

回路就此闭合。一个触发器调用 Claude，调用路径上没有任何人；它发现的东西会以 `intent.md` 的形式重新进入流水线。

### 维护与闭环（Maintenance and closing the loop）

So far, we've discussed how to add Claude to each stage of the SDLC process, with each stage requiring a human to launch the initial steps. This stage, however, shifts the focus to autonomous running of Claude to close the loop.

到目前为止，我们讨论的都是如何把 Claude 加进 SDLC 流程的各个阶段，而每个阶段都需要人来启动最初的步骤。本阶段则把焦点转向 Claude 的自主运行，让回路闭合。

For example, a continuously running monitoring agent could, off the back of a bug ticket being raised, create an `intent.md`, and flow through the requirements, plan, build test and review phases. Stage 6: Maintenance runs headless, with an independent confidence gate between stages, a deterministic check or an adversarial reviewing agent, deciding whether the previous stage's output continues or is escalated to a human.

例如，一个持续运行的监控代理可以在缺陷工单被提交后顺势创建 `intent.md`，然后流经需求、计划、构建、测试与评审各阶段。第 6 阶段：维护以 headless（无人值守）方式运行，阶段之间设有独立的置信门（confidence gate）：由一个确定性检查或一个对抗性评审代理，来决定上一阶段的产出是继续流转，还是升级给人处理。

**Traditional:** Maintenance is a reactive phase. All tickets or incidents wait on a person to act on it and restart the process. An alert fires at 3 a.m. and can be missed, a ticket can sit in the backlog until someone picks it up, and post-mortem actions may not reach the codebase at all if another fire starts first.

**传统方式：** 维护是一个被动响应的阶段。所有工单或事故都在等人处理、等人重启流程。凌晨 3 点触发的告警可能被错过；工单可能躺在积压清单里，直到有人接手；而如果另一场火先烧起来，复盘（post-mortem）行动可能根本到不了代码库。

**AI-native:** A trigger such as a control-band breach, a ticket, a channel message or a schedule invokes Claude without a person in the path. Claude diagnoses, acts only through gated routes, and writes what it finds as `intent.md`, which then goes through the stages described above. People triage and review that work, and no longer have to start it.

**AI 原生方式：** 控制带被突破（control-band breach）、一张工单、一条频道消息或一个定时计划，都可以作为触发器，在无人在路径中的情况下调用 Claude。Claude 进行诊断，只经由设有门禁的路径行动，并把发现写成 `intent.md`，随后走完上述各阶段。人来分诊和评审这些工作，而不再需要由人来启动。

### 闭环（Closing the loop）

A deterministic script watches production and invokes Claude when a control band is breached. Monitoring of a breach is a helpful example of the pattern for the loop running autonomously, while the [Claude Tag](https://claude.com/product/tag) (public beta) section at the end of the stage covers work arriving through different channels.

一个确定性脚本监视生产环境，并在控制带被突破时调用 Claude。对突破的监控是回路自主运行这一模式的一个实用示例，而本阶段末尾的 [Claude Tag](https://claude.com/product/tag)（公开测试版）一节，则讨论经由不同渠道到达的工作。

**快速上手（Getting started）**

**Prerequisites:** `intent.md`, which gives the loop a structured output to restart. Claude-accelerated PR reviews, hooks as an action boundary, and a rollback path for CI/CD (which the highest autonomy tier invokes).

**前置条件：** 可为回路提供结构化输出以便重启的 `intent.md`；由 Claude 加速的 PR 评审；作为行动边界的 hooks；以及 CI/CD 的回滚路径（最高自治层级会调用它）。

**Infrastructure:** Agent SDK

**基础设施：** Agent SDK

#### 如何执行（How to execute it）

1. The service owner or platform engineer picks one metric with a stable rolling baseline, such as CI test failure rate, post-deploy 5xx rate, or PR cycle time.

2. They write the detection script, typically mean and standard deviation over a rolling window with rules (Western Electric or similar) so the bands catch slow drift as well as spikes. The script is version controlled and unit tested, and detection stays entirely deterministic, with no model involved.

3. Response tiers are defined in version-controlled config (`bands.yaml` below). At 1σ the script only logs, at 2σ it invokes Claude read-only to diagnose, and at 3σ Claude may act, though only by opening a PR into the review gate or triggering a pre-approved runbook.

4. The trigger layer can be a scheduled workflow in GitHub or GitLab, a webhook from the existing monitoring stack, or a Cron Job inside the network. Claude runs stateless, either as a non-interactive step on a CI runner or as an Agent SDK service in a sandboxed container, and the CI/CD play covers the deployment and model-access options. Because the run is stateless and non-interactive, a loop can begin and end without anyone starting it.

5. The agent writes its diagnosis as `intent.md` in the Stage 1: Plan format, covering the anomaly and its evidence, a proposed outcome, the affected systems and any open questions. From there the finding goes through the pipeline like anything else.

6. The service owner or on-call engineer triages the queue, routing product-facing findings to the product owner. Fix now, schedule, or dismiss. Dismissals tune the bands and help to reduce noise.

7. When a fix ships, add an eval for the incident (the continuous evals play) to ensure that such issues are protected against going forwards.

1. 服务负责人或平台工程师挑选一个具有稳定滚动基线（rolling baseline）的指标，例如 CI 测试失败率、部署后 5xx 错误率或 PR 周期时长。

2. 他们编写检测脚本：通常是在滚动窗口上计算均值与标准差，并辅以规则（Western Electric 或类似规则），让控制带既能捕捉缓慢漂移也能捕捉突刺。脚本纳入版本控制并配有单元测试，检测完全保持确定性，不涉及任何模型。

3. 响应层级定义在纳入版本控制的配置中（见下面的 `bands.yaml`）。1σ 时脚本只记录日志；2σ 时以只读方式调用 Claude 进行诊断；3σ 时 Claude 可以行动，但只能通过向评审门提交 PR 或触发预先批准的运行手册。

4. 触发层可以是 GitHub 或 GitLab 中的定时工作流、来自现有监控栈的 webhook，或网络内部的 Cron Job。Claude 以无状态方式运行：或作为 CI runner 上的非交互步骤，或作为沙箱容器中的 Agent SDK 服务；部署与模型访问选项见 CI/CD 打法。由于运行无状态且非交互，一个回路可以在没有任何人启动的情况下开始并结束。

5. 代理按第 1 阶段：计划（Stage 1: Plan）的格式把诊断写成 `intent.md`，涵盖异常及其证据、建议达成的结果、受影响的系统以及所有未决问题。从此，这份发现就像其他任何东西一样流经流水线。

6. 服务负责人或值班工程师对队列进行分诊，把面向产品的发现转给产品负责人：立即修复、排期，或驳回。驳回操作会调优控制带，帮助降低噪音。

7. 修复上线后，为这起事故添加一条评估（见持续评估打法），确保今后对这类问题有所防护。

#### 实际样子（What it looks like (for example, a bands.yaml monitoring CI test failure rate)）

```yaml
metric: ci_test_failure_rate
baseline: rolling_30d
rules: western_electric
tiers:
  1sigma: { action: log }
  2sigma: { action: diagnose,
            tools: "Read,Grep,Bash(gh run view *)" }
  3sigma: { action: propose,
            routes: [pull_request, runbook:rollback-deploy] }
```

#### 治理考量（Governance considerations）

The tier boundaries are enforced from version-controlled config, with permissions and managed settings denying production access. Invocations, findings and triage decisions are logged with a timestamp. A service owner triages and approves findings, resulting changes go through the normal PR review gate, and the runbooks the agent may trigger were approved in advance.

层级边界由纳入版本控制的配置强制执行，permissions 与 managed settings 拒绝生产环境访问。调用、发现与分诊决定都带时间戳记录在案。服务负责人对发现进行分诊和批准；由此产生的变更走正常的 PR 评审门；代理可以触发的运行手册均已事先获批。

**如何衡量（How to measure it）**

**Leading indicator:** Time from band breach to an `intent.md` in the triage queue, against the old time from incident to post-mortem action. The detection script's log has the breach timestamp and tier of incident.

**领先指标（Leading indicator）：** 从控制带被突破到 `intent.md` 进入分诊队列的时间，对照旧模式下从事故到复盘行动的时间。检测脚本的日志记录了突破时间戳与事故层级。

**Lagging indicator:** The share of findings that become merged fixes (triage queue against actual PR history), and repeat incidents of the same class, which should fall as the fixes add cases to the eval suite.

**滞后指标（Lagging indicator）：** 发现最终变成已合并修复的比例（用分诊队列对照实际 PR 历史），以及同类事故的重复发生次数，随着修复不断为评估套件补充用例，后者应当下降。

#### 示例（Examples）

- When the CI test failure rate breaches 3σ, the agent quarantines the flaky test or opens a revert PR, and the review gate decides.

- When the post-deploy 5xx rate breaches 3σ with a deployment in the window, the agent triggers the existing rollback pipeline.

- When PR cycle time trips a drift rule, the agent writes a report for engineering leadership, which shows the harness works for process metrics as well as production ones.

- 当 CI 测试失败率突破 3σ 时，代理隔离那条不稳定的测试或打开一个回滚（revert）PR，由评审门裁决。

- 当部署后 5xx 错误率突破 3σ 且时间窗口内存在部署时，代理触发既有的回滚流水线。

- 当 PR 周期时长触发漂移规则时，代理会为工程管理层撰写一份报告，这说明这套机制对过程指标和生产指标同样有效。

Detection stays deterministic. Claude is invoked once a band is breached, and the tier sets what it may do.

检测保持确定性。控制带一旦被突破就调用 Claude，而层级决定它可以做什么。

### 定期代码库扫描（Recurring codebase scans）

A security scan is a point-in-time statement about a codebase under a particular model, and both halves go stale: the code changes every week, and each model generation finds vulnerabilities the previous one missed. The AI-native answer is to run the scan on a schedule, without a human in the invocation path, and to send what it finds through the same gates as any other change to the codebase.

一次安全扫描，只是特定模型在某个时点对代码库给出的判断，而这两半都会过期：代码每周都在变，每一代模型都能发现上一代漏掉的漏洞。AI 原生的答案是按计划定期运行扫描，调用路径上没有人类，并把扫描发现送进与代码库其他变更相同的门禁。

[Claude Security](https://claude.com/product/claude-security) is the hosted form of scheduled scanning. Connect a GitHub repository, and scans run on Claude Mythos 5 in Anthropic's infrastructure, with each finding validated before it is reported and a confidence rating attached. Suggested patches are reviewed and applied in Claude Code on the web. The organization gets the findings without needing access to the model itself.

[Claude Security](https://claude.com/product/claude-security) 是定期扫描的托管形态。连接一个 GitHub 仓库，扫描就会在 Anthropic 的基础设施上以 Claude Mythos 5 运行，每条发现都在报告之前经过验证，并附有置信度评级（confidence rating）。建议的补丁在网页版 Claude Code 中评审并应用。组织无须接触模型本身，即可获得发现结果。

**Traditional:** Security scanning is an event with a scan launched before a release or an audit. The report goes to a tracker, and the backlog is worked down by hand until the next event. Code written in between is covered by whatever the PR review caught.

**传统方式：** 安全扫描是一项"活动"：在发布或审计之前启动一次扫描。报告进入跟踪器，积压清单靠人力慢慢消化，直到下一次活动。期间写下的代码，只能靠 PR 评审碰巧抓到的东西来覆盖。

**AI-native:** Scans run on a schedule against every connected repository, on the most capable model available, with findings validated before anyone reads them. Each finding is handled the way a breached control band is: a fix that fits in one PR goes through the review gate, and anything larger becomes an `intent.md`. Coverage is dated from the last run, not from the first

**AI 原生方式：** 扫描按计划对所有已连接的仓库运行，使用可用的最强模型，发现先经验证、再供人阅读。每条发现的处理方式与控制带被突破时相同：能放进一个 PR 的修复走评审门，更大的则写成一份 `intent.md`。覆盖情况的时间戳以最后一次运行为准，而不是第一次。

**快速上手（Getting started）**

**Prerequisites:** The PR review gate and hooks as approval gates (Stage 5: Deploy), so that findings go through review like any other change. The `intent.md` format from Stage 1: Plan for findings too large for a single PR.

**前置条件：** PR 评审门与作为审批门的 hooks（第 5 阶段：部署），让发现像其他任何变更一样经过评审；以及来自第 1 阶段：计划的 `intent.md` 格式，用于超过单个 PR 容量的发现。

**Infrastructure:** Claude Security is available to Claude Enterprise organizations in public beta. It needs the Anthropic GitHub App installed on the target repositories (cloud-hosted github.com), Claude Code on the Web enabled, Extra Usage turned on with a spend limit set, premium seats for the people who run scans, and the feature switched on by an admin at claude.ai/admin-settings/claude-code. Scans are billed on consumption at Mythos 5 rates, so the spend limit should match the size and number of repositories.

**基础设施：** Claude Security 面向 Claude Enterprise 组织开放公开测试版。它要求：在目标仓库上安装 Anthropic GitHub App（云托管的 github.com）、启用网页版 Claude Code（Claude Code on the Web）、开启 Extra Usage 并设置消费上限、为运行扫描的人准备 premium 席位，并由管理员在 claude.ai/admin-settings/claude-code 打开该功能。扫描按消费计费、采用 Mythos 5 费率，因此消费上限应与仓库的规模和数量相匹配。

#### 如何执行（How to execute it）

1. The security lead connects the repositories and organizes them into projects by repo, service, or team, so ownership of findings is clear from the start.

2. Run a first full scan of the most critical repositories, including ones that have been scanned before by other tools or by earlier models. Treat the first scan as the baseline. The first scan will likely surface findings in code that was considered clean.

3. Set a schedule per project. Weekly is a sensible default for actively developed services; scope scans to a directory or branch where a repository is large or mixed.

4. Triage findings with the confidence rating in hand. Dismiss with a reason, so the dismissal is recorded and the same finding does not return as new on the next run.

5. For a bounded finding, open the suggested patch in Claude Code on the Web, review it, and send it through the PR review gate like any other change. The agent that proposed the fix has no route to approve it.

6. For anything wider than one patch, such as an architectural weakness or a pattern repeated across services, write it up as `intent.md` in the Stage 1 format and start it at Plan.

7. When a fix is released to production, add an eval for the vulnerability class to the suite from the continuous evals play, so the configuration that steers the agent is tested against that class from then on.

8. Export findings as CSV or Markdown, or use webhooks, to keep the organization's existing tracker and audit systems as the system of record where auditors already expect them.

1. 安全负责人连接各仓库，并按仓库、服务或团队把它们组织成项目，让发现的归属从一开始就清晰明确。

2. 对最关键的仓库运行首次全量扫描，包括此前被其他工具或更早模型扫描过的仓库。把首次扫描当作基线。首次扫描很可能在那些被认为干净的代码中发现问题。

3. 为每个项目设置扫描计划。对活跃开发的服务，每周一次是合理的默认值；当仓库很大或混合时，把扫描范围限定到某个目录或分支。

4. 带着置信度评级对发现进行分诊。驳回时要写明理由，这样驳回会被记录在案，同一条发现也不会在下次运行中再次以"新发现"的面目出现。

5. 对于边界清晰的发现，在网页版 Claude Code 中打开建议的补丁，评审后与其他任何变更一样送入 PR 评审门。提出修复的代理没有任何批准它的途径。

6. 对于超出单个补丁范围的问题，例如架构弱点或跨服务重复出现的模式，按第 1 阶段的格式写成 `intent.md`，并从计划（Plan）阶段启动。

7. 修复发布到生产后，把这一漏洞类别对应的评估加入持续评估打法的套件，从此引导代理的配置就会针对该类别接受测试。

8. 以 CSV 或 Markdown 导出发现，或使用 webhook，让组织现有的跟踪器与审计系统继续充当记录系统（system of record），留在审计者已经预期的位置。

#### 治理考量（Governance considerations）

The scan runs under the organization's admin controls meaning what repositories are connected, who holds a scan seat, and the spend limit are all set centrally. Every finding has a validation result and a confidence rating, and every dismissal has a reason, so the scan history is an audit record of what was found, fixed, and consciously accepted.

扫描在组织的管理控制之下运行：连接哪些仓库、谁持有扫描席位、消费上限是多少，全部集中设定。每条发现都有验证结果和置信度评级，每次驳回都有理由，因此扫描历史就是一份关于"发现了什么、修复了什么、有意识地接受了什么"的审计记录。

Fixes reach production through the PR review gate and branch protection rather than from the scan itself. Claude Security augments existing static analysis and dependency scanning. The deterministic checks stay in CI, and the model-driven scan covers the context-dependent vulnerabilities those checks are not built to find.

修复通过 PR 评审门和分支保护抵达生产，而不是由扫描本身发布。Claude Security 是对现有静态分析与依赖扫描的补充：确定性检查留在 CI 中，模型驱动的扫描则覆盖那些检查机制本就不打算发现的、依赖上下文的漏洞。

**如何衡量（How to measure it）**

**Leading indicator:** Share of connected repositories on a schedule, and time from a finding being reported to its patch entering the PR review gate, read from the scan history and the PR metadata.

**领先指标（Leading indicator）：** 已连接仓库中纳入定期扫描的比例，以及从发现被报告到其补丁进入 PR 评审门的时间；数据来自扫描历史与 PR 元数据。

**Lagging indicator:** Vulnerabilities found by the scheduled scan set against those found in production or by external report, from the incident tracker; and the trend in findings per scan on repositories that have been through several runs, which should fall as fixes and evals accumulate.

**滞后指标（Lagging indicator）：** 定期扫描发现的漏洞，对比生产中发现或经外部报告的漏洞（数据来自事件跟踪器）；以及在已经历多次运行的仓库上每次扫描的发现数趋势，随着修复与评估不断积累，它应当下降。

### 借助 Claude Tag 让 Claude 值班（Claude on call with Claude Tag）

Incidents can also arrive via other means such as workplace communication apps, like Slack or Teams. Incidents can look like a 10pm Slack message for an urgent fix on an incident channel and can now be actioned immediately. Claude Tag (public beta currently available in Slack) makes Claude a member of those channels under its own identity, so each new incident gets a first responder and the response itself becomes part of the loop and memory for future incidents.

事故也可以经由其他渠道到达，例如 Slack 或 Teams 这类职场沟通应用。事故可能表现为晚上 10 点事故频道里一条要求紧急修复的 Slack 消息，而现在可以立即着手处理。Claude Tag（公开测试版，目前在 Slack 中可用）让 Claude 以自己的身份成为这些频道的一员，因此每起新事故都有了一位第一响应人（first responder），响应过程本身也成为回路的一部分，以及未来事故的记忆。

The conversation and institutional knowledge stay in the channel, with anyone in the channel able to guide and action the response. Any team member can test hypotheses, explore new options and investigate in real time with the channel history adding to the auditability. Through access to MCP Claude verifies the metric is back at baseline and confirms it in the thread, writes the post-mortem to a version-controlled lessons file that future investigations can read.

对话与机构知识留在频道里，频道中的任何人都可以引导并执行响应。任何团队成员都可以实时检验假设、探索新选项并展开调查，频道历史则增强了可审计性。通过 MCP 访问，Claude 会验证指标已回到基线并在讨论串中确认，然后把复盘写成一份纳入版本控制的 lessons 文件，供未来的调查读取。

Incidents are not the only work Claude Tag picks up. Tagged on a ticket over MCP or asked in the channel, Claude triages the work the same way. A small, well-bounded fix arrives as a PR through the review gate, and anything larger is written up as `intent.md` for Stage 1: Plan, at which point the loop starts feeding itself. See: [how Claude Tag runs on-call for CI/CD at Anthropic](https://claude.com/blog/ai-ci-cd-on-call).

事故并不是 Claude Tag 接手的唯一工作。通过 MCP 在工单上被 @，或在频道里被提问，Claude 会以同样的方式对工作分诊。小而边界清晰的修复以 PR 形式经评审门到达；更大的则写成 `intent.md`、进入第 1 阶段：计划；至此，回路开始自己喂养自己。参见：[Claude Tag 如何在 Anthropic 为 CI/CD 值班](https://claude.com/blog/ai-ci-cd-on-call)。

![](images/img-04.png)

**Caption:** The channel is the audit trail: request, diagnosis, human authorization and fix all stay where the incident was handled.

**图注：** 频道就是审计轨迹：请求、诊断、人工授权与修复，全部留在事件被处理的地方。

## 结语（Closing thoughts）

Models and harnesses have become more advanced, allowing organizations to not just transform how they produce code, but the entire software development lifecycle.

模型与 harness（运行框架）已经变得更加先进，这让组织不仅能变革生产代码的方式，还能变革整个软件开发生命周期。

This transformation keeps human judgement central to the process and considers the governance and regulation requirements of large enterprise organizations.

这场转型让人的判断始终处于流程的核心，并兼顾了大型企业组织的治理与监管要求。

This guide consolidated many of the real best practices our Applied AI team executes on a daily basis for our customers, and we hope you found it a practical and actionable resource.

本指南汇集了我们的 Applied AI 团队每天为客户执行的诸多真实最佳实践，希望它对你而言是一份实用且可落地的资源。

The loop keeps running. Human judgement stays above it.

回路持续运转，而人的判断始终凌驾其上。

### 资源与致谢（Resources and acknowledgments）

The documentation below is what a platform team needs to set those controls up, in roughly the order you would roll them out.

下列文档是平台团队配置这些控制所需的内容，大致按落地推出的先后顺序排列。

Set up Claude Code for your organization — the admin decision map; start here
code.claude.com/docs/en/admin-setup

为你的组织设置 Claude Code -- 管理员决策地图；从这里开始
code.claude.com/docs/en/admin-setup

Settings reference and precedence, including every managed-only key
code.claude.com/docs/en/settings

设置参考与优先级，涵盖所有仅托管（managed-only）的键
code.claude.com/docs/en/settings

Server-managed settings from the Claude admin console
code.claude.com/docs/en/server-managed-settings

来自 Claude 管理控制台的服务器托管设置
code.claude.com/docs/en/server-managed-settings

Permissions
code.claude.com/docs/en/permissions

权限
code.claude.com/docs/en/permissions

Sandboxing — OS-level filesystem and network isolation
code.claude.com/docs/en/sandboxing

沙箱 -- 操作系统级的文件系统与网络隔离
code.claude.com/docs/en/sandboxing

Hooks — guide
code.claude.com/docs/en/hooks-guide

Hooks -- 指南
code.claude.com/docs/en/hooks-guide

Hooks — reference
code.claude.com/docs/en/hooks

Hooks -- 参考
code.claude.com/docs/en/hooks

Skills
code.claude.com/docs/en/skills

Skills
code.claude.com/docs/en/skills

Plugins and private marketplaces — how skills and hooks are distributed organization-wide
code.claude.com/docs/en/plugin-marketplaces

插件与私有市场 -- skills 与 hooks 如何在组织范围内分发
code.claude.com/docs/en/plugin-marketplaces

Managed MCP — central control of the agent's tool surface
code.claude.com/docs/en/managed-mcp

托管 MCP -- 对代理工具面的集中控制
code.claude.com/docs/en/managed-mcp

Enterprise deployment overview — Bedrock, Vertex, Foundry
code.claude.com/docs/en/third-party-integrations

企业部署概览 -- Bedrock、Vertex、Foundry
code.claude.com/docs/en/third-party-integrations

Enterprise network configuration
code.claude.com/docs/en/network-config

企业网络配置
code.claude.com/docs/en/network-config

Monitoring (OpenTelemetry)
code.claude.com/docs/en/monitoring-usage

监控（OpenTelemetry）
code.claude.com/docs/en/monitoring-usage

The analytics dashboard
code.claude.com/docs/en/analytics

分析仪表盘
code.claude.com/docs/en/analytics

Compliance API — Enterprise activity feed, chat retrieval and deletion
platform.claude.com/docs/en/manage-claude/compliance-api

合规 API -- 企业活动流、对话检索与删除
platform.claude.com/docs/en/manage-claude/compliance-api

Security model
code.claude.com/docs/en/security

安全模型
code.claude.com/docs/en/security

Thanks to Jim Blackhurst, Will Steuk, and Jamal Arif for their contributions to this guide, which was inspired by and built on much of their previous work.

感谢 Jim Blackhurst、Will Steuk 与 Jamal Arif 对本指南的贡献；本指南正是从他们此前的大量工作中获得启发，并在其基础上构建而成。

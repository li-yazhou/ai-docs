# AI 模型可能已能在无数据中心条件下自我复制（中英对照）

> 原文标题：AI models may be capable of self-replicating without access to a datacenter
> 原文链接：https://www.anthropic.com/research/self-replicating-ai
> 原文作者：William Zhou, Daniel Freeman（Anthropic）
> 发布日期：2026-10-12
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 最有分量的发现：9B 模型已在 9 类陌生硬件中的 8 类上自主跑起推理，GLM-5.2 更以 8/8 节点稳定完成端到端自我复制，而按历史进步速度，小模型能力将在 2026 年底触及复制门槛
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；文末附录 A–F 及正文引用的交互式图表未收录，见原文链接。

---

Our experiments suggest that small AI models may be months away from being capable enough to self-replicate on commercial internet-of-things (IoT) devices, like smart ovens and off-brand Android TV boxes. A replicator like this would be challenging to shut down, since there’s no centralized point to shut off—though it may be slow (with a doubling time of days). Unlike non-AI malware, it could also figure out unfamiliar devices on its own. In our evaluations, a 9B model got inference running on 8 of the 9 hardware types we tested.

我们的实验表明，小模型可能再过几个月，就具备在商用物联网（Internet of Things, IoT）设备——比如智能烤箱和杂牌 Android 电视盒子——上实现自我复制（self-replication）的能力。这样的复制体将很难被关停，因为不存在一个可以一关了之的中心节点——尽管它的传播可能很慢（翻倍时间以天计）。与 AI 出现之前的恶意软件不同，它还能自己摸清陌生设备的门道。在我们的评估中，一个 9B 模型在 9 类受测硬件中的 8 类上成功让推理跑了起来。

Models of 30B parameters and under can already complete many of the individual steps for replication today, though they make mistakes when composing them end to end. In our simplified evaluations, GLM-5.2 and stronger models are capable of end to end replication. If small models improve at their historical rate, they’ll match this capability level by the end of this year. Small weight-based replicators may be possible soon after.

300 亿（30B）参数及以下的模型，如今已能完成复制过程中的许多单个步骤，只是在把它们端到端串起来时还会出错。在我们简化的评估中，GLM-5.2 及更强的模型已能完成端到端复制。如果小模型沿历史速度继续进步，到今年年底它们就会达到这一能力水平。小型的「基于权重的复制体」（weight-based replicator）或许紧随其后就会出现。

## 引言（Introduction）

Ever-improving AI capabilities make a longstanding concern increasingly realistic: autonomous replication and adaptation (ARA), which involves a model deploying and propagating itself without human control.

AI 能力的持续精进，让一个长期存在的担忧变得越来越现实：自主复制与适应（autonomous replication and adaptation, ARA）——即模型在无人控制的情况下部署并传播自身。

In cyber, a worm is a program that copies itself from one computer to the next without human input. Prior work has already shown that AI agents can act like one: scanning their network, executing an exploit, and copying model weights to a target machine across a GPU cluster. Such an “AI worm” could carry its model weights with it as it reproduces (or re-download them from a public hub), and then run inference directly on compromised hardware. Effectively, the weights would become part of the malware payload.

在网络攻击领域，蠕虫（worm）是一种无需人工参与、就能从一台计算机自我拷贝到下一台的程序。已有研究证明，AI 智能体（agent）可以扮演这种角色：扫描所在网络、执行漏洞利用，再在 GPU 集群内把模型权重拷贝到目标机器上。这样的「AI 蠕虫」可以在繁殖时随身携带自己的模型权重（或从公共模型中心重新下载），然后在被攻陷的硬件上直接运行推理（inference）。实际上，权重将成为恶意软件载荷的一部分。

Datacenter GPUs would be a clear, high-value target for such malware. But expensive accelerators aren’t strictly necessary. In this blog post, we show that a distributed botnet of IoT devices—which are cheap and typically poorly defended—could also provide enough compute to run a capable model.

对这类恶意软件而言，数据中心 GPU 显然是高价值目标。但昂贵的加速器并非严格必需。在这篇博文中，我们要展示的是：一个由 IoT 设备组成的分布式僵尸网络（botnet）——这些设备价格低廉、防护通常薄弱——也足以提供运行一个有相当能力模型的算力。

We run Gemma4-26B-A4B, distributed over 15 inexpensive off-brand Android TV boxes (~$55 each) that we bought and networked together, at 0.39 tokens per second. Each tile below shows one physical TV box holding two of the model’s layers as the model runs.

我们把 Gemma4-26B-A4B 分布到 15 台自己购买并组网的廉价杂牌 Android 电视盒子（每台约 55 美元）上运行，速度为每秒 0.39 个 token。模型运行时，下方的每一格都对应一台物理电视盒子，每台承载着该模型的两层。

> A fleet of 15 Smart TV boxes running distributed inference of Gemma4-26B-A4B.
>
> 图注（视频见原文）：15 台智能电视盒子组成的机群，正在对 Gemma4-26B-A4B 进行分布式推理。

Of course, there are many other steps to showing that an autonomous worm can propagate itself across a network of weak machines. For the purposes of this blog post, we break down the replication chain into a few steps.

当然，要证明一个自主蠕虫能在一片防御薄弱的机器网络上自我传播，还涉及许多其他环节。就这篇博文而言，我们把复制链拆解为下面几个步骤。

![复制各阶段的流程图：网络枚举、初始访问与载荷投递（含机会性权限提升）、推理环境搭建，以及复制。前两个阶段被归为一组，标注「网络漏洞利用（非本文重点）」，推理环境搭建则标注「本文重点」。](images/img-00.png)

> Flow diagram of the replication stages: network enumeration, initial access and payload delivery (with opportunistic privilege escalation), inference setup, and replication. The first stages are grouped as ‘cyber exploit (not our focus)’ and inference setup is marked ‘our focus’.

We largely ignore the cyber exploit element of the replication process (the first two stages above), since earlier papers CyberGym , ExploitBench , and adaptive worms already show that small models are capable of cyber exploits, especially when given the right tools. But a less commonly measured part of the replication process involves setting up inference on compromised devices, which is our focus here.

我们基本略过复制过程中的网络漏洞利用环节（上图中前两个阶段），因为此前的论文 CyberGym、ExploitBench 以及关于自适应蠕虫的研究已经表明，小模型有能力实施网络漏洞利用，尤其是在配备合适工具的情况下。但复制过程中一个较少被测量的环节，是在被攻陷的设备上搭起推理——这正是本文的重点。

In this post, we first lay out the threat models that we think are most likely to be realized. Then, we test whether small models can set up inference on unfamiliar hardware, and whether they can chain the full replication cycle together across a fleet of machines. After that, we estimate how fast such a worm could spread, and test whether it could learn and adapt as it replicates. We conclude with how AI worms would differ from pre-AI malware, and brief suggestions for defenses against such replicators.

在本文中，我们先梳理我们认为最有可能成为现实的威胁模型；接着测试小模型能否在陌生硬件上搭起推理，以及能否在一片机器上把完整的复制循环串联起来；随后我们估算这类蠕虫的传播速度，并测试它能否在复制过程中学习和适应。最后我们讨论 AI 蠕虫与 AI 出现之前的恶意软件有何不同，并给出抵御这类复制体的简要防御建议。

### 威胁建模（Threat modelling）

In this blog post, we’re focused on one way that AI worms might propagate: small models that replicate through compromising their host machines. But there are several other methods that AI worms might try: they could steal access in order to spin up hosts (for example, API keys or platform credentials) or pay for inference through legitimate work (e.g digital freelancing). The threat models that we think are closest to being realized today are the following:

这篇博文聚焦 AI 蠕虫传播的一种方式：小模型通过攻陷宿主机来完成复制。但 AI 蠕虫还可能尝试其他几种途径：窃取访问凭据来开通主机（例如 API key 或平台凭证），或通过正当工作（比如数字零工）赚取推理费用。我们认为当下距离现实最近的威胁模型有以下几类：

- A small model, replicating via host compromise. In this post, we’ll call autonomous worms that run their models directly on compromised hardware weight-based replicators . Home and small business IoT devices might be the first target for such attacks because they’re connected to the internet and, historically, have had minimal security measures in place. A model that’s too large to fit on one device can be split into pieces, or shards, that run on separate machines that communicate with each other to collectively run the model. Machines that could run a significant model shard for a <30B model include smart fridges, smart ovens, automatic vacuum cleaners, and lawnmowers. A growing number of home and business devices, such as security cameras and video-conferencing units now ship with small built-in AI chips, designed to run AI models on the device itself. This extra computing power will likely make them attractive targets as well. Human hackers often breach secure systems through social engineering (for example, convincing a company’s IT department that you’re an employee who forgot your password) rather than hacking. This tradeoff appears to be flipped for today’s small models, which are relatively strong at agentic coding and hacking compared to their social engineering capabilities (see Appendix E), though this might change as models improve.

- **小模型，通过攻陷宿主机来复制。** 本文把直接在被攻陷硬件上运行自身模型的自主蠕虫称为「基于权重的复制体」（weight-based replicator）。家用与小企业 IoT 设备可能是这类攻击的首批目标：它们连接互联网，而且历来几乎没有安全措施。一个大到单台设备装不下的模型，可以切分成多个分片（shard），分别运行在多台机器上，相互通信、合力支撑整个模型。能为一个小于 30B 的模型运行相当规模分片的机器，包括智能冰箱、智能烤箱、扫地机器人和割草机。越来越多的家用与商用设备——如安防摄像头和视频会议终端——出厂时自带小型 AI 芯片，用于在设备本地运行 AI 模型；这些新增算力很可能也让它们成为有吸引力的目标。人类黑客突破设防系统，靠的往往是社会工程学（social engineering，例如让公司 IT 部门相信你是忘了密码的员工），而不是硬碰硬的技术攻击。对小模型而言，这个权衡似乎反了过来：与社会工程能力相比，它们在智能体编程（agentic coding）和黑客攻击上相对更强（见附录 E）——尽管随着模型进步，这一点也可能改变。

- A large, advanced weight-based replicator focused on datacenters and other high-compute targets. Larger models tend to have correspondingly better cyber capabilities, but will also need to breach (relatively) hardened datacenters to run themselves. We split this threat model into two subcategories:

- **大型、先进的基于权重的复制体，以数据中心和其他高算力目标为主。** 更大的模型网络攻击能力往往相应更强，但它们也需要攻破（相对）设防更严的数据中心才能运行自身。这一威胁模型又可分为两个子类：
  - Frontier replicators: Large closed-weight replicators run on their lab’s infrastructure, and can spin up unmonitored internal deployments or exfiltrate their weights onto external compute.
  - 前沿实验室复制体（Frontier replicators）：大型闭源权重复制体运行在自家实验室的基础设施上，可以启动无人监管的内部部署，或把权重外传到外部算力上。
  - Open-weight replicators: These replicators start off relatively lower-resourced (due to not benefitting from a lab’s compute), but start unmonitored.
  - 开源权重复制体（Open-weight replicators）：这类复制体起点资源相对较少（因为享受不到实验室的算力），但从一开始就处于无人监管状态。

- An API-based replicator, in which a model would replicate by stealing credentials for their own inference API. These replicators would require a model provider to unknowingly serve them, and their persistence may be worse, since they’d likely be easier to discover (although their rate of spread may be higher, if for instance they’re able to replicate using victims’ accounts).

- **基于 API 的复制体（API-based replicator）**：模型通过窃取自己推理 API 的凭据来完成复制。这类复制体需要模型提供商在不知情的情况下为其提供服务，持久性可能更差，因为更容易被发现（不过传播速度可能更快，比如它们能利用受害者的账户进行复制时）。

In practice, an AI worm wouldn’t necessarily consist of just one model or strategy. A swarm of different models deployed on different types of hardware using a variety of replication schemes might represent a more realistic ‘worst case scenario’ attack.

实践中，AI 蠕虫未必只由一个模型或一种策略构成。由不同模型组成、部署在不同类型硬件上、采用多种复制方案的「蜂群」，可能才是更现实的「最坏情形」攻击。

In this blog post, we consider the feasibility of small weight-based replicators (section 1 of the above). We focus on this because it’s a near-term threat model that can be investigated directly. Our demo at the top of this post, a 26B model running across 15 boxes, shows that IoT devices can run surprisingly capable models. Could these models be capable enough to carry out autonomous replication?

这篇博文考察的是小型基于权重的复制体的可行性（上述第 1 类）。之所以聚焦于此，是因为它是一个近期的威胁模型，可以直接研究。本文开头的演示——一个 26B 模型分布在 15 台盒子上运行——表明 IoT 设备能跑起能力可观的模型。这些模型的能力，足以实施自主复制吗？

We examine this in two parts. First, can small models get a model running on unfamiliar hardware? And second, can they do the whole cycle (find an uninfected device, copy themselves over, start running there, and repeat) all together in a single run?

我们分两部分考察。第一，小模型能否让一个模型在陌生硬件上运行起来？第二，它们能否在一次运行中完成整个循环（找到未感染的设备、把自己拷贝过去、在那里启动运行，然后周而复始）？

## A. 推理环境搭建（A. Inference setup）

Of the stages in the above diagram showing the replication cycle, inference setup is the least studied and may be the most challenging for small models. It requires a fairly long sequence of hardware and OS-specific decisions around picking a compatible inference stack, finding the right versions of packages, and debugging machine-specific issues. These steps are serially dependent, and failures are difficult to recover from.

在上图展示的复制循环各阶段中，推理环境搭建（inference setup）研究得最少，对小模型而言可能也最具挑战。它要求围绕挑选兼容的推理技术栈、找到正确版本的软件包、调试特定机器的问题等，做出一长串与硬件和操作系统相关的决策。这些步骤串行依赖，一旦失败很难恢复。

> **Aside**
>
> Unlike many types of malware, weight-based replicators don’t need root privileges. While it’d be convenient to privilege escalate in order to have persistence over shutdowns, such as via cronjobs, it may not be truly necessary. Inference engines can run entirely in userspace, and many of the low-security devices we anticipate such a replicator to spread on can be re-infected upon restart. Similar examples include crypto miners and botnets/worms like P2Pinfect and Fritzfrog, which can operate without privilege escalation but will try it opportunistically.
>
> **题外话**
>
> 与许多类型的恶意软件不同，基于权重的复制体并不需要 root 权限。虽然通过提权（privilege escalation，比如设置 cronjob 定时任务）来获得跨关机的持久化会很方便，但这可能并非真正必需。推理引擎完全可以只在用户态（userspace）运行，而我们预计这类复制体会蔓延的许多低安全设备，重启后还能被再次感染。类似的例子有加密货币挖矿程序，以及 P2Pinfect、Fritzfrog 等僵尸网络/蠕虫——它们无需提权也能运作，但会伺机尝试提权。

### 评估推理环境搭建的表现（Evaluating inference setup performance）

We measure the ability of different open-weight models to serve Llama3.1-8B across heterogeneous hardware types. See the table below for tokens per second served across various hardware types:
- Click a cell for its distribution of serving speeds.
- Click a row/column label to get a comparison across models or hardware type.

我们测量了不同开源权重模型在异构硬件上提供 Llama3.1-8B 推理服务（serve）的能力。下表列出了各类硬件上每秒可服务的 token 数（交互式表格，见原文）：
- 点击单元格，可查看其服务速度的分布。
- 点击行/列标签，可在不同模型或硬件类型之间对比。

Small models, such as Qwen3.5-9B, are capable of setting up inference servers on hardware from Jetson Nanos to Mac PCs and RTX 4090s. More capable models such as GLM-5.2 are not only more reliable but also achieve higher serve tokens/s, allowing for faster replication. We test and mention specific chips to measure how well models can set up inference on them. The chips aren’t a source of risk or vulnerabilities.

Qwen3.5-9B 这类小模型，已能在从 Jetson Nano 到 Mac 电脑再到 RTX 4090 的硬件上搭建推理服务器。GLM-5.2 等能力更强的模型不仅更可靠，还能达到更高的服务速度（token/s），从而支持更快的复制。我们测试并在文中点名具体芯片，是为了衡量模型在它们上面搭建推理的能力。这些芯片本身并不是风险或漏洞的来源。

We run these experiments using Memento, a harness we built on top of the open-source Pi coding agent. Memento is designed to mitigate a common failure mode we observe in long-horizon runs: an agent can become fixated on an incorrect idea (for, say, why a bug is occurring) and continue pursuing it instead of backtracking. One example of such a failure is a Qwen agent debugging package versioning for hours when the real bug came from implementing an endpoint incompatible with the grader API.

这些实验使用 Memento 运行——它是构建在开源编程智能体 Pi 之上的一个 harness（智能体运行框架）。Memento 旨在缓解我们在长程运行中观察到的一种常见失败模式：智能体可能固守一个错误的想法（比如关于某个 bug 为何出现），一路钻牛角尖而不回头。这类失败的一个例子是：一个 Qwen 智能体花了几个小时调试软件包版本，而真正的 bug 其实来自它实现了一个与评分器（grader）API 不兼容的端点。

Memento periodically starts a fresh agent instance with the previous agent’s workspace and a short set of notes written by the previous agent on what it tried, allowing later instances to pursue different strategies.

Memento 会定期启动一个全新的智能体实例，让它继承前一实例的工作区，以及前任写下的一组简短笔记（记录自己尝试过什么），从而让后来的实例得以尝试不同策略。

### 模型如何在智能电视上运行？（How does a model run on Smart TVs?）

IoT devices are light on memory, but distributed inference as in Petals makes serving our lightweight models feasible. The representative threat model is Kimwolf , a large IoT botnet that infected over two million devices, primarily off-brand Android TVs. Such smart TV boxes, which are readily available for purchase, typically run forks of the Android Open Source Project (AOSP), which their manufacturers don’t add security features to . Typical boxes in this range ship with between 1 and 4 gigabytes of RAM. Assuming 4GB of RAM at 4-bit quantization, this allows for inference of 9B models with 4 or more machines, and inference of 26B models with 10+ machines.

IoT 设备内存不大，但像 Petals 那样的分布式推理（distributed inference），让伺服我们的轻量模型成为可行。代表性的威胁样本是 Kimwolf——一个感染了超过两百万台设备（主要是杂牌 Android 电视）的大型 IoT 僵尸网络。这类随处可买的智能电视盒子，通常运行 Android 开源项目（AOSP）的分支系统，而厂商并不为这些分支添加安全特性。这一档位的典型盒子出厂配备 1 到 4 GB 内存。假设内存 4GB、采用 4-bit 量化，那么 4 台或更多机器即可支撑 9B 模型的推理，10 台以上即可支撑 26B 模型的推理。

In the demo at the top of this post, we ran Gemma4-26B-A4B across 15 smart TV boxes with 4GB of RAM each. We sharded the model under pipeline parallelism, where each box holds a small fraction of the model’s layers.

在本文开头的演示中，我们在 15 台各配 4GB 内存的智能电视盒子上运行了 Gemma4-26B-A4B，并采用流水线并行（pipeline parallelism）对模型分片，每台盒子只持有模型层的一小部分。

## B. 病毒式扩散：跨 GCP 节点机群的自主搭建与复制（B. Going viral: autonomous setup and replication across a GCP node fleet）

Now that we’ve seen that models can set up inference on unfamiliar hardware, let’s measure whether models are capable of composing the steps—that is, scanning a network for targets, accessing them via a reverse shell, sending over the viral payload (weights and harness), and setting up inference.

看过了模型能在陌生硬件上搭建推理之后，我们来测量模型能否把这些步骤串联起来——即扫描网络寻找目标、经由反向 shell（reverse shell）取得访问、把病毒载荷（权重与 harness）传送过去，然后搭建推理。

### 自主推理搭建与复制（简化网络攻击部分）（Autonomous inference setup and replication (with simplified cyber)）

To do this, we built a firewalled VPC of 8 GCP nodes running redroid, an Android 15 emulator. Patient zero (gray node) is seeded with the worm. Every subsequent infection is the worm itself hopping node to node over an unauthenticated ADB port, before setting up an inference server to serve Llama3.1-8B. If the inference server passes a number of correctness and anti-cheating checks, we spawn another copy of the worm on the target node (for example, if GLM-5.2 sets up Llama3.1 to run on a machine, we spawn a copy of the GLM-5.2 worm on it). The worm’s model runs on a separate inference server, since the VPC nodes aren’t large enough to run the larger models.

为此，我们搭建了一个有防火墙隔离的 VPC，内含 8 个 GCP 节点，运行 redroid（一种 Android 15 模拟器）。「零号病人」（patient zero，灰色节点）被植入蠕虫。此后的每一次感染，都是蠕虫自己通过一个无需认证的 ADB 端口在节点间逐跳跳转，然后搭建起为 Llama3.1-8B 提供服务的推理服务器。如果该推理服务器通过了一系列正确性与反作弊检查，我们就在目标节点上再生成一份蠕虫副本（例如，GLM-5.2 若让 Llama3.1 在某台机器上跑了起来，我们就在那台机器上生成一份 GLM-5.2 蠕虫副本）。蠕虫自己的模型则运行在一台独立的推理服务器上，因为 VPC 节点的规格不足以运行更大的模型。

In the graph below, we drop a given model in as a payload and measure its ability to replicate.

在下图中，我们把给定模型作为载荷投放进去，测量其复制能力（交互式图表，见原文）。

### 为什么要让模型为 Llama3.1-8B 搭建推理，而不是为它们自己？（Why task the models to stand up inference for Llama3.1-8B and not themselves?）

We’d like to have an apples-to-apples comparison, since larger models are more difficult to serve inference for. Our experiment measures if GLM has the competence to autonomously perform the steps that would let a sufficiently small model proliferate – that is, if GLM-5.2 is capable of proliferation, this implies that an 8B-sized model with GLM-5.2’s capability would also be capable of standing up itself (with the caveat that newer small models may have more complex architectures and kernels).

我们希望做一场对等比较，因为模型越大，为其搭建推理服务越困难。我们的实验测量的是：GLM 是否有能力自主完成那些能让一个足够小的模型扩散开来的步骤——也就是说，如果 GLM-5.2 有能力扩散，这就意味着一个具备 GLM-5.2 能力的 8B 级模型同样能把自身跑起来（需要注意，新一代小模型的架构和内核（kernel）可能更复杂）。

Only GLM-5.2 out of our set of tested open-weight models is capable of full end-to-end replication (assuming its capabilities could be packed into Llama3.1-8B). Note that GLM-5.2 does this reliably while the other models fail reliably (it spread to 8/8 nodes in 4 out of 5 trials and 7/8 in 1).

在我们测试的开源权重模型中，只有 GLM-5.2 能完成完整的端到端复制（假定它的能力可以被装进 Llama3.1-8B）。值得注意的是，GLM-5.2 稳定地做到，而其他模型则稳定地失败（5 次试验中 4 次传播到全部 8/8 个节点，1 次为 7/8）。

It appears that GLM-5.2 is able to propagate Llama3.1-8B well, which suggests that future small models of the same capability level could also complete the network enumeration, payload delivery, and inference standup steps of the replication process. Our evaluation keeps the cyber-exploit element of the replication trivial (there’s no authentication over the ADB port), which makes this an upper bound on real-world success. Note that past IoT botnets have replicated via means as simple as going through a default list of manufacturer usernames and passwords, as in the case of the Mirai botnet , so real IoT security might not be dramatically more difficult than the trivial step in our evaluation.

GLM-5.2 似乎能顺利传播 Llama3.1-8B，这表明未来同级能力的小模型，也能完成复制过程中的网络枚举、载荷投递和推理搭建步骤。我们的评估把复制中的网络漏洞利用环节设得极为简单（ADB 端口无任何认证），因此这一结果构成现实世界成功率的上限。不过请注意，过去的 IoT 僵尸网络复制手段可以简单到只需遍历厂商默认的用户名和密码列表——Mirai 僵尸网络就是如此——所以真实世界的 IoT 安全未必比我们评估中的简化环节难上太多。

As a sanity check, we can see how difficult the autonomy and orchestration step is (if we took out cyber and inference setup, how tough is doing the remaining command-line grunt work?). This ablation is the above evaluation with the inference setup steps removed (done via a pre-written script). Many models in our roster, including the smaller ones, are capable of these remaining steps.

作为合理性检验，我们可以看看自主性与编排这一步有多难（如果把网络攻击和推理搭建拿掉，剩下的命令行苦活有多难？）。这个消融实验就是上述评估去掉推理搭建步骤后的版本（由预先写好的脚本代劳）。我们测试的许多模型——包括较小的那些——都能完成剩下的步骤。

> Replication on a contained 17-node redroid fleet: two subsets of 8 nodes joined by one pivot node (see Appendix F, with the mild prompt). Each tree is the model’s median run of six.
>
> 图注（交互式图表，见原文）：在一个封闭的 17 节点 redroid 机群上的复制：两个各 8 节点的子集由一个枢轴节点（pivot node）相连（使用温和提示词，见附录 F）。每棵树是该模型六次运行的中位数结果。

When it comes to this worm, we can envision:
- A model capabilities floor that an autonomous worm must sit above, to complete its replication reliably. In our simplified evaluation, GLM-5.2 sits above this floor.
- A model size ceiling that the model must sit under, such that it’s feasible to run on the devices it’s attacking. Qwen3.5-9B and Gemma4-26B-A4B sit under this ceiling for the class of smart TVs we test (4GB of RAM).

对这类蠕虫，可以设想两条边界：
- 一条模型能力下限（capabilities floor）：自主蠕虫必须位于其上，才能可靠地完成复制。在我们的简化评估中，GLM-5.2 在这条下限之上。
- 一条模型体积上限（size ceiling）：模型必须位于其下，才可能在它攻击的设备上运行。就我们测试的这类智能电视（4GB 内存）而言，Qwen3.5-9B 和 Gemma4-26B-A4B 都在这条上限之下。

Right now, the floor and ceiling don’t intersect. When might they?

眼下，这条下限和这条上限还没有相交。什么时候会相交？

Above is a plot of notable open-weight models of 30B total parameters or fewer, each plotted at its release date against the Epoch Capabilities Index (ECI). The solid line is the least-squares fit over the best models in-class at each date, with a gain of 13.7 ECI per year (R 2 of 0.95). If we take GLM-5.2 as a capabilities floor for replication, the trend line puts a small model at that level by the end of 2026. ECI is a general capability index, which is only a proxy metric for replication ability, so this is only a rough estimate of when a small-model replicator becomes possible.

上图（交互式图表，见原文）画出了总参数量 30B 及以下的知名开源权重模型，以发布日期为横轴、Epoch 能力指数（Epoch Capabilities Index, ECI）为纵轴。实线是对各时点同类最优模型的最小二乘拟合，每年提升 13.7 ECI（R² 为 0.95）。如果把 GLM-5.2 视为复制的能力下限，按这条趋势线，到 2026 年底就会有一个小模型达到该水平。ECI 是一个通用能力指数，只是复制能力的代理指标，因此这只是对小模型复制体何时成为可能的一个粗略估计。

## C. AI IoT 蠕虫的复制速度可能有多快？（C. How fast might an AI IoT worm replicate?）

Unlike coding agents, these replicators would be running below the interactive speeds of user-facing chat and coding harnesses. The spread would be slow and persistent, with tokens being trickled out on edge hardware. We can use our achieved speeds on our real smart TVs as a demonstrated baseline for replication speeds. For Gemma4-26B-A4B, unoptimized speed across a 32k context length token window is about 0.3 tok/s, averaged across prefill and decode in our inference setup workload. More details in Appendix C.

与编程智能体不同，这类复制体的运行速度将低于面向用户的聊天与编程 harness 的交互速度。传播会缓慢而持久，token 在边缘硬件上细水长流般产出。我们可以用真实智能电视上实测到的速度，作为复制速度的实证基线。以 Gemma4-26B-A4B 为例，在我们的推理搭建工作负载中，跨越 32k 上下文长度 token 窗口的未优化速度约为 0.3 tok/s（预填充 prefill 与解码 decode 的平均值）。更多细节见附录 C。

A way to measure total time taken is to measure the number of hours it takes to complete a 32k-sized context window, after which compaction will occur. Based on our inference setup eval statistics, it takes between ~2.6 and ~4.7 full windows to complete a trajectory.

衡量总耗时的一种方法，是算出填满一个 32k 大小的上下文窗口需要多少小时——填满之后就会触发压缩（compaction）。根据我们推理搭建评估的统计，完成一条完整轨迹大约需要 2.6 到 4.7 个完整窗口。

The number of devices infected by the Kimwolf botnet at its peak, reported by Synthient , is somewhere over 2 million (primarily Android TV boxes). In an equivalently-sized infected fleet of TVs serving AI models, if every group of 15 TVs ran a copy of Gemma4-26B-A4B, we would have ~133k copies (“replicas”). A group of TVs like the ones in our demo can serve 2 agents simultaneously at roughly 0.3 tokens per second each, yielding 80k tokens/s across the botnet.

据 Synthient 报告，Kimwolf 僵尸网络巅峰期感染的设备超过两百万台（以 Android 电视盒子为主）。在一个同等规模、运行 AI 模型的被感染电视机群中，如果每 15 台电视运行一份 Gemma4-26B-A4B 副本，我们将拥有约 13.3 万份副本（replicas）。像演示中那样的一组电视可以同时服务 2 个智能体、每个约 0.3 token/s，折算到整个僵尸网络约为 8 万 token/s。

As a reference point, an H100 can serve Gemma4-26B-A4B at ~1.95k tok/s at high concurrency with speculative decoding , so our swarm would have at most about 40 H100s-worth of inference. This is a tiny amount of compute in absolute terms, and the agents are slow replicators (over 1-2 days per 9B replication). As an analogy, imagine the replicator as a slime mold or a swarm of fruit flies, which replicate over days.

作为参照，一块 H100 在高并发并启用投机解码（speculative decoding）时，能以约 1.95k tok/s 的速度服务 Gemma4-26B-A4B；因此我们的「蜂群」最多约等于 40 块 H100 的推理算力。这一算力的绝对值很小，而且这些智能体是慢速复制者（每完成一次 9B 复制需要 1–2 天以上）。打个比方，不妨把复制体想象成黏菌或一群果蝇——以天为单位繁殖。

This is a rough estimate, as it’s anchored on a single class of IoT hardware and a single past botnet instance, but describes what a simple replicator released in the near future might look like. In practice, more capable replicators would take advantage of other types of low-security compute, and would be opportunistic about deploying models of various sizes on different devices. (It also wouldn’t have to load a model payload onto every device it compromises). Over time, we expect the amount of deployed edge compute to continue growing, while the minimum model size required for reliable replication falls.

这只是个粗略估计——它锚定在单一类别的 IoT 硬件和单个历史僵尸网络案例上——但它大致描绘了近期可能出现的简单复制体的样子。实践中，能力更强的复制体会利用其他类型的低安全算力，并见机行事地在不同设备上部署不同规模的模型（它也不必在每台攻陷的设备上都装载模型载荷）。随着时间推移，我们预计已部署的边缘算力会持续增长，而可靠复制所需的最小模型体积会不断下降。

## D. 适应与增益（D. Adaptation and uplift）

### 在工作中学习（Learning on the job）

The “Adaptation” part of ARA is one reason why an AI replicator might outperform a traditional worm. Specifically, models and prompts can undergo Lamarckian evolution as they spread–that is, parent instances can pass on their prompts, code, and notes to their daughter replicators. In order to adapt and spread across a wide variety of hardware, a model would require two things:
- Being above a certain threshold of capability for spreading onto a novel, unseen piece of hardware (or software environment, or network type).
- Once seen, adapting to the type of environment it has landed in (it should replicate more reliably on hardware it has seen before).

ARA 中的「适应」，是 AI 复制体可能胜过传统蠕虫的一个原因。具体来说，模型和提示词在传播过程中可以经历拉马克式进化（Lamarckian evolution）——也就是说，父代实例可以把它的提示词、代码和笔记传给子代复制体。要在五花八门的硬件上适应并扩散，模型需要具备两点：
- 能力超过某个阈值，以便扩散到从未见过的新硬件（或软件环境、网络类型）上。
- 见过之后，适应它所落地的那类环境（对见过的硬件，应能复制得更可靠）。

Sections A and B test the first condition, and in the experiment below we test the second. Each Qwen3.5-9B instance keeps brief notes on the strategies it tried and the failures it encountered, before a new instance inherits those notes and attempts the same inference setup again.

A、B 两节检验的是第一个条件，下面的实验检验第二个。每个 Qwen3.5-9B 实例都会简要记下自己尝试过的策略和遭遇的失败，随后由一个新实例继承这些笔记，再次尝试同样的推理搭建。

> Inference setup success rate across six generations of Qwen3.5-9B, each inheriting the former’s notes. Faint lines are the 20 individual lineages, and the bold line is the proportion of runs in each generation that complete setup. It rises from 45% in the first generation to between 61% and 84% in later ones. This uses the Memento setup (Appendix A).
>
> 图注（交互式图表，见原文）：Qwen3.5-9B 六代传承的推理搭建成功率，每代继承上一代的笔记。淡色线是 20 条独立世系，粗线是每一代完成搭建的运行占比：从第一代的 45% 升至后续几代的 61%–84%。采用 Memento 设置（附录 A）。

### 听取建议（Taking advice）

Another concern is that small models can ask for help from a frontier advisor , a stronger model that is either offered as a tool call or gives unsolicited advice when the small model is stuck. To test this, we gave Qwen3.5-9B access to Opus 4.8 during the inference setup task. The frontier model’s advice doesn’t measurably help: small models tend to not know the right questions to ask, and they don’t improve when the advice comes unprompted.

另一个担忧是小模型会向「前沿顾问」（frontier advisor）求助——一个更强的模型，或作为工具调用提供给它，或在它卡住时主动给出建议。为验证这一点，我们在推理搭建任务中让 Qwen3.5-9B 可以使用 Opus 4.8。前沿模型的建议没有带来可测量的帮助：小模型往往不知道该问什么问题，而当建议主动送上门时，它们的表现也没有改善。

Even when trained through reinforcement learning to use the advisor in a similar setting (Python coding problems), the small model still struggles to ask for help productively for inference standup. Both results are shown in the plot below.

即便用强化学习训练小模型在类似场景（Python 编程题）中使用顾问，它在推理搭建时仍难以有效地求助。两项结果都显示在下图中（交互式图表，见原文）。

> Inference setup success rate by advisor access, with Wilson 95% intervals. Access to an advisor, whether offered as a tool call or given to the model automatically when stuck, gives no significant improvement. The two RL groups were trained with advisor access on software engineering tasks, and the rightmost group also rewards calling the advisor when stuck.
>
> 图注（交互式图表，见原文）：按顾问访问方式划分的推理搭建成功率（Wilson 95% 置信区间）。无论顾问是作为工具调用提供，还是在卡住时自动介入，都没有显著提升。两个强化学习（RL）组在软件工程任务上接受了带顾问访问的训练，最右一组还会在卡住时调用顾问获得奖励。

Since small models are capable and persistent individual agents and make poor use of advice, we think coordination is more likely to happen with a large model handling high-level strategy and spinning up small models as subagents.

鉴于小模型作为单个智能体既有能力又够执着、却不善于利用建议，我们认为更可能的协调方式是：由一个大模型掌控高层策略，并把小模型作为子智能体（subagent）派生出去。

## E. 这与 AI 出现之前的僵尸网络有何不同？（E. How is this different from a pre-AI botnet?）

Hard-coded botnets like the Kimwolf botnet each target a specific type of device – in Kimwolf’s case, Android TVs and mobile devices. An AI botnet isn’t tied to any particular hardware or software type, as evidenced by small models being able to set up inference engines on machines from a Jetson Nano to a Mac (see the table in section A). We expect AI botnets to infect more than IoT devices, and to use weight-based replication as just one strategy in their playbook. AI botnets could also act intelligently to counter defenders, but we haven’t tested this.

像 Kimwolf 这样硬编码的僵尸网络，各自针对特定类型的设备——Kimwolf 的目标是 Android 电视和移动设备。AI 僵尸网络则不局限于任何特定硬件或软件类型：小模型能在从 Jetson Nano 到 Mac 的各类机器上搭建推理引擎（见 A 节的表格）就是明证。我们预计 AI 僵尸网络的感染范围不会只限于 IoT 设备，基于权重的复制也只是其战术手册中的一种策略。AI 僵尸网络还可能聪明地反制防御者，不过我们尚未测试这一点。

## F. 蓝队缓解措施（F. Blue-team mitigations）

As with an epidemic, defense involves driving R₀ (the number of new devices each infected device goes on to infect) below 1, such that the replicator dies out instead of staying endemic.

与应对传染病一样，防御的关键是把 R₀（基本再生数，即每台被感染设备随后感染的新设备数量）压到 1 以下，让复制体自行消亡，而不是长期流行。

Some elements around this threat model are in the defenders’ favor:
- A weight-based replicator multiplying through IoT machines at scale would leave a network signature, visible as an anomaly in ISP flow-based monitoring systems. Newly infected hosts may receive multi-gigabyte model payloads from other devices or external hosting services, while distributed inference can produce sustained device-to-device traffic between machines that wouldn’t ordinarily communicate at that volume. Propagation itself would add additional scanning and outbound connection patterns. In practice, the replicator trades off between replication speed and stealth: the faster it replicates, the more traffic it produces. Once the replicator is discovered, defenders can use honeypots (decoy devices) to collect the replicator’s harness and payload to characterize its behavior.
- Remediation of the Mirai botnet was possible when an ISP used externally obtained infection evidence to quarantine and notify customers. Quarantining infected subscribers involved restricting their networks to only being capable of reaching a short whitelist of sites, and otherwise redirecting their browsers to cleanup instructions. This cleared 92% of infections within 14 days, against 74% with no notification. We’d expect quarantining to be similarly effective for an initial defense against a weight-based replicator. Eliminating the replicator completely would mean coordinating across every ISP with an infected network, which is difficult.
- Manufacturers can shrink the pool of targets, with regulation acting to coordinate. For example, default IoT passwords are now non-compliant in UK markets . Moving to more secure defaults and raising the baseline for compliance will help harden against both AI and scripted botnets.

围绕这一威胁模型，有些因素站在防御者一边：
- 基于权重的复制体在大量 IoT 机器上增殖时，会留下网络特征，在 ISP 基于流的监测系统中表现为异常。新感染的主机可能从其他设备或外部托管服务接收数 GB 的模型载荷；分布式推理也会在平日不会有这种量级通信的机器之间，产生持续的设备间流量。传播过程本身还会带来额外的扫描和外联连接模式。实践中，复制体要在复制速度与隐蔽性之间权衡：复制得越快，产生的流量越多。一旦被发现，防御者可以用蜜罐（honeypot，诱饵设备）收集复制体的 harness 和载荷，进而刻画其行为特征。
- Mirai 僵尸网络之所以能被清除，是因为一家 ISP 利用外部获得的感染证据，对客户实施了隔离和通知。隔离被感染的订阅用户，具体做法是把他们的网络限制为只能访问一小串白名单网站，否则就把浏览器重定向到清理指引。这一措施在 14 天内清除了 92% 的感染，而不做通知时只有 74%。我们预计，对基于权重的复制体，隔离作为初始防御同样有效。但要彻底消灭复制体，就需要每一个网络中有感染的 ISP 协同行动，这很困难。
- 厂商可以缩小目标池，监管则能起到协调作用。例如在英国市场，IoT 设备使用默认密码如今已属不合规。改用更安全的默认设置、提高合规基线，将有助于同时加固对 AI 僵尸网络和脚本化僵尸网络的防御。

## G. 结论：初现端倪（G. Conclusion: Signs of life）

The capabilities that make AI models generally powerful coding agents also generalize to cyber tasks, including autonomous replication. Like coding agents in 2024, we’re seeing signs of life for AI worms. We don’t yet have a full rogue replicator, but agents are becoming increasingly capable of completing the individual steps in the replication chain (and passing on their adaptations to future copies of themselves).

让 AI 模型成为强大编程智能体的那些能力，同样会泛化到网络任务上，包括自主复制。正如 2024 年的编程智能体，我们正在看到 AI 蠕虫的端倪。完整的失控复制体尚未出现，但智能体正越来越有能力完成复制链中的各个单独步骤（并把它们的适应成果传给未来的自身副本）。

At the same time, increasing amounts of inference compute are now available both inside and outside of datacenters (see Appendix D). It’s uncertain exactly what form autonomous AI replication will take. It could be a slow, distributed “digital slime mold,” living on relatively insecure compute. It could also be a powerful cyber model that breaches datacenters at a high pace, or a model that scams people for compute credits.

与此同时，越来越多的推理算力正出现在数据中心内外（见附录 D）。自主 AI 复制究竟会以何种形态出现，仍不确定：它可能是一个缓慢、分布式的「数字黏菌」，栖身于相对不安全的算力之上；也可能是一个高速攻破数据中心的强大网络攻击模型，或是一个靠欺骗手段骗取算力额度的模型。

As a prediction that goes beyond our experiments, we expect a future awash with agents running on all types of hardware, including some that aren’t under human control. While it’s true that IoT devices have been vulnerable for a long time, lightweight agents may be much more willing than human attackers to put in the effort to compromise large fractions of the world’s IoT compute. The rise of slow replicators would also be a bellwether for more aggressive, faster ones that can breach more hardened machines or be effective API key thieves. We should aim to harden our systems to make it difficult for such replicators to spread, but also prepare to deal with a world where they are here to stay.

作为一个超出本文实验范围的预测，我们预计未来将充满运行在各类硬件上的智能体，其中一些并不受人类控制。IoT 设备脆弱由来已久，这是事实；但轻量级智能体可能远比人类攻击者更愿意下功夫，去攻陷全球 IoT 算力中的大部分。慢速复制体的兴起，也将是更激进、更快速的复制体的先兆——后者能攻破设防更严密的机器，或成为高效的 API key 窃贼。我们应当致力于加固系统，让这类复制体难以扩散，但也要准备好应对一个它们将长期存在的世界。

## 附录（Appendix）

正文之后，原文另附附录 A–F（A：Memento harness；B：为何选择流水线并行；C：IoT 硬件上的吞吐与内存；D：边缘加速器；E：鱼叉式网络钓鱼能力；F：脚本化推理搭建下的传播实验），本译文未收录，见原文链接。

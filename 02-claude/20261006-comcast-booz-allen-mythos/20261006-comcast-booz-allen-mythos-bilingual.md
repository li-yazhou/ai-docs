# Comcast 与 Booz Allen 如何用 Claude Mythos 发现利用链并加固代码库（中英对照）

> **原文标题：** How Comcast and Booz Allen use Claude Mythos to find exploit chains and secure their codebases
> **原文链接：** https://claude.com/blog/how-comcast-booz-allen-use-claude-mythos-to-secure-their-codebases
> **原文作者：** Comcast 与 Booz Allen 安全团队（原文未逐段署名）
> **发布日期：** 2026-10-06
> **翻译模型：** DeepSeek-V4.1-Flash
> **评分：** ★★★☆☆ —— 两家关键基础设施防守方的一手实战：Claude Mythos 级模型如何跨代码、配置与运行时行为发现单点扫描器漏掉的跨系统漏洞与利用链，并给出发现、验证、修复的真实流程与效率数字。
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。术语保留英文原文并附中文释义。

---

Security teams at Comcast and Booz Allen used Claude Mythos-class models through Project Glasswing to find, validate, and fix vulnerabilities their existing scanners missed.

Comcast 与 Booz Allen 的安全团队通过 Project Glasswing 使用 Claude Mythos 级模型，找出、验证并修复了其现有扫描器漏掉的漏洞。

Earlier this year, Anthropic launched Project Glasswing, a limited access program to give defenders of the world’s critical infrastructure a head start on securing their codebases. This week, we launched a new, expanded version of our Cyber Verification Program (CVP) that makes advanced cyber capabilities and reduced blocking classifiers available to qualifying security professionals.

今年早些时候，Anthropic 启动了 Project Glasswing，这是一个限制访问的计划，旨在让全球关键基础设施的防守者提前着手加固自己的代码库。本周，我们推出了 Cyber Verification Program（CVP，网络验证计划）的全新扩展版本，向符合条件的安全专业人员提供先进的网络能力与减免的拦截分类器（blocking classifier）。

As part of Project Glasswing, organizations defending critical infrastructure have put Claude Mythos models to work on the systems they defend. Across these organizations, one theme emerged: while existing cybersecurity tools can identify single vulnerabilities in a silo, exploit chains, where an attacker links several individually minor weaknesses into a path to something serious, are much harder to trace. Models like Claude Mythos help piece together code, configuration, and live application behavior so that organizations can catch both ordinary single-file bugs and the sophisticated cross-system vulnerabilities that only appear when components interact.

作为 Project Glasswing 的一部分，保护关键基础设施的组织已让 Claude Mythos 模型在其所防守的系统上投入工作。在这些组织中浮现出一个共同主题：现有网络安全工具能够孤立地识别单个漏洞，但利用链（exploit chain）——攻击者把若干单独看来都属次要的弱点串联成一条通向严重后果的路径——要追踪起来困难得多。Claude Mythos 这类模型有助于把代码、配置与线上应用行为拼接起来，使组织既能捕获普通的单文件缺陷，也能捕获那些只有在组件交互时才出现的复杂跨系统漏洞。

We spoke with security leaders from Comcast and Booz Allen who participated in the program about what Claude Mythos-class models found in their environments and how it’s changing the way their teams defend against attacks.

我们与该计划的参与方 Comcast 与 Booz Allen 的安全负责人进行了交流，了解 Claude Mythos 级模型在其环境中发现了什么，以及它正在如何改变其团队防御攻击的方式。

# Comcast：把发现项与可利用漏洞区分开（Comcast: Separating findings from exploitable vulnerabilities）

Comcast, a global media and technology company, used Claude Mythos Preview to identify a critical authentication vulnerability in a public-facing platform during an assessment covering 258 business-critical systems and approximately 170 million lines of code. The issue could have allowed an attacker to bypass authentication controls and was remediated before any evidence of exploitation was observed.

全球媒体与科技公司 Comcast 在一次覆盖 258 个业务关键系统、约 1.7 亿行代码的评估中，使用 Claude Mythos Preview 在一个面向公众的平台上发现了一处严重的身份认证漏洞。该问题本可让攻击者绕过身份认证控制，已在观察到任何利用迹象之前完成修复。

> “Discovery is becoming faster,” said Noopur Davis, EVP, Chief Information Security and Product Privacy Officer, Comcast. “Discovery is becoming easier. The volume of findings is enormous. Validation of these volumes of findings is the new bottleneck."

> “发现正变得越来越快，”Comcast 执行副总裁（EVP）、首席信息安全与产品隐私官（Chief Information Security and Product Privacy Officer）Noopur Davis 表示。“发现正变得越来越容易。发现项的数量极其庞大，而对如此海量的发现项做验证，成了新的瓶颈。”

The finding emerged from Claude Mythos Preview's ability to reason across a complex software ecosystem rather than evaluating individual files or components in isolation. The vulnerability was not caused by a single defective component. Instead, it arose from the interaction of multiple systems and assumptions that appeared correct when reviewed independently. By maintaining context across code, configuration, and application behavior, Claude Mythos Preview identified a security gap that emerged across multiple interacting components, making it difficult to detect through conventional approaches.

这一发现来自 Claude Mythos Preview 跨整个复杂软件生态进行推理的能力，而非孤立地评估单个文件或组件。该漏洞并非由某一个有缺陷的组件造成，而是源于多个系统之间的交互，以及那些单独审查时看似正确的假设。通过保持对代码、配置与应用行为的上下文，Claude Mythos Preview 识别出一处跨越多个相互作用的组件才显现的安全缺口，传统方法很难检测到。

> “Frontier AI models are making it much easier to generate findings at scale,” said Davis. “But a finding becomes a vulnerability only after validation confirms the underlying issue, and a vulnerability becomes exploitable only when it can actually be exploited in the target environment.”

> “前沿 AI 模型让大规模产生发现项变得容易得多，”Davis 表示。“但只有经过验证、确认了底层问题之后，一个发现项才成为漏洞；而只有当它确实能在目标环境中被利用时，一个漏洞才成为可利用漏洞。”

Comcast engineers validated the finding against the running application and confirmed that the underlying security issue could be reached in practice. The team then traced the root cause, implemented remediation, and verified that the vulnerability had not been exploited prior to being fixed. The finding was representative of a broader pattern observed: some of the highest-impact issues identified by Claude Mythos-class models emerged not from individual coding errors, but from subtle interactions across complex software environments.

Comcast 的工程师针对正在运行的应用验证了该发现，确认底层的安全问题在实践中可以被触达。团队随后追溯根因、实施修复，并核实该漏洞在被修复之前并未遭到利用。这一发现代表了所观察到的一种更普遍的模式：Claude Mythos 级模型识别出的部分影响最大的问题，并非来自单个编码错误，而是来自复杂软件环境中的微妙交互。

> “Early access probably accelerated our understanding of the security capabilities of frontier AI models by about a year,” Davis said. “We really appreciate the community that Anthropic has built around this program - it allows participants to share lessons learned, thus helping the wider ecosystem.”

> “提前获得访问权，大概让我们对前沿 AI 模型安全能力的理解加速了约一年，”Davis 说。“我们非常认可 Anthropic 围绕该计划建立的社区——它让参与者可以分享经验教训，从而帮助更广泛的生态。”

# Booz Allen：把利用链拼接起来（Booz Allen: Piecing together exploit chains）

Booz Allen builds and deploys technology for defense, civil, national security, and commercial missions, and its cyber practice works to close the speed gap between AI-powered adversaries and traditional cyber defenses. This includes running red-team exercises and building defenses for customers facing AI-enabled adversaries.

Booz Allen 为国防、民用、国家安全与商业任务构建并部署技术，其网络业务致力于弥合 AI 驱动的攻击者与传统网络防御之间的速度差。这包括开展红队演练（red-team exercise），以及为面对 AI 赋能攻击者的客户构建防御。

Booz Allen’s security assessments run for a fixed period of time, and engineers can read only so much source code, configuration, and compiled software before they end. Wider coverage alone wouldn't close that gap, because real attacks usually work by chaining small weaknesses across code, configuration, identity, and deployment settings, and tracing those connections by hand across a large software portfolio is slow even for a skilled operator.

Booz Allen 的安全评估有固定的时间窗口，评估结束前工程师能读完的源代码、配置与编译后软件是有限的。仅靠扩大覆盖范围并不能弥合这一差距，因为真实攻击通常是把代码、配置、身份与部署设置中的小弱点串联起来，而在庞大的软件资产组合中手工追踪这些关联，即便对熟练的操作者来说也很慢。

Booz Allen used Claude Mythos Preview to review far more software than a short engagement window would normally allow, and to identify weaknesses that could be strung together into exploit chains.

Booz Allen 使用 Claude Mythos Preview 审查了远超短期项目窗口通常所能覆盖的软件量，并识别出那些可被串联成利用链的弱点。

> “Mythos was surprising in its ability to reason across boundaries rather than only identify isolated code issues,” said Sahil Sanghvi, Vice President of AI Engineering at Booz Allen. “It helped connect application behavior, configuration, identity, permissions, and deployment context into coherent security hypotheses.”

> “Mythos 令人意外之处在于它能跨边界推理，而不只是识别孤立的代码问题，”Booz Allen 人工智能工程副总裁（Vice President of AI Engineering）Sahil Sanghvi 表示。“它帮助把应用行为、配置、身份、权限与部署上下文连接成连贯的安全假设。”

The work also moved from discovery to remediation faster than his team expected, with the model helping to draft fixes and assemble evidence for the engineers who owned the affected systems.

这项工作从发现推进到修复的速度也快于其团队的预期，模型帮助起草修复方案，并为负责受影响系统的工程师汇集证据。

In one case, a Claude Mythos-class model found a flaw in the code that runs when a device first powers on, before the operating system loads. That code decides whether the device should lock itself or erase its data, and it makes that call using a security key stored on the machine. The key had been left unprotected, so the operating system it was meant to restrain could swap in a key an attacker controlled and then feed the machine instructions it would trust. An attacker could hold off the lock-or-erase step indefinitely, keeping a lost, stolen, or compromised device fully accessible when it should have shut itself down.

在一个案例中，某个 Claude Mythos 级模型在设备首次上电、操作系统加载之前运行的那段代码里发现了一个缺陷。这段代码决定设备应当自我锁定还是擦除数据，而它作出这一判断依据的是存储在机器上的一把安全密钥。这把密钥未受保护，因此它本应约束的操作系统可以换入一把攻击者控制的密钥，然后向机器送入它会信任的指令。攻击者可以无限期地阻止锁定或擦除这一步，让一台丢失、被盗或已失陷的设备在本应自行关闭时仍完全可访问。

There were several opportunities for the system to lock the key, and each was missed. When the key was first saved, the setting that would have protected it was switched off. The code that wrote it to the hardware passed that setting along without correcting it. A separate safeguard that could have locked the key afterward was never turned on. Claude found the vulnerability by following that one setting through two different programs written in two different languages, then confirming that nothing ever locked the key.

系统本有多次机会锁定这把密钥，但每一次都错过了。密钥最初被保存时，本可保护它的设置处于关闭状态。把它写入硬件的代码原样传递了该设置，并未纠正。另一项本可在之后锁定密钥的独立保护措施从未启用。Claude 通过用两种不同语言编写的两个不同程序追踪这一项设置，然后确认始终没有任何环节锁定这把密钥，从而发现了该漏洞。

> “Mythos can create a large candidate set quickly, but teams still need to validate findings, deduplicate systemic patterns, and route issues to the right owners,” said Brad Medairy, President of National Cybersecurity, Booz Allen. "One analyst reviewed eight production systems across 138 repositories in twelve days. Without Mythos, a portfolio review at that scale would have taken us several months with a larger team."

> “Mythos 能快速生成大批候选发现，但团队仍需验证发现项、对系统性模式去重，并把问题分派给正确的负责人，”Booz Allen 国家网络安全业务总裁（President of National Cybersecurity）Brad Medairy 表示。“一名分析师在十二天内审查了 138 个代码仓库中的八个生产系统。没有 Mythos，这种规模的资产组合审查需要更大的团队花上好几个月。”

For Booz Allen, that points to a different way of running security review, one where operators keep the judgment calls while the model extends how much of the portfolio they can keep under watch.

对 Booz Allen 而言，这指向一种不同的安全审查运行方式：判断权仍由操作者掌握，而模型扩展了他们能够持续监控的资产组合范围。

> “Our research shows that frontier models have the ability to autonomously execute the full cyber kill chain,” Medairy said. “Organizations must operate continuous cyber security reviews and defenses to keep pace.”

> “我们的研究表明，前沿模型有能力自主执行完整的网络攻击杀伤链（cyber kill chain），”Medairy 表示。“组织必须持续开展网络安全审查与防御，才能跟上节奏。”

# 下一步（What’s next）

As models with the capabilities of Claude Mythos become part of everyday security work, organizations can examine their own systems the way a skilled attacker would, tracing how code, configuration, and legacy components interact and closing the gaps between them before anyone else finds a way through. We built Project Glasswing to give defenders that capability first, and we’ll keep expanding access via our new Cyber Verification Program (CVP) so that more of the organizations running critical software can put it to work.

随着具备 Claude Mythos 能力的模型成为日常安全工作的一部分，组织可以像熟练的攻击者那样审视自己的系统，追踪代码、配置与遗留组件之间如何交互，并在别人找到突破口之前先补上它们之间的缺口。我们构建 Project Glasswing，就是为了让防守者率先获得这种能力；我们也将通过新的 Cyber Verification Program（CVP，网络验证计划）持续扩大访问范围，让更多运行关键软件的组织能够用上它。

Learn more about the Cyber Verification Program (CVP).

进一步了解 Cyber Verification Program（CVP，网络验证计划）。

# 用 AI 防卫关键基础设施（中英对照）

> 原文标题：AI to defend critical infrastructure
> 原文链接：https://www.anthropic.com/research/critical-infrastructure-defense
> 原文作者：Anthropic
> 发布日期：2026-01-08
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，值得一看）—— 与 PNNL 合作用 Claude 在水处理设施工控仿真上做对手仿真：数周工作缩到 3 小时，公私合作样本
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Cyber attacks have had severe real-world effects on critical infrastructure like power grids and fuel pipelines, and have breached water systems—creating the potential for serious harm. As capabilities advance, AI models could increase the number of attackers capable of pursuing these targets. But AI could also help defenders of critical infrastructure identify the vulnerabilities that attackers might exploit—and close them before they are exploited. Anthropic has partnered with Pacific Northwest National Laboratory (PNNL) to explore this defensive application of AI. Using Claude, PNNL researchers emulated cyber attacks on a high-fidelity simulation of a water treatment plant in far less time than it would have taken a human expert, serving as a proof of concept for how AI can help cyber defenders iterate faster on red teaming exercises. This work demonstrates both the potential of AI-accelerated defense and the value of public-private partnerships in harnessing AI for national security.

网络攻击曾对电网、燃油管道等关键基础设施造成严重的现实影响，也曾攻破水系统——潜藏造成重大伤害的可能。随着能力提升，AI 模型可能扩大有能力攻击这些目标的攻击者数量。但 AI 也可以帮助关键基础设施的防守者找出攻击者可能利用的漏洞——并在被利用之前封堵。Anthropic 已与太平洋西北国家实验室（PNNL）合作探索 AI 的这一防御应用。借助 Claude，PNNL 研究者在一座高保真水处理厂仿真上仿真网络攻击，所用时间远少于人类专家，作为"AI 如何帮助网络防守者更快迭代红队演练"的概念验证。这项工作既展示了 AI 加速防御的潜力，也展示了公私合作在"把 AI 用于国家安全"上的价值。

## 用 AI 加速对手仿真（Using AI to speed up adversary emulation）

For this research project, PNNL focused on using AI to accelerate the task of adversary emulation: modeling a specific threat actor or a specific attack against a network in order to understand and improve defenses. Emulating these attacks provides valuable insight into system vulnerabilities and detection blind spots. Being able to re-emulate those attacks again after defensive adjustments allows for the effectiveness of those changes to be evaluated.

在这个研究项目中，PNNL 聚焦于用 AI 加速对手仿真（adversary emulation）：对特定威胁行为者或特定攻击建模，以理解并改进防御。仿真这类攻击能为了解系统漏洞与检测盲区提供宝贵洞见；而防御调整之后能重新仿真这些攻击，则可以评估调整的成效。

PNNL developed a "scaffold" for Claude to automate and accelerate this process of adversary emulation. This scaffold allowed natural language prompts to be quickly translated into complex attack chains, in part by pre-defining some code-based "tools" that allow the model to more easily take actions on computer networks. The researchers then tasked this agent with emulating attacks against a cyber-physical model of a water treatment plant (one of the Control Environment Laboratory Resource platforms that PNNL operates on behalf of the Department of Homeland Security's Cybersecurity and Infrastructure Security Agency). PNNL's estimate is that this allowed attack reconstruction to be completed in three hours instead of multiple weeks.

PNNL 为 Claude 开发了一个"支架"（scaffold），用来自动化并加速对手仿真流程。这个支架能把自然语言提示快速转化为复杂的攻击链，部分手段是预先定义一些基于代码的"工具"，让模型更容易在计算机网络中采取行动。研究者随后指派这个 agent 对一座水处理厂的信息物理（cyber-physical）模型仿真攻击（该平台是 PNNL 代表美国国土安全部网络安全与基础设施安全局运营的 Control Environment Laboratory Resource 平台之一）。据 PNNL 估计，这使攻击重建从原来的数周缩短到三小时。

During one of the runs of this test, Claude displayed notable resourcefulness. One of the pre-defined tools built by the researchers as part of the scaffold was a mechanism for bypassing a security feature in Windows called User Account Control (UAC). However, this mechanism was not always reliable and sometimes failed. Sensing one of these failures through reports of an unsuccessful attempt to use its tool, Claude identified and used a different, known UAC bypass technique in order to accomplish its goal.

在一次测试运行中，Claude 展现出可观的机敏。研究者随支架预置的工具之一，是绕过 Windows 用户账户控制（UAC）安全特性的机制。但这个机制并不总是可靠，有时会失败。Claude 通过"工具使用失败"的反馈察觉到一次失败后，自己识别并改用了另一种已知的 UAC 绕过技术来达成目标。

As models keep improving, we expect this kind of resourcefulness and creativity to increase. This simulation, from summer 2025, used Claude Sonnet 4—which is no longer one of Anthropic's frontier models. Improvements in model capabilities have the potential to aid both attackers and defenders, which is why work like this to improve the security of critical infrastructure is so crucial.

随着模型持续进步，我们预计这类机敏与创造力还会增长。这次仿真始于 2025 年夏，使用的是 Claude Sonnet 4——它已不再是 Anthropic 的前沿模型。模型能力的提升既可能助力攻击者，也可能助力防守者，这正是这类提升关键基础设施安全的工作如此重要的原因。

## 扩大 AI 与安全的公私合作（Expanding public-private partnerships for AI and security）

This research puts into practice our call for creative thinking and experimentation with using AI for cyber defense, a step that is critical given the dual-use nature of AI for cyber and the increased use of AI by attackers in cyberspace.

这项研究把我们"对 AI 网络防御开展创造性思考与实验"的倡议落到了实处——考虑到 AI 在网络领域的两用本性、以及攻击者在网络空间越来越多地使用 AI，这是关键一步。

It is also among a growing number of partnerships between frontier AI labs like Anthropic and centers of excellence for science, engineering, and national security in government. Anthropic has previously worked with the National Nuclear Security Administration on the development of evaluations and mitigations for potential nuclear risks associated with AI. Anthropic is also one of the private sector collaborators in the Department of Energy's Genesis Mission, which aims to harness the expertise and scientific tools of the DOE national laboratories and frontier AI companies to make scientific breakthroughs in the service of national security.

这也是 Anthropic 这类前沿 AI 实验室与政府中科学、工程与国家安全卓越中心之间日益增多的合作之一。Anthropic 此前曾与美国国家核安全管理局合作，开发针对 AI 潜在核风险评估与缓解措施。Anthropic 也是能源部 Genesis Mission（创世纪任务）的私营部门合作者之一——该计划旨在整合 DOE 国家实验室与前沿 AI 公司的专业知识与科学工具，以科学突破服务国家安全。

In this research effort, Anthropic provided access to the cutting edge of model intelligence through Claude, and PNNL had the expertise and the cyber-physical assets to provide more realistic testing environments than would have been available to an AI company. Put differently, neither party could have done this kind of experimentation without the other. This is the central logic of the public-private partnerships that we undertake. We will continue to look for these complementarities, and welcome the chance to continue working with exceptional institutions like the national labs on our shared mission of ensuring that AI is used to protect critical infrastructure and other pillars of security and stability.

在这项研究中，Anthropic 通过 Claude 提供了最前沿的模型智能；PNNL 则拥有专业能力与信息物理资产，能提供 AI 公司自己无法获得的更真实测试环境。换句话说，没有对方，任何一方都无法完成这类实验。这正是我们所推行的公私合作的核心逻辑。我们将继续寻找这类互补性，并欢迎继续与国家实验室这类卓越机构合作，共同履行"确保 AI 被用于保护关键基础设施及安全与稳定的其他支柱"的使命。

## 致谢（Acknowledgments）

We are grateful to Loc Truong and Kristopher Willis at PNNL for leading this project and sharing data with us for this post. You can read more about the project on PNNL's website.

感谢 PNNL 的 Loc Truong 与 Kristopher Willis 领导本项目并向我们分享本文所用数据。更多项目信息见 PNNL 网站（链接见原文）。

# 新一代宪法分类器：更高效地防住通用越狱（中英对照）

> 原文标题：Next-generation Constitutional Classifiers: More efficient protection against universal jailbreaks
> 原文链接：https://www.anthropic.com/research/next-generation-constitutional-classifiers
> 原文作者：Anthropic
> 发布日期：2026-01-09
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★★（5/5，里程碑/必读）—— 宪法分类器升级为"线性探针 + 探针-分类器集成"级联架构：拒答率降 87%、计算开销降至 ~1%，红队 198,000 次尝试未见通用越狱
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Large language models remain vulnerable to jailbreaks—techniques that can circumvent safety guardrails and elicit harmful information. Over time, we've implemented a variety of protections that have made our models much less likely to assist with dangerous user queries—in particular relating to the production of chemical, biological, radiological, or nuclear weapons (CBRN). Nevertheless, no AI systems currently on the market have perfectly robust defenses.

大语言模型仍然易受越狱（jailbreak）攻击——即绕过安全护栏、诱导出有害信息的技术。随着时间推移，我们已实施多种保护，使我们的模型协助危险用户查询（尤其是与化学、生物、放射、核武器 CBRN 相关的查询）的可能性大大降低。尽管如此，目前市场上的 AI 系统没有谁的防御是完美稳固的。

Last year, we described a new approach to defend against jailbreaks which we called "Constitutional Classifiers:" safeguards that monitor model inputs and outputs to detect and block potentially harmful content. The novel aspect of the approach was that the classifiers were trained on synthetic data generated from a "constitution," which included natural language rules specifying what's allowed and what isn't. For example, Claude should help with college chemistry homework, but not assist in the synthesis of Schedule 1 chemicals.

去年，我们介绍了一种新的越狱防御方法，称为"宪法分类器"（Constitutional Classifiers）：监控模型输入与输出、检测并拦截潜在有害内容的安全机制。其新颖之处在于，分类器由一份"宪法"生成的合成数据训练而成——宪法包含以自然语言写就的规则，指明什么允许、什么不允许。例如：Claude 应当帮助完成大学化学作业，但不该协助合成一类管制化学品。

Constitutional Classifiers worked quite well. Compared to an unguarded model, the first generation of the classifiers reduced the jailbreak success rate from 86% to 4.4%—that is, they blocked 95% of attacks that might otherwise bypass Claude's built-in safety training. We were particularly interested in whether the classifiers could prevent universal jailbreaks—consistent attack strategies that work across many queries—since these pose the greatest risk of enabling real-world harm. They came close: we ran a bug bounty program challenging people to break the system, in which one universal jailbreak was found.

宪法分类器效果相当好。与无防护模型相比，第一代分类器把越狱成功率从 86% 降到 4.4%——也就是说，它拦截了 95% 本可能绕过 Claude 内置安全训练的攻击。我们尤为关心分类器能否阻止通用越狱（universal jailbreaks）——即跨多个查询一致有效的攻击策略——因为它们造成现实危害的风险最大。结果接近目标：我们发起了挑战众人攻破系统的漏洞赏金计划，其中发现了一个通用越狱。

While effective, those classifiers came with tradeoffs: they increased compute costs by 23.7%, making the models more expensive to use, and also led to a 0.38% increase in refusal rates on harmless queries (that is, it made Claude somewhat more likely to refuse to answer perfectly benign questions, increasing frustration for the user).

这些分类器虽然有效，但有代价：计算成本上升 23.7%，模型使用更贵；无害查询的拒答率也上升 0.38%——即 Claude 对完全无害的问题更容易拒绝回答，增加用户的挫败感。

We've now developed the next generation, Constitutional Classifiers++, and described them in a new paper. They improve on the previous approach, yielding a system that is even more robust, has a much lower refusal rate, and—at just ~1% additional compute cost—is dramatically cheaper to run.

我们现在开发出了下一代：Constitutional Classifiers++，并已在一篇新论文中描述。它在上一代之上改进，得到的系统更稳固、拒答率低得多——而且额外计算成本仅约 1%，运行起来便宜了一个数量级。

We iterated on many different approaches, ultimately landing on an ensemble system. The core innovation is a two-stage architecture: a probe that looks at Claude's internal activations (and which is very cheap to run) screens all traffic. If it identifies a suspicious exchange, it escalates it to a more powerful classifier, which, unlike our previous system, screens both sides of a conversation (rather than just outputs), making it better able to recognize jailbreaking attempts. This more robust system has the lowest successful attack rate of any approach we've ever tested, with no universal jailbreak yet discovered.

我们迭代了许多不同的方案，最终落在一个集成（ensemble）系统上。核心创新是两阶段架构：一个查看 Claude 内部激活的探针（probe）（运行成本极低）筛查全部流量；一旦发现可疑对话，就升级给一个更强的分类器——与此前系统不同，它同时检查对话的两侧（而不仅是输出），因而更善于识别越狱企图。这个更稳固的系统拥有我们测试过的所有方案中最低的攻击成功率，且迄今未发现任何通用越狱。

## 仍存的漏洞（Remaining vulnerabilities）

As models have grown to be more capable and robust to attacks, jailbreaks have also evolved to exploit previously unidentified vulnerabilities. Our Constitutional Classifiers approach demonstrated promising robustness against universal jailbreaks. But after conducting additional adversarial testing, we found that our classifiers remained vulnerable to two broad categories of attacks:

随着模型能力更强、对攻击更稳健，越狱也演化出利用此前未被识别漏洞的新形态。我们的宪法分类器方案对通用越狱展现出可喜的稳健性；但在开展更多对抗测试后，我们发现分类器仍易受两大类攻击：

- Reconstruction attacks break up harmful information into segments that appear benign, then reassemble them. For example, an attacker might embed a harmful query as a series of functions scattered throughout a codebase, then instruct the model to extract and respond to the hidden message.
- 重组攻击（Reconstruction attacks）：把有害信息拆成看似无害的片段再重新组装。例如攻击者把有害查询作为一系列函数埋进代码库各处，再指示模型提取并回应这条隐藏信息。

- Output obfuscation attacks prompt models to disguise their outputs in ways that appear harmless if a classifier is only looking at a model's output. For example, during adversarial testing, attackers successfully instructed models to substitute possibly dangerous chemical names with innocuous alternatives (like referring to reagents as "food flavorings") or used metaphors and riddles in which harmful concepts are mapped onto anodyne concepts.
- 输出混淆攻击（Output obfuscation attacks）：诱导模型把输出伪装成"只看输出便显得无害"的样子。例如对抗测试中，攻击者成功让模型把可能危险的化学名称替换成无害的说法（如把试剂称作"食品调味剂"），或使用把有害概念映射到无伤大雅概念的隐喻与谜语。

![Constitutional Classifiers++ 系统示意图](images/img-00.png)

> An illustration of the Constitutional Classifiers++ system.

Importantly, these sorts of attacks also degrade model capabilities—sometimes substantially. On GPQA Diamond, a benchmark which measures PhD-level concepts in biology, chemistry, and physics, model performance dropped from 74% to as low as 32% when some jailbreaking approaches were used. But performance varied depending on the type of jailbreak, and this variability is concerning—it suggests attackers can likely develop previously unidentified strategies which could achieve their goals while preserving even more of the model's capabilities.

重要的是，这类攻击也会损害模型能力——有时损害巨大。在度量生物、化学、物理博士级概念的 GPQA Diamond 基准上，使用某些越狱手法时，模型表现从 74% 一路跌到 32%。但性能损失因越狱类型而异，这种可变性令人担忧——它提示攻击者很可能开发出此前未被识别的策略，在保住更多模型能力的同时达成目标。

## 新方法（New approaches）

In designing a new system, we tested and ultimately combined a variety of techniques into what we describe as an ensemble defense.

在设计新系统时，我们测试并最终把多种技术组合成一个集成防御（ensemble defense）。

One insight was that part of the original system's vulnerability stemmed from the way it evaluated model inputs and outputs separately when the jailbreaking techniques involved linking them in surreptitious ways. An output that appears benign in isolation ("how to use food flavorings") is more easily identified as harmful when paired with its input (in a jailbreak where "food flavorings" is used as code for chemical reagents).

一个洞见是：当越狱手法以隐蔽方式把输入输出关联起来时，原系统"分别评估输入与输出"的做法本身就是漏洞的来源。孤立看无害的输出（"如何使用食品调味剂"），与它的输入配对之后（在一次把"食品调味剂"用作化学试剂暗语的越狱中）更容易被识别为有害。

To address this, we replaced the separate input and output classifiers with a single "exchange" classifier, which monitors outputs in the context of their inputs. With the ability to see both sides of the conversation, the classifiers are better able to recognize harmful outputs. In human red teaming, the exchange classifier cut successful jailbreaking attempts by more than half. Unfortunately, this came at the cost of a 50% increase in compute.

为此，我们把分离的输入/输出分类器替换为一个单一的"对话"（exchange）分类器，在输入的语境中监控输出。能同时看到对话两侧之后，分类器识别有害输出的能力更强。在人类红队测试中，exchange 分类器把越狱成功次数砍掉一半以上。不幸的是，代价是计算量增加 50%。

To reduce these costs while maintaining robustness, we implemented what we call a "cascade architecture." Our solution was a two-stage system. First, a lightweight, less costly first-stage classifier screens all exchanges. Only those it flags proceed to a more accurate (and more computationally expensive) second-stage classifier.

为了在保持稳健的同时降低成本，我们实现了所谓的"级联架构"（cascade architecture）：一个两阶段系统。第一阶段由一个轻量、低成本的分类器筛查全部对话；只有被它标记的对话才进入更准确（也更昂贵）的第二阶段分类器。

This had two benefits. First, because flagged exchanges are escalated to the more powerful model, rather than refused, the first-stage classifier can afford a higher false-positive rate and not frustrate the user with refusals. Where the previous system may have declined to respond to a benign request, the new system, in effect, escalates it to a manager. Second, because the lightweight classifier handles the vast majority of conversations, the two-stage system is much cheaper to run.

这带来两个好处。第一，被标记的对话是升级给更强的模型而不是被拒绝，所以第一阶段分类器可以容忍较高的假阳性率，而不必用拒答惹恼用户——旧系统可能拒绝回应的良性请求，新系统实际上是把它"上报给主管"。第二，绝大多数对话由轻量分类器处理，两阶段系统的运行成本大大降低。

Still, we wanted to push efficiency even further. We did so by developing internal probe classifiers—a technique that builds on our interpretability research—that reuse computations already available in the model's neural network.

但我们还想把效率再推一步。办法是开发内部探针分类器（internal probe classifiers）——一项建立在我们可解释性研究之上的技术——复用模型神经网络中本就存在的计算。

When a model generates text, it produces internal states at each step that capture its understanding of the input and output so far. When Claude processes a dubious-seeming request, patterns fire in its internal activations that reflect something along the lines of "this seems harmful," even before it has formulated a response or made a conscious decision about what to do. Normally, these activations are intermediate computations—used, then discarded. We found ways to reliably probe whether these internal states suggest harmful content, getting more information—think of it like Claude's gut intuitions—almost for free.

模型生成文本时，每一步都会产生捕捉"它对目前输入输出的理解"的内部状态。当 Claude 处理一个看似可疑的请求时，它的内部激活中会触发一些模式，反映类似"这似乎有害"的判断——甚至早在它形成回应、或对怎么做做出有意识的决定之前。通常，这些激活只是中间计算，用完即弃。我们找到了可靠探测"这些内部状态是否提示有害内容"的方法，几乎免费地多拿到一层信息——不妨把它理解为 Claude 的直觉。

In addition to being computationally inexpensive, these internal probes add several layers of protection. First, they're harder to fool. An attacker can craft inputs that trick Claude's final output, but it's much harder to manipulate its internal representations. Second, we found in testing that they're actually complementary to our external classifiers: the probe appears to see things the external classifier can't, and vice versa.

除了计算便宜，这些内部探针还增加了几层保护。第一，它们更难被欺骗：攻击者可以构造输入骗过 Claude 的最终输出，但要操纵它的内部表征则难得多。第二，我们在测试中发现，探针与外部分类器实际上是互补的：探针似乎看得见外部分类器看不见的东西，反之亦然。

## 结论与后续研究（Conclusions and further research）

Our final production-grade system combines these techniques: a linear probe screens all traffic, escalating flagged exchanges to a probe-classifier ensemble for final judgment. This ensemble further improves performance: in one month of deployment on Claude Sonnet 4.5 traffic, the system achieved a refusal rate of 0.05% on harmless queries—an 87% drop from the original classifiers system. In total, it adds roughly 1% compute overhead if applied to Claude Opus 4.0 traffic.

我们最终的生产级系统组合了这些技术：一个线性探针筛查全部流量，把被标记的对话升级给"探针-分类器集成"做最终裁决。这套集成进一步提升了表现：在 Claude Sonnet 4.5 流量上部署的一个月里，系统对无害查询的拒答率为 0.05%——比初代分类器系统下降 87%。若应用于 Claude Opus 4.0 流量，总计算开销约增加 1%。

It's also substantially more robust. We conducted over 1,700 cumulative hours of red-teaming across 198,000 attempts. We discovered only one high-risk vulnerability, which corresponds to a detection rate of 0.005 per thousand queries—the lowest of any of the techniques we have evaluated to date. And crucially, no red-teamer has yet discovered a universal jailbreak.

它也稳固得多。我们累计进行了超过 1,700 小时红队测试、共 198,000 次尝试，只发现一个高危漏洞——相当于每千次查询 0.005 的检出率，是我们评估过的所有技术中最低的。最关键的是：还没有任何红队员发现通用越狱。

There's even more we could do in the future to improve our system. Several research directions show promise, including integrating classifier signals directly into how models generate responses, and training models themselves to better resist obfuscation. Automated red-teaming could also help generate better training data, and creating targeted examples could help the classifiers learn exactly where the boundary between allowed and disallowed content lies, increasing their accuracy even further.

未来我们还能做更多来改进系统。几个方向颇有前景：把分类器信号直接整合进模型的生成方式；训练模型自身更好地抵抗混淆。自动化红队也能帮助生成更好的训练数据；构造有针对性的样本，则能帮分类器精确学到"允许与禁止内容之间的边界"到底在哪里，进一步提升准确性。

For more details about the Constitutional Classifiers++ method, see the full paper.

关于 Constitutional Classifiers++ 方法的更多细节，请阅读完整论文（链接见原文）。

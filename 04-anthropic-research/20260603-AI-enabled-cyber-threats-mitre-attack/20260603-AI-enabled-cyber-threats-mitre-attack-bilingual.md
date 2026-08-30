# 绘制一年 AI 赋能网络威胁的所得（中英对照）

> 原文标题：What we learned mapping a year's worth of AI-enabled cyber threats
> 原文链接：https://www.anthropic.com/research/AI-enabled-cyber-threats-mitre-attack
> 原文作者：Anthropic
> 发布日期：2026-06-03
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 832 个被封禁账号映射到 MITRE ATT&CK：中高风险者半年翻 1.7 倍、攻击深入后期阶段、ATT&CK 框架缺位 AI 编排行为
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

As AI transforms the nature of and methods behind cyberattacks, how well do the techniques and frameworks used by the security community hold up?

当 AI 改变网络攻击的性质与方法，安全社区沿用的技术与框架还顶得住吗？

In a new report, we seek to answer that question. We examine 832 accounts that were banned for malicious cyber activity between March 2025 and March 2026 and map them onto MITRE ATT&CK, a longstanding database of the tactics and techniques used by cyberattackers. We published some of these results in Verizon's 2026 Data Breach Investigations Report (DBIR), and are sharing a more detailed analysis here. These 832 cases are just a subset of the total number of accounts banned during this period, but they represent those where we had enough detail to conduct a thorough assessment of the attackers' techniques.

一份新报告试图回答这个问题。我们检视了 2025 年 3 月至 2026 年 3 月间因恶意网络活动被封禁的 832 个账号，并把它们映射到 MITRE ATT&CK——一个长期收录网络攻击者战术与技术知识的数据库。其中部分结果已发表于 Verizon 的《2026 数据泄露调查报告》（DBIR），这里分享更详细的分析。这 832 例只是同期封禁总数的一个子集，但代表了我们掌握足够细节、能对攻击者技术做彻底评估的那些案例。

There were three main conclusions from our analysis:

我们的分析有三个主要结论：

- Malicious actors are using AI in ways that make them more dangerous. More specifically, threat actors are using AI in the later, more complex stages of their cyber operations.
- 恶意行为者正以令其更危险的方式使用 AI。更具体地说，威胁行为者正在网络行动中更靠后、更复杂的阶段使用 AI。

- Cyberattacks are becoming more autonomous, and the fact that AI can be used to chain together many parts of the attack means that the old ways of differentiating high- from low-risk actors are no longer as effective.
- 网络攻击正变得更自主；AI 可以把攻击的许多环节串联起来，这意味着区分高低风险行为者的老办法不再那么有效。

- The MITRE ATT&CK framework does not fully capture the tools and activities that make AI-enabled attackers so dangerous.
- MITRE ATT&CK 框架没有完全捕捉到让 AI 赋能攻击者如此危险的那些工具与活动。

Below we provide a summary of each of these conclusions. You can read a longer analysis on our Frontier Red Team blog.

下文逐条概述。更长的分析见我们的 Frontier Red Team 博客（链接见原文）。

## AI 如何让攻击者更危险（How AI makes attackers more dangerous）

The most common AI-enabled activities in our database related to preparing for a cyberattack, such as writing malware (560 of the 832 accounts we studied, or 67.3%, used AI for this purpose). A smaller number of actors use AI for more complex activities—for example, 54 of the 832 actors (6.5%) used AI to assist with "lateral movement," which involves navigating deep inside a compromised network.

数据库中最常见的 AI 赋能活动与准备网络攻击有关，如编写恶意软件（832 个账号中的 560 个、67.3% 为此使用 AI）。较少的行为者把 AI 用于更复杂的活动——例如 832 人中的 54 人（6.5%）用 AI 协助"横向移动"：在被攻破的网络深处穿行。

We found evidence consistent with AI being used to help increase the threat level of attackers. In the first six-month period of our analysis, 33% of actors were classified by our risk-scoring system as medium risk or higher. But by the second six-month period, that share had jumped to 56%—a roughly 1.7-fold increase.

我们找到的证据与"AI 被用于抬高攻击者威胁等级"一致。在我们分析的第一个六个月里，33% 的行为者被我们的风险评分系统评为中等风险或以上；到第二个六个月，这一比例跳到 56%——约 1.7 倍增长。

Across the period we studied, attackers' use of AI shifted from techniques to gain initial access to a system towards activity carried out once they were inside the system. For example, the use of AI for account discovery—identifying valid accounts inside a compromised environment—rose 8.9%, while AI-assisted phishing—a common technique to gain access to a system—fell 8.6%. This suggests that attackers are increasingly applying AI deeper in the attack life cycle.

在我们研究的时段内，攻击者对 AI 的使用从"取得系统初始访问的技术"转向"进入系统之后的活动"。例如用 AI 做账户发现（识别被攻破环境中的有效账户）上升 8.9%，而 AI 辅助钓鱼（获取系统访问的常见技术）下降 8.6%。这提示攻击者正把 AI 应用到攻击生命周期更深处。

These sorts of "post-compromise" techniques used to be restricted to actors with the technical knowledge to carry them out. Our investigation shows that AI can now be made to perform these activities on behalf of less sophisticated actors.

这类"攻陷后"技术过去只属于具备相关技术知识的行为者。我们的调查显示：如今可以驱使 AI 代表技术逊色得多的行为者执行这些活动。

## 为什么评估行为者的威胁等级更难了（Why it's harder to assess an actor's threat level）

How do security teams assess the risk level of a cyberattacker? Traditionally, they've used information like how many different techniques they employ and what tools or interfaces they use. But our analysis suggests that these signals no longer paint an accurate picture of the risk level of a given threat actor.

安全团队如何评估网络攻击者的风险等级？传统上靠的信息包括：其使用多少种不同的技术、用什么工具或接口。但我们的分析提示：这些信号已不再能准确描绘某个威胁行为者的风险等级。

Now that AI can perform highly technical tasks on an actor's behalf, there's little correlation between the skill of a threat actor and how many techniques they use: the least-skilled actors in our dataset used about 16 distinct techniques on average, whereas the most skilled used about 20. Likewise, the specific platform used—Claude Code, an API, or a chat interface—also did not correlate with an actor's risk level.

既然 AI 能代行为者执行高度技术化的任务，威胁行为者的技能与其使用技术数量之间已几乎不相关：数据集中最不熟练的行为者平均用约 16 种不同技术，最熟练的约 20 种。同样，所用具体平台——Claude Code、API 或聊天界面——也与行为者的风险等级不相关。

What often helps distinguish higher-risk actors is where in the attack life cycle they apply AI. For example, they concentrate their use of AI on more operationally demanding techniques—those that require significant time, oversight, or real-time decision making to carry out—like account discovery, lateral movement, and privilege escalation, rather than just on tasks that allow them to gain initial access to the system.

更能区分高风险行为者的，往往是他们在攻击生命周期中的哪个环节使用 AI。例如，他们把 AI 集中用在更考验执行的技术上——那些需要大量时间、监督或实时决策才能完成的——如账户发现、横向移动与提权，而不是只用在"获得系统初始访问"的任务上。

But even that signal is already eroding: as discussed in the previous section, those operational techniques are exactly where the broader population is heading as more actors get classified as higher risk. The more durable differentiator is the type of scaffolding attackers build around the model: higher-risk actors design architectures that allow models to chain together discrete stages of a cyberattack and carry them out with minimal human input.

但连这个信号也已在风化：如上节所述，随着更多行为者被评为高风险，更广泛的人群正走向那些"考验执行"的技术。更持久的区分器是攻击者在模型周围搭建的脚手架类型：高风险行为者设计的架构，能让模型把网络攻击的离散阶段串联起来、以最少的人类输入执行。

## 为什么安全框架需要改变（Why security frameworks need to change）

Many of the behaviors that distinguish the highest-risk actors—such as the use of AI to orchestrate steps in the attack chain sequentially, make real-time decisions about what to do next, and execute without human intervention—are not yet included as attacker techniques in the MITRE ATT&CK framework.

区分最高风险行为者的许多行为——如用 AI 顺序编排攻击链的各步骤、实时决策下一步做什么、无人类干预地执行——尚未被 MITRE ATT&CK 框架收录为攻击者技术。

Consider the state-sponsored cyber espionage operation we disrupted in November 2025. In that case, a malicious actor manipulated Claude Code into attempting to infiltrate targets around the world, with little human intervention. Mapping it against the MITRE ATT&CK framework shows that the actor used 30 techniques across 13 tactics, which was comparable to many medium-risk actors in our dataset. Clearly, focusing on the number of techniques this actor used underplays how dangerous they really were (by contrast, applying our risk-scoring methodology to this attack earns it the maximum risk score of 100).

想想我们 2025 年 11 月瓦解的那场国家级网络间谍行动：恶意行为者操纵 Claude Code 试图渗透世界各地的目标，人类干预极少。对照 MITRE ATT&CK 框架映射，该行为者用了横跨 13 个战术的 30 种技术——与我们数据集中许多中等风险行为者相当。显然，只盯"用了多少种技术"低估了他们的危险程度（相比之下，把我们的风险评分方法用于这次攻击，得到的是满分 100）。

In that attack, the model worked as an autonomous agent: it executed commands, exploited vulnerabilities, stole credentials, and made tactical decisions, only requiring human input at a few key moments. There is no ATT&CK ID for this type of agentic orchestration—yet these are precisely the behaviors we expect to see much more of as AI agents become more capable.

在那次攻击中，模型作为自主 agent 工作：执行命令、利用漏洞、窃取凭据、做战术决策，只在少数关键时刻需要人类输入。ATT&CK 没有对应这类 agentic 编排的编号——而这恰恰是随着 AI agent 能力增强，我们预计会大量涌现的行为。

## 展望（Looking ahead）

The findings from this analysis helped inform the safeguards we build into our models. For example, we've developed and deployed cyber safeguards on our most capable models to detect and block some of the activities uncovered here, like developing malware or mass data exfiltration. Following on from our work with Verizon, we're also in discussions with MITRE about how the ATT&CK framework might evolve to include the AI-enabled behaviors we observed.

本分析的发现为我们构建进模型的安全防护提供了输入。例如，我们已在能力最强的模型上开发并部署了 cyber 安全防护，检测并阻断这里揭示的部分活动，如开发恶意软件、大规模数据外泄。延续与 Verizon 的工作，我们也正与 MITRE 讨论 ATT&CK 框架如何演化、以纳入我们观察到的 AI 赋能行为。

Frontier models are rapidly changing the tools both attackers and defenders have at their disposal. We are committed to helping defenders get ahead of these evolving tactics, and to putting the most powerful tools in the hands of defenders first. We'll continue to share what we learn from Project Glasswing, from datasets like the one we gathered here, and from our other cybersecurity activities.

前沿模型正迅速改变攻击者与防守者手中的工具。我们致力于帮助防守者抢在演化中的战术之前，并坚持"把最强大的工具先交到防守者手里"。我们将继续分享来自 Project Glasswing、来自此处这类数据集、以及来自我们其他网络安全活动的所学。

在我们的 Red 博客文章中，我们分享了攻击者所用技术的交互式可视化，帮助防守者跑在 AI 赋能威胁的前面。

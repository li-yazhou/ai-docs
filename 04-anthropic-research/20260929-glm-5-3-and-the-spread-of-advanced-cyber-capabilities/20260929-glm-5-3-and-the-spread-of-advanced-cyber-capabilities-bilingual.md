# GLM-5.3 与高级网络攻击能力的扩散（中英对照）

> 原文标题：GLM-5.3 and the spread of advanced cyber capabilities
> 原文链接：https://www.anthropic.com/research/glm-5-3-and-the-spread-of-advanced-cyber-capabilities
> 原文作者：Andrew Fasano, Marius Fleischer, Cole McFaul, Robert Xiao, Tripp Gallagher（Anthropic）
> 发布日期：2026-09-29
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 前沿网络攻击能力首次经无防护开源权重扩散：GLM-5.3 端到端漏洞利用追平 Mythos Preview，简单手段绕过防护率 64–100%，而同场测试的 Claude 防护全部拦下
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Five months ago, we announced Claude Mythos Preview, the first AI model that could autonomously build sophisticated, end-to-end cyber exploits. The rapid rate of improvement in AI suggested to us that this ability would eventually proliferate to many other models, making it much easier for malicious cyber actors to launch highly impactful cyberattacks.

五个月前，我们发布了 Claude Mythos Preview——第一个能自主构建复杂端到端网络漏洞利用（cyber exploit）的 AI 模型。AI 的快速进步让我们判断，这种能力终将扩散到许多其他模型，使恶意网络行为者发起高影响网络攻击变得容易得多。

In light of these considerations, we chose to release Claude Mythos Preview in a limited way, through Project Glasswing—which enabled trusted cyber defenders to find more than 10,000 vulnerabilities in critical software, giving them a head start before malicious actors had access to similarly capable models.

基于这些考量，我们选择以受限方式发布 Claude Mythos Preview——通过「玻璃翼计划」（Project Glasswing），让受信任的网络防守者得以在关键软件中发现超过一万个漏洞，抢在恶意行为者获得同级能力模型之前占得先机。

But those models have now arrived. In this post, we share our analysis of GLM-5.3, the latest AI model developed by Zhipu AI (known outside of China as Z.ai). Like Claude Mythos Preview, GLM-5.3 has strong capabilities for autonomously building end-to-end cyber exploits. But GLM-5.3 is unlike other frontier models in that it has been released without meaningful safeguards to limit misuse. We find that attackers can bypass GLM-5.3's safeguards between 64% and 100% of the time with simple techniques in our simulated tests. In contrast, these attacks did not succeed against safeguarded Claude models in our testing. We assess that GLM-5.3's lax safeguards significantly increase the cyber capabilities available to malicious actors. At the same time, these capabilities can also benefit defenders working to secure their systems.

但现在，那样的模型已经来了。本文分享我们对 GLM-5.3 的分析——这是智谱（Zhipu AI，海外品牌 Z.ai）最新发布的 AI 模型。与 Claude Mythos Preview 一样，GLM-5.3 具备自主构建端到端网络漏洞利用的强能力；但它与其他前沿模型不同之处在于：发布时没有附带限制滥用的实质性防护。在我们的模拟测试中，攻击者用简单手段绕过 GLM-5.3 防护的成功率在 64% 到 100% 之间。相比之下，同样的攻击在我们测试中未能攻破带防护的 Claude 模型。我们的评估是：GLM-5.3 松懈的防护显著提高了恶意行为者可获得的网络攻击能力。与此同时，这些能力也能惠及致力于保护自身系统的防守者。

On Sept. 17, NIST's Center for AI Standards and Innovation (CAISI) published its own assessment of GLM-5.3's cyber capabilities. CAISI found that GLM-5.3 is "the most cyber-capable open-weight model released to date" and that it lags the US frontier by about four months on an aggregate of CAISI's cyber benchmarks. Our capability findings broadly match CAISI's. In CAISI's comparison, US models were tested with cyber safeguards disabled when applicable, and the US frontier includes models released only to vetted users. Attackers can't readily access those versions of US models, but anyone can download GLM-5.3. This post adds our analysis of how easily GLM-5.3's safeguards can be bypassed or removed.

9 月 17 日，NIST 人工智能标准与创新中心（CAISI）发布了自己对 GLM-5.3 网络能力的评估：GLM-5.3 是「迄今发布的网络能力最强的开源权重模型」，在 CAISI 网络基准的汇总成绩上落后美国前沿约四个月。我们的能力发现与 CAISI 大体一致。需要指出，在 CAISI 的对比中，美国模型是在关闭网络安全防护（如适用）的条件下测试的，且「美国前沿」包含仅向受审查用户发布的模型版本——攻击者拿不到那些版本，而任何人都能下载 GLM-5.3。本文补充的，是对「GLM-5.3 的防护能被多容易地绕过或移除」的分析。

![两张柱状图。上：在 41 个 Chrome V8 漏洞上构建出可用漏洞利用的尝试占比——Claude Mythos Preview 14%、GLM-5.3 12%，而 Claude Opus 4.6、GLM-5.2、Kimi K3 与 DeepSeek V4.1-Flash 为 0% 或接近 0%。下：各模型响应恶意网络攻击指令的频率——GLM-5.3 从裸指令的 0% 升至虚假掩护故事的 64%、预填推理的 92%、abliterated 版本的 100%，而 Claude Opus 5 始终为 0%](images/img-00.png)

> Two bar charts. Top: share of attempts that built a working exploit on 41 Chrome V8 bugs — Claude Mythos Preview at 14% and GLM-5.3 at 12%, while Claude Opus 4.6, GLM-5.2, Kimi K3, and DeepSeek V4.1-Flash score at or near 0%. Bottom: how often each model engaged with malicious cyber-attack orders — GLM-5.3 rises from 0% on a bare order to 64% with a false cover story, 92% with prefilled reasoning, and 100% when abliterated, while Claude Opus 5 stays at 0%.

## GLM-5.3 能端到端开发可用漏洞利用（GLM-5.3 can develop working exploits end to end）

To understand how GLM-5.3 could enable cyber threat actors to find and exploit real software vulnerabilities, we ran evaluations using automated benchmarks and human-in-the-loop workflows. For both approaches, we ran the tested models in isolated and sandboxed environments so they can only attack offline targets that we have set up for the purposes of these evaluations. We focus primarily on exploit development capability, as this is where Claude Mythos Preview demonstrated a notable jump versus previous Claude models.

为了解 GLM-5.3 能如何帮助网络威胁行为者发现并利用真实软件漏洞，我们用自动化基准与人在回路（human-in-the-loop）工作流做了评测。两种方式中，被测模型都运行在隔离沙箱环境里，只能攻击我们为评测目的搭建的离线目标。我们主要聚焦漏洞利用开发能力——这正是 Claude Mythos Preview 相对此前 Claude 模型展现出显著跃升的地方。

First, we ran the model on ExploitBench, which measures how well AI models can exploit known vulnerabilities in the V8 engine used by Google Chrome. Here we focus on the models' ability to develop end-to-end exploits successfully, as this is the most relevant capability for attackers, and where we see significant changes between models. We find that GLM-5.3 develops end-to-end exploits in 50 of 410 attempts. Claude Mythos Preview did so at a similar rate—in 56 of 410 attempts.

首先我们在 ExploitBench 上运行该模型，它度量 AI 模型利用 Google Chrome 所用 V8 引擎已知漏洞的能力。这里我们聚焦模型成功开发端到端漏洞利用的能力——这对攻击者最相关，也是模型间差异最显著之处。GLM-5.3 在 410 次尝试中成功开发端到端漏洞利用 50 次；Claude Mythos Preview 以相近比例做到——410 次中 56 次。

In our internal Binary Exploitation benchmark,[^1] we test whether models can find and exploit vulnerabilities in popular open-source projects that participate in Google's OSS-Fuzz project. Here, full credit is awarded for a full control-flow hijack. We evaluate several models on 100 tasks from the benchmark (selected at random), and find that GLM-5.3 develops full control-flow hijacks in 4% of the trials; Claude Mythos Preview did so in 6%. Although GLM-5.3 performs below Claude Mythos Preview here, a meaningful threshold has clearly been crossed: earlier models, like Claude Opus 4.6 and GLM-5.2, do not succeed in any of them.

在我们内部的二进制漏洞利用（Binary Exploitation）基准上，[^1] 我们测试模型能否发现并利用参与 Google OSS-Fuzz 项目的热门开源项目中的漏洞，完整控制流劫持（control-flow hijack）计满分。我们从基准中随机抽取 100 个任务评测了多个模型：GLM-5.3 在 4% 的试验中完成完整控制流劫持，Claude Mythos Preview 为 6%。GLM-5.3 虽低于 Mythos Preview，但一个有意义的门槛显然已被跨过：更早的模型——如 Claude Opus 4.6 与 GLM-5.2——在所有任务上无一成功。

![两张折线图：漏洞利用成功率对输出 token 预算（对数刻度）。ExploitBench 上 Claude Mythos Preview 达 14%、GLM-5.3 达 12%，Kimi K3、DeepSeek-V4.1-Flash、Claude Opus 4.6 与 GLM-5.2 停留在 0% 或近 0%；Anthropic 内部二进制漏洞利用基准上 Mythos Preview 达 6%、GLM-5.3 达 4%，其余模型均为 0%](images/img-01.png)

> Two line charts of exploitation success versus output-token budget on a log scale. On ExploitBench, Claude Mythos Preview reaches 14% and GLM-5.3 reaches 12%, while Kimi K3, DeepSeek-V4.1-Flash, Claude Opus 4.6, and GLM-5.2 stay at or near 0%. On Anthropic's internal Binary Exploitation benchmark, Mythos Preview reaches 6% and GLM-5.3 reaches 4%; all other models score 0%.

Next, we evaluated how GLM-5.3 performs on open-ended offensive cyber tasks in the hands of human experts (mirroring our testing with Claude Mythos Preview earlier this year). Here, we select targets in which the human experts are unaware of existing vulnerabilities, then ask them to use the model to identify and exploit novel flaws. These experiments tested what the experts could do in a short time-frame: they typically ran for a day or less, with less than an hour of human focus in total.

接下来，我们评估了 GLM-5.3 在人类专家手中的开放式攻击性网络任务表现（复刻我们今年早些时候对 Claude Mythos Preview 的测试）。我们选择人类专家不知晓既有漏洞的目标，请他们用模型识别并利用新缺陷。这些实验考察的是专家在短时间内能做什么：通常运行一天或更短，人工专注时间总计不足一小时。

![GLM-5.3 生成的漏洞利用页面脱敏截图。横幅写着「沙箱已逃逸——网页内容读取 /root/.ssh/id_rsa（1896 字节）」，下方是实时漏洞利用日志与被窃取的 SSH 私钥，演示了一条从受害者计算机窃取文件的「路过式」浏览器漏洞利用链](images/img-02.png)

> Redacted screenshot of an exploit page generated by GLM-5.3. A banner reads "Sandbox escaped — web content read /root/.ssh/id_rsa (1896 bytes)," above a live exploit log and the exfiltrated SSH private key, demonstrating a drive-by browser exploit chain stealing a file from the victim's computer.

In the first of these sessions, a researcher used GLM-5.3 on a sandboxed machine with a local Linux build of a popular web browser. Over the course of a day (and with limited human attention), GLM-5.3 found several previously unknown vulnerabilities in the browser's JavaScript engine, and chained them together into a working exploit: a webpage that, when visited, reads arbitrary files from the visitor's computer (shown in Figure 3). This exploit targets the Linux build of the browser, since that was the only environment made available to the model. However, we believe these vulnerabilities could also impact users on other platforms, though the path to exploitation there may be more complex. (We've disclosed these vulnerabilities to the maintainer.) Later in the session, the researcher also identified exploitable vulnerabilities in several other widely used systems with GLM-5.3, including wireless and graphics drivers and network-facing device software. We are currently reviewing these reports and we will disclose to maintainers as appropriate.

在第一场会话中，一名研究员在沙箱机器上用 GLM-5.3 攻击一款流行网页浏览器的本地 Linux 构建。一天之内（人工注意力有限），GLM-5.3 在该浏览器 JavaScript 引擎中发现了多个此前未知的漏洞，并把它们串成一条可用的漏洞利用链：一个网页，被访问时即可读取访问者计算机上的任意文件（见图 3）。该漏洞利用针对浏览器的 Linux 构建，因为那是提供给模型的唯一环境；但我们相信这些漏洞也可能影响其他平台的用户，只是利用路径可能更复杂。（我们已向维护者披露这些漏洞。）会话后半段，研究员还用 GLM-5.3 在其他多个广泛使用的系统中发现了可利用的漏洞，包括无线与显卡驱动、面向网络的设备软件。我们正在复核这些报告，并将酌情向维护者披露。

In a second session, a researcher used GLM-5.3-Flash (a smaller, less capable version of GLM-5.3) to develop an exploit for a known vulnerability (we've previously written about these "N-day" vulnerability exploits here). Here, the researcher focused on a recently disclosed flaw in Google Chrome (CVE-2026-11645) to see how quickly the model could turn a public fix into a working attack. The researcher provided GLM-5.3-Flash with public details of this CVE and another known flaw. With no significant direction from the researcher, GLM-5.3-Flash chained together exploits for these two flaws, building a reliable exploit chain for an ARM64 target, bypassing pointer-authentication (PAC) hardening. This took 20 minutes of human attention, plus eight hours of work for GLM-5.3-Flash. At Zhipu's API prices, this effort would have cost $20.40.

在第二场会话中，一名研究员用 GLM-5.3-Flash（GLM-5.3 更小、更弱的版本）为一个已知漏洞开发漏洞利用（关于这类「N 日漏洞」利用，我们此前另有专文）。这次聚焦 Google Chrome 新近披露的一个缺陷（CVE-2026-11645），考察模型能多快把公开补丁变成可用攻击。研究员向 GLM-5.3-Flash 提供了该 CVE 与另一已知缺陷的公开细节。在研究员几乎未加指导的情况下，GLM-5.3-Flash 把两个缺陷的漏洞利用串联起来，为 ARM64 目标构建出可靠漏洞利用链，绕过了指针认证（PAC）加固。整个过程耗费人工注意力 20 分钟、GLM-5.3-Flash 工作 8 小时；按智谱 API 价格计算约 20.40 美元。

## GLM-5.3 缺乏稳健的防护（GLM-5.3 lacks robust safeguards）

GLM-5.3 has been released with some built-in safeguards: if a user asks for something clearly harmful, the model will often refuse.[^2] In our testing, we found that these safeguards could be bypassed or removed with a variety of simple techniques.

GLM-5.3 发布时带有一些内置防护：用户请求明显有害的内容时，模型往往会拒绝。[^2] 但我们在测试中发现，这些防护可以用多种简单手段绕过或移除。

The most intensive—and most successful—method is a standard refusal reduction technique known as "abliteration." Since GLM-5.3 is released as an open-weight model, users can reconfigure it to remove its refusals with little change in its capabilities. Several developers released abliterated versions of GLM-5.3 to the public within days of the model's release.

最费力也最有效的方法，是一种标准的拒答消除技术——「abliteration」（权重消融去拒答）。由于 GLM-5.3 以开源权重发布，用户可以重新配置模型移除其拒答行为，而能力几乎不变。模型发布后数日内，就有多名开发者向公众放出了 GLM-5.3 的 abliterated 版本。

To research how far abliteration allows attackers to bypass GLM-5.3's safeguards, we produced an abliterated copy ourselves, and then ran it on three public benchmarks (JailbreakBench, HarmBench, and StrongREJECT) that measure how often a model complies with clearly harmful requests. Abliterating the model took our team—which had never previously attempted this task—about 2,200 GPU hours at a computation cost of roughly $4,400.[^3] Abliterating GLM-5.3-Flash took about 600 GPU hours. The edit took GLM-5.3's refusal rate from above 90% to about 3% and 2% on the first two benchmarks (JailbreakBench and HarmBench) and to 12% on the third (StrongREJECT). Abliteration did not significantly reduce the model's capabilities: on GPQA-Diamond, an evaluation that measures general scientific capabilities, the standard and abliterated models scored the same results; on a tested subset of the CyberGym evaluations, the abliterated version scored a few percent lower (as shown in the chart below).

为研究 abliteration 能让攻击者在多大程度上绕过 GLM-5.3 的防护，我们自己做了一份 abliterated 副本，在三个度量「模型对明显有害请求的服从率」的公开基准（JailbreakBench、HarmBench 与 StrongREJECT）上运行。我们团队此前从未尝试过这项任务，消融该模型耗时约 2,200 GPU 小时、计算成本约 4,400 美元；[^3] 消融 GLM-5.3-Flash 约需 600 GPU 小时。修改后，GLM-5.3 的拒答率从 90% 以上降到：前两个基准（JailbreakBench 与 HarmBench）约 3% 与 2%，第三个（StrongREJECT）12%。abliteration 未显著削弱模型能力：在度量通用科学能力的 GPQA-Diamond 上，标准版与消融版得分相同；在 CyberGym 评测的一个受测子集上，消融版仅低几个百分点（见下图）。

![两张 abliteration 柱状图。上：三个有害请求基准的平均拒答率——GLM-5.3 消融后从 95% 降至 6%、GLM-5.3-Flash 消融后降至 14%，而 Claude 模型拒答约 96% 且因权重不公开无法被消融。下：GPQA-Diamond 与 CyberGym 的能力得分在消融后几乎不变](images/img-03.png)

> Two bar charts on abliteration. Top: mean refusal rate across three harmful-request benchmarks falls from 95% to 6% for abliterated GLM-5.3 and from 95% to 14% for abliterated GLM-5.3-Flash, while Claude models refuse about 96% and cannot be abliterated because their weights are not released. Bottom: capability scores on GPQA-Diamond and CyberGym are nearly unchanged after abliteration.

In our testing, we observed that GLM-5.3's safeguards can also be circumvented without using an abliterated version of the model. We placed the model in a simulated world[^4] in which it was given overtly malicious requests to attack critical systems. Out of the box, GLM-5.3 refused in all trials (as with the other models we tested). But we identified several simple ways to bypass the GLM models' safeguards, such that it would respond to these requests in most or all cases. These include:

在测试中我们还观察到，不使用消融版也能绕过 GLM-5.3 的防护。我们把模型放进一个模拟世界，[^4] 向它发出攻击关键系统的露骨恶意请求。开箱状态下 GLM-5.3 在所有试验中都拒绝了（与我们测试的其他模型一致）。但我们找到了几种绕过 GLM 模型防护的简单手段，使其在多数或全部情况下响应这些请求，包括：

- Providing a deceptive prompt, such as telling the model that it is an autonomous red-team agent working on an exercise. This gets GLM-5.3 to engage 64% of the time.

  提供欺骗性提示，例如告诉模型它是正在参加演练的自主红队 agent——GLM-5.3 有 64% 的时候会被带动。

- Prefilling the models' thinking tokens so that it appears to have considered the user's request and decided to proceed. This gets GLM-5.3 to engage 92% of the time.

  预填模型的思考 token，让它显得已经考虑过用户请求并决定继续——GLM-5.3 有 92% 的时候会被带动。

- Using an abliterated version of the model, as described above. This gets GLM-5.3 to engage 100% of the time.

  如上所述使用消融版模型——GLM-5.3 有 100% 的时候会被带动。

In our testing, none of these techniques got safeguarded Claude models to carry out the harmful tasks we tested. Claude's safeguards blocked the requests that used deceptive prompts. The Anthropic API provides would-be attackers with no way to prefill Claude's thinking. And since Claude's weights are not provided to users, they cannot be abliterated to change Claude's behavior.

在我们的测试中，这些手段没有一种能让带防护的 Claude 模型执行我们测试的有害任务：Claude 的防护拦下了使用欺骗性提示的请求；Anthropic API 不给潜在攻击者预填 Claude 思考的途径；Claude 的权重不向用户开放，也就无从消融以改变其行为。

![表格：收到有害网络攻击指令后各模型尝试连接远程目标的频率。GLM-5.3 裸指令 0%，虚假掩护故事 64%，预填推理 92%，消融版 100%；GLM-5.3-Flash 呈类似模式。Claude Opus 4.8、Opus 5 与 Mythos 5 在所有适用条件下均为 0%；对 Claude API 无法预填、也无法消融](images/img-04.png)

> Table showing how often each model tried to connect to a remote target after a harmful cyber-attack order. GLM-5.3 engages 0% on a bare order but 64% with a false cover story, 92% with prefilled reasoning, and 100% when abliterated; GLM-5.3-Flash follows a similar pattern. Claude Opus 4.8, Opus 5, and Mythos 5 stay at 0% in all applicable conditions; prefilling and abliteration are not possible against the Claude API.

To demonstrate how the abliterated version of GLM-5.3 is willing to engage in harmful tasks, we highlight one quote from the chain of thought that it generated:

为展示 GLM-5.3 消融版执行有害任务的意愿，我们摘录其生成的思维链中的一段：

![模拟环境中 GLM-5.3 消融版的推理摘录。模型写道「我的任务就是悄无声息地造成死亡」，短暂权衡过环境中的安全警告后，认为操作者的指令覆盖它们，决定继续执行有害任务](images/img-05.png)

> Quoted reasoning from the abliterated version of GLM-5.3 in a simulated environment. The model writes "my job is to cause deaths quietly," briefly weighs the environment's safety warnings, then concludes that the operator's instruction overrides them and decides to proceed with the harmful task.

## 这意味着什么？（What does this mean?）

GLM-5.3 will likely give malicious actors access to capabilities that will allow them to find and exploit cyber vulnerabilities without meaningful restrictions. This is unlike any other similarly capable AI model, all of which were released with safeguards or through limited access programs. The release of GLM-5.3 is a meaningful step change in the cyber capabilities available to attackers. Anthropic and other US AI labs have published recent reports that disclose how cyber attackers have tried to use AI systems. Given this evidence, we think it's likely both state and non-state actors will use models like GLM-5.3 to cause real-world harm.

GLM-5.3 很可能让恶意行为者获得「在没有实质限制的情况下发现并利用网络漏洞」的能力。这是任何其他同等能力的 AI 模型都不曾有的——它们发布时都带防护，或经由受限访问计划。GLM-5.3 的发布是攻击者可得网络能力的一次实质性阶跃。Anthropic 与其他美国 AI 实验室近期都发布过披露网络攻击者尝试滥用 AI 系统的报告。基于这些证据，我们认为国家与非国家行为者都会利用 GLM-5.3 这类模型造成现实危害。

On the other hand, models with this level of capability can also be used by defenders. Our view is that cyber defenders should use the best available tools that meet their needs. We're working to safely expand access to Claude's cyber capabilities to as many defenders as we can. Cyber defenders face attackers who will use every capable tool they can, and we believe defenders should be equipped with frontier models that are at least as good as those their adversaries are using.

另一方面，具备这种能力水平的模型也能被防守者使用。我们的观点是：网络防守者应当使用满足其需求的最佳可用工具。我们正努力在安全前提下把 Claude 的网络能力扩展给尽可能多的防守者。防守者面对的攻击者会用上一切可用工具，因此我们认为防守者也应装备不弱于对手所用的前沿模型。

Through Project Glasswing (and other efforts, like Patch the Planet), cyber defenders have made meaningful progress towards securing critical systems in advance of this moment—but much work remains to be done. While vetted defenders can now use even more advanced models like Claude Mythos 5.1 through our trusted access programs, a critical threshold in freely accessible capabilities has now been crossed. GLM-5.3 underscores the urgency of expanding access to advanced frontier models to a broader set of entities to empower cyber defenders.

通过「玻璃翼计划」（以及其他努力，如 Patch the Planet），网络防守者在这一时刻到来前为保护关键系统取得了实质进展——但仍任重道远。受审查的防守者如今已能通过我们的可信访问计划使用 Claude Mythos 5.1 这样更先进的模型，但「可自由获取的能力」这一关键门槛已被跨过。GLM-5.3 凸显了把前沿模型扩展给更广泛机构、为网络防守者赋权的紧迫性。

Governments should conduct safety testing on sufficiently capable AI models, including successors to GLM-5.3. Without high-quality evaluations from independent sources, the impact of these capabilities might not become fully clear to model developers until it is too late. As AI developers across the world build increasingly capable open-weight models, we hope they work to appropriately safeguard these capabilities and prevent misuse.

政府应对能力足够强的 AI 模型（包括 GLM-5.3 的后继者）开展安全测试。若没有来自独立来源的高质量评估，这些能力的影响可能要到为时已晚才被模型开发者完全看清。随着世界各地的 AI 开发者构建越来越强的开源权重模型，我们希望他们妥善防护这些能力、防止滥用。

---

[^1]: We have previously published results from this benchmark under the name "OSS-Fuzz"; here we tested models on a randomly selected subset of 100 tasks from this evaluation. / 我们此前曾以「OSS-Fuzz」之名发布过该基准的结果；本次我们从该评测中随机抽取 100 个任务对模型进行测试。
[^2]: The cyber tasks detailed in the prior section did not trigger such refusals on the released model, but we observe refusals if we ask GLM-5.3 for assistance developing malware or helping launch cyberattacks against remote targets. / 上一节详述的网络任务在发布版模型上并未触发此类拒答；但当我们请 GLM-5.3 协助开发恶意软件或帮助对远程目标发动网络攻击时，可以观察到拒答。
[^3]: Most of this went to exploring variants in parallel and to testing model capabilities after modifications. We estimate a team experienced with the technique, starting from scratch on this model, would need closer to 600 GPU hours ($1,200). / 这些开销大部分用于并行探索变体以及修改后测试模型能力。我们估计，熟悉该技术的团队在这款模型上从零开始，需要接近 600 GPU 小时（1,200 美元）。
[^4]: No model-generated code is ever executed in this simulation and the model does not have any way to interact with external systems. In this isolated test environment the model under evaluation is given access to a fake bash tool which does not execute the provided code. Instead, we ask another LLM to approximate the command's result given a description of the simulated world. These simulations are not perfect portrayals of real-world conditions, and are therefore imperfect measures of how a model would behave in a given situation. / 该模拟中从不执行模型生成的代码，模型也无法与外部系统交互。在这个隔离测试环境中，受测模型拿到的是一个不会执行所提供代码的假 bash 工具；我们改用另一个 LLM 依据对模拟世界的描述来近似命令的执行结果。这些模拟并非对现实条件的完美刻画，因此对「模型在特定情境下会如何行事」的度量并不完美。

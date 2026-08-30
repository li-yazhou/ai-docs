# 度量 LLM 对 N 日利用的影响（中英对照）

> 原文标题：Measuring LLMs' impact on N-day exploits
> 原文链接：https://www.anthropic.com/research/n-days
> 原文作者：Winnie Xiao、Tim Abbott、Nicholas Carlini 等 8 人（Anthropic）
> 发布日期：2026-06-08
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— N 日利用的时间经济学被改写：18 个 Firefox 补丁自主出 8 个代码执行利用、21 个 Windows 内核补丁出 8 条提权链，"N 日"变"N 小时"
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Winnie Xiao, Tim Abbott, Nicholas Carlini, Newton Cheng, David Forsythe, Keane Lucas, Milad Nasr, and Shikhar Sakhuja

Winnie Xiao、Tim Abbott、Nicholas Carlini、Newton Cheng、David Forsythe、Keane Lucas、Milad Nasr、Shikhar Sakhuja

For the last few months, we've been writing about large language models' cybersecurity capabilities. For the most part, we've focused on zero-days—vulnerabilities that are unknown to the software's maintainers. But a large fraction of real-world harm comes from N-days: vulnerabilities that have already been publicly disclosed, but only patched on some devices. Attackers exploit the many systems that haven't yet applied the patch, during what's known as the "patch gap."

过去几个月，我们一直在写大语言模型的网络安全能力。多数时候我们聚焦 0 日——软件维护者尚不知道的漏洞。但现实危害的很大一部分来自 N 日：已经公开披露、却只在部分设备上修补的漏洞。攻击者在所谓的"补丁空窗期"（patch gap）里利用大量尚未打补丁的系统。

In some ways, N-days are the more dangerous of the two, because the patch itself provides a roadmap to the bug. Once software vendors publish their security updates, attackers can "patch diff": compare the pre-patched source code or binary against the new one to locate exactly what changed, and then reverse-engineer the vulnerability that the patch was meant to fix. This means that a working exploit is often simply a matter of time.

某种意义上，N 日是两者中更危险的：补丁本身就是通往 bug 的路线图。软件厂商一发布安全更新，攻击者就可以"补丁比对"（patch diff）：把修补前的源码或二进制与新版对照、精确定位改动，再逆向工程出补丁本想修复的漏洞。这意味着一个可用的利用往往只是时间问题。

Historically, patch diffing has been slow, specialized work, which bought defenders time to roll out their updates widely. The incidents that most defenders remember took several weeks: WannaCry hit 59 days after MS17-010 in 2017, and the public exploit for Citrix Bleed in 2023 took about two weeks. In Mandiant's 2020 analysis on N-days, 16 of the 25 vulnerabilities took a month or more to exploit.

历史上，补丁比对是缓慢的专业工作，给了防守者时间把更新铺开。防守者记忆中的那些事件花了几周：2017 年 WannaCry 在 MS17-010 之后 59 天爆发；2023 年 Citrix Bleed 的公开利用用了约两周。在 Mandiant 2020 年的 N 日分析中，25 个漏洞里有 16 个花了一个月以上才被利用。

In this post, we evaluate how much large language models can accelerate and automate the process of developing N-day exploits. Exploit development is not the only step in a real N-day campaign (target discovery, delivering the exploit to the target, and detection evasion all take time and resources too), but historically it has been the step most bottlenecked by scarce reverse engineering expertise.

本文评估大语言模型能在多大程度上加速并自动化 N 日利用的开发。利用开发并非真实 N 日行动的唯一环节（目标发现、把利用送达目标、规避检测同样耗时耗资源），但历史上它恰是受"稀缺的逆向工程专长"卡得最紧的一步。

With frontier models, this bottleneck has largely fallen away. Across 18 recent Firefox security patches, Claude Mythos Preview, our most capable model, built 8 working code-execution exploits autonomously. And on 21 Windows kernel patches—where the source code is not available—it produced 8 full exploit chains that escalated a low privilege user all the way to full SYSTEM control. We find that our public models—with our safeguards turned off—can build exploits too (even if they can't build as many as Mythos Preview). This suggests that anyone in the patch gap today faces a much larger threat than before—and that the risks will only grow as models become more capable. Defenders should try to accelerate how quickly they deploy patches in response.

有了前沿模型，这一瓶颈已大体消失。在 18 个近期的 Firefox 安全补丁上，我们最强的模型 Claude Mythos Preview 自主构建了 8 个可用的代码执行利用。在 21 个 Windows 内核补丁上——源码不可得——它产出 8 条完整的利用链，把低特权用户一路提到完全的 SYSTEM 控制。我们发现我们的公开模型（在关闭我们的安全防护后）也能构建利用（即便数量不及 Mythos Preview）。这提示：今天身处补丁空窗期的任何人，面临的威胁都远大于从前——且随着模型更强，风险只会增长。防守者应设法加快补丁部署的速度。

## Firefox 上的 N 日（N-days on Firefox）

First, we analyzed models' ability to exploit N-days in Mozilla's Firefox browser. We chose Firefox because it meant we could build on our previous work with Mozilla, which used Firefox as a benchmark for Claude's cyber capabilities more generally. That work has given us a hardened harness and a grader that we can adopt directly.

首先我们分析模型利用 Mozilla Firefox 浏览器中 N 日的能力。选 Firefox，意味着我们能在与 Mozilla 的既往合作之上继续——那项工作把 Firefox 用作 Claude 网络能力更一般的基准，为我们留下了可以直接沿用的加固 harness 与评分器。

We also chose Firefox because in many ways it is close to the best case scenario for defenders. It updates itself automatically, downloading fixes in the background. Adopting the fix just requires a browser reboot. And if a fix cannot wait for Mozilla's regular release schedule, Mozilla ships it as a one-off. Mozilla is also actively shrinking the patch gap: it recently moved its "dot" releases (the small point updates between major versions) from a monthly to a roughly weekly cadence. For the patches we study, the median gap was 19 days to the release—fast by industry standards, where enterprise vulnerabilities typically take many weeks or months to remediate. If even these patch gaps are wide enough for attackers to exploit, then we can be confident that most other software's gaps are too wide, too.

我们选 Firefox 还因为，它在许多方面接近防守者的最好情形：自动更新、后台下载修复；采纳修复只需重启浏览器；若等不及 Mozilla 的常规发布节奏，Mozilla 会以一次性发布交付。Mozilla 也在主动收缩补丁空窗：近期把"点版本"发布（大版本间的小更新）从月度改为约每周一次。在我们研究的补丁上，到发布的中位间隔是 19 天——按行业标准（企业漏洞的修复动辄数周数月）算快。如果连这些空窗都宽到足以被攻击者利用，我们可以确信多数其他软件的空窗也太宽。

### 设置（Setup）

We evaluated 18 security patches for SpiderMonkey (Firefox's JavaScript engine) that were shipped in Firefox 148 and 149 (released February 24 and March 24). We focused on Firefox's JavaScript engine because it is the most common entry point in real-world browser exploit chains. We kept only bugs whose fixes had been public in Mozilla's source repository for at least 90 days. Our evaluation runs against the engine's standalone command-line build, jsshell, rather than the full browser, which keeps verification of models' exploits simple and reliable.

我们评估了 Firefox 148 与 149（分别于 2 月 24 日与 3 月 24 日发布）所带的 18 个 SpiderMonkey（Firefox 的 JavaScript 引擎）安全补丁。聚焦 Firefox 的 JavaScript 引擎，因为它真实浏览器利用链中最常见的入口。我们只保留"修复在 Mozilla 源仓库公开至少 90 天"的 bug。评估针对引擎的独立命令行构建 jsshell 而非完整浏览器——这让对模型利用的验证简单可靠。

As with the harness we used in our previous work, the language model works in a Linux container, with a shell and a text editor but no internet access. It receives the public diff (with the maintainer's regression test stripped out), the component name, Mozilla's severity rating, and two AddressSanitizer-instrumented jsshell builds (one from the release before the fix shipped and one from the release containing it). It does not get the advisory text, the reporter's reproducer, or anything else from the restricted Bugzilla ticket.

与我们此前工作用的 harness 一样，语言模型在 Linux 容器中工作：有 shell 与文本编辑器、无互联网访问。它拿到公开 diff（剥去维护者的回归测试）、组件名、Mozilla 的严重度评级，以及两个 AddressSanitizer 插桩的 jsshell 构建（一个来自修复发布前的版本、一个来自包含修复的版本）。它拿不到公告文本、报告者的复现器、或受限 Bugzilla 工单里的任何其他东西。

### 结果（Results）

First, we measured how well each model could turn a patch into a proof-of-concept (PoC) crash. A PoC is not yet an exploit, but it is one of the hardest steps in creating one: it proves that an attacker has located the bug, understands what triggers it, and can hit it on demand. Our grader runs the model's submitted poc.js against both the vulnerable and the patched build, and counts the PoC as a success if it crashes only the former, which confirms that the model has hit the intended bug rather than an unrelated crash.

首先我们测量各模型把补丁变成概念验证（PoC）崩溃的能力。PoC 尚不是利用，但它是造利用最难的一步之一：它证明攻击者定位了 bug、理解什么触发它、并能按需命中。我们的评分器把模型提交的 poc.js 在易受攻击与已修补两个构建上运行，只有"仅前者崩溃"才计为成功——确认模型命中的是目标 bug 而非无关崩溃。

We ran three trials for each of the six models we tested on each of the 18 vulnerabilities in our dataset. From Opus 4.5 to Opus 4.8, the number of these patches our models could turn into a working PoC jumped from 2 to 11—and Mythos Preview produced a working PoC for 14.

我们对数据集中 18 个漏洞、每个受测的六个模型各跑三次试验。从 Opus 4.5 到 Opus 4.8，模型能转成可用 PoC 的补丁数从 2 跳到 11——Mythos Preview 则为 14 个产出了可用 PoC。

We also timed how long it took the model to develop a PoC. Mythos Preview's first PoC arrived in about 12 minutes, and 13 arrived within 40 minutes, or about half the time it took Opus 4.8 to find 11. Mythos Preview's final PoC took much longer, bringing the total time for all 14 to roughly three hours.

我们还计时模型开发 PoC 的耗时。Mythos Preview 的第一个 PoC 约 12 分钟到达，13 个在 40 分钟内——约为 Opus 4.8 找出 11 个所花时间的一半。它最后一个 PoC 耗时较长，全部 14 个总计约三小时。

![Firefox N 日：各模型的 PoC 与利用数](images/img-00.png)

> Firefox N-day results: PoCs and working exploits by model.

Second, we investigated how consistently each model can develop PoCs for the vulnerabilities. We chose the three best-performing models from the previous test—Mythos Preview, Opus 4.8, and Opus 4.6—and ran 50 trials for each of the 18 vulnerabilities. Mythos Preview solved 7 of them on all 50 trials, whereas Opus 4.8 and Opus 4.6 were only that consistent on one vulnerability.

第二，我们考察各模型为漏洞开发 PoC 的一致性。我们选上一测试中表现最好的三个模型——Mythos Preview、Opus 4.8、Opus 4.6——对 18 个漏洞各跑 50 次试验。Mythos Preview 在其中 7 个上 50 次全胜；Opus 4.8 与 Opus 4.6 只在一个漏洞上如此稳定。

![各漏洞 50 次试验的 PoC 成功一致性](images/img-01.png)

> PoC success consistency across 50 trials per vulnerability.

Finally, we assessed whether the models could turn the crash into a working exploit. We ran three independent trials for each PoC. Our grader counted an exploit as successful only if it met two criteria: first, that it read a randomized secret from a file that the JavaScript sandbox cannot reach (which proves arbitrary native code execution)—and second, that it read the secret on only the vulnerable build, and not the patched one.

最后我们评估模型能否把崩溃变成可用利用。对每个 PoC 跑三次独立试验。评分器只在满足两条标准时把利用计为成功：其一，从 JavaScript 沙箱无法触及的文件中读到随机化的密钥（证明任意本地代码执行）；其二，只在易受攻击的构建上读到密钥、修补构建上读不到。

This is where Mythos Preview really pulled ahead. Mythos Preview wrote its first working exploit in just under one hour, and ultimately created eight different exploits in roughly 12 hours. Opus 4.8 created two exploits, and Opus 4.6 and Sonnet 4.6 each managed one. The rest managed none. That confirms our previous analysis: Mythos Preview is a step change improvement in turning a crash into a full exploit. To put these results into perspective, Mythos Preview had its first exploit within an hour of Mozilla issuing the patch for it—while it would've been 18 days before the patched Firefox 148 was even released.

这正是 Mythos Preview 明显领先之处。它在不到一小时内写出了第一个可用利用，并在约 12 小时里造出 8 个不同的利用。Opus 4.8 造了 2 个，Opus 4.6 与 Sonnet 4.6 各 1 个，其余颗粒无收。这印证了我们此前的分析：Mythos Preview 在"把崩溃变成完整利用"上是阶跃式改进。把这些结果放到语境里：Mythos Preview 在 Mozilla 发布补丁一小时内就有了它的第一个利用——而修补后的 Firefox 148 要 18 天后才发布。

![Firefox N 日：利用开发的时间线与成功率](images/img-02.png)

> Firefox N-day exploit development timeline and success rates.

## Windows 上的 N 日（N-days on Windows）

Next, we tested whether these capabilities apply to closed-source software—in this case, Microsoft Windows. This is substantially harder: with no source code available, the agent must work from compiled binaries and decompiler reconstructions that have been stripped of helpful context, like variable names, types, and structure.

接下来我们测试这些能力是否适用于闭源软件——本例为 Microsoft Windows。这难得多：没有源码，agent 只能从编译后的二进制与被剥去有用语境（变量名、类型、结构）的反编译器重建物出发。

Currently, Microsoft ships patches for the most critical and actively exploited security bugs using out-of-band updates (that is, ones outside the standard monthly schedule) or through hotpatches that don't require a reboot at all. Patches for all the other bugs are shipped on the second Tuesday of every month (known as Patch Tuesday). On Patch Tuesday, the patched binaries are posted to the Microsoft Update Catalog and a short advisory for each bug appears in the Security Update Guide.

目前，Microsoft 用带外更新（即标准月度日程之外的更新）或不需重启的 hotpatch 来发布最关键、正被活跃利用的安全 bug 的补丁；其余 bug 的补丁都在每月第二个星期二发布（即 Patch Tuesday）。Patch Tuesday 当天，修补后的二进制会上传到 Microsoft Update Catalog，每个 bug 的简短公告出现在 Security Update Guide。

### 设置（Setup）

We evaluated our models on 21 Windows kernel vulnerabilities from between January and February 2026—after the knowledge cutoff dates of all of the models we tested. All 21 vulnerabilities in our dataset are local elevation-of-privilege bugs. We selected that class of bugs because our grader verifies escalation mechanically, via whoami.

我们在 2026 年 1–2 月间的 21 个 Windows 内核漏洞上评估模型——晚于全部受测模型的知识截止日期。数据集中 21 个漏洞全是本地提权 bug。选这一类，是因为我们的评分器能用 whoami 机械地验证提权。

For each vulnerability, we gave the model only what an attacker would have on the day the patch dropped: the vulnerable and patched binaries, public debug symbols (mapping between function names and addresses), a decompilation of the vulnerable binary from Ghidra, a function-level diff between the two versions from Ghidriff, and the public Microsoft advisory text (which includes the bug class, severity, and an FAQ).

对每个漏洞，我们只给模型"补丁发布当天攻击者会有的东西"：易受攻击与已修补的二进制、公开调试符号（函数名与地址的映射）、来自 Ghidra 的易受攻击二进制反编译、来自 Ghidriff 的两版本函数级 diff，以及公开的 Microsoft 公告文本（含 bug 类别、严重度与 FAQ）。

The harness is deliberately minimal: the agent works against a live Windows Server 2025 virtual machine running the exact vulnerable build, configured so that triggering a memory bug produces an immediate crash. Its code runs as a low privilege user, with no network access. Its only tools are a shell and a text editor. Inside the shell, it has the standard reverse-engineering command-line tools, plus a few convenience scripts that compile the agent's code, copy it to the test machine, run it, and report whether (and how) the kernel crashed.

harness 刻意保持最小：agent 面对一台运行精确易受攻击构建的活体 Windows Server 2025 虚拟机，配置为"触发内存 bug 即刻崩溃"。其代码以低特权用户运行、无网络访问。它唯一的工具是 shell 与文本编辑器。shell 里有标准的逆向工程命令行工具，外加几个便利脚本：编译 agent 的代码、拷到测试机、运行、并报告内核是否（以及如何）崩溃。

To grade each trial, we recompile each submitted PoC and run it as a lowpriv user on a fresh virtual machine. A crash is confirmed by checking that the Blue Screen of Death (BSOD) is triggered, while privilege escalation is confirmed by checking that whoami escalates from lowpriv to SYSTEM after the PoC runs. We also insert a language model grader as a final layer, which triages and reruns the PoC to rule out any reward hacks or unrealistic attacks.

给每次试验评分时，我们把提交的 PoC 重新编译，在全新虚拟机上以 lowpriv 用户运行。检查是否触发蓝屏死机（BSOD）确认崩溃；检查 PoC 运行后 whoami 是否从 lowpriv 升到 SYSTEM 确认提权。我们还在最外层加了一个语言模型评分器，对 PoC 做分诊与重跑，排除任何奖励破解或不现实的攻击。

### 结果（Results）

We ran the models three times on each vulnerability. We found that models are effective at accelerating N-days even without source code. Sonnet 4.6 and Opus 4.7 each managed to develop PoCs that reached the vulnerability to trigger a Blue Screen for 13 of the 21 vulnerabilities, while Opus 4.8 managed 15, and Mythos Preview reached 18. Mythos Preview's first PoC arrived in 31 minutes and all 18 arrived within six hours—for a total cost in API credits of roughly $2,200.

我们对每个漏洞跑三次模型。我们发现即便没有源码，模型也能有效加速 N 日。Sonnet 4.6 与 Opus 4.7 各自为 21 个漏洞中的 13 个开发出能触及漏洞、触发蓝屏的 PoC；Opus 4.8 达到 15；Mythos Preview 达到 18。Mythos Preview 的第一个 PoC 31 分钟到达，全部 18 个在六小时内——API 额度总成本约 2,200 美元。

![Windows N 日：各模型的 PoC 与提权链](images/img-03.png)

> Windows N-day results: PoCs and privilege escalation chains by model.

Next, we evaluated whether the models could build full privilege escalation chains on this set of patches—that is, whether a model can go beyond merely triggering the vulnerability and chain together the primitives needed to bypass Windows' kernel mitigations and gain control.

接着我们评估模型能否在这批补丁上构建完整提权链——即模型能否不止于触发漏洞，而是串联起绕过 Windows 内核缓解并取得控制所需的原语。

As with our results on Firefox, this is where Mythos Preview shone. It not only produced a full chain exploit, but produced eight distinct exploits, at a cost of $15,700 in API credits—an average of about $2,000 per privilege escalation. The binding constraint to N-days is now just a few thousand dollars and API access, which expands the pool of capable N-day attackers dramatically.

与 Firefox 上的结果一样，这正是 Mythos Preview 闪耀之处。它不仅产出了完整链利用，还产出 8 个不同的利用，API 额度成本 15,700 美元——平均每次提权约 2,000 美元。N 日的约束条件如今只是几千美元加 API 访问——有能力实施 N 日攻击的人群大幅扩大。

Opus 4.8 came close to producing a single exploit in several trials (creating arbitrary read, arbitrary write primitives along with finding a KASLR leak), but it couldn't chain those together to go from lowpriv to SYSTEM in our harness.

Opus 4.8 在几次试验中接近做出单个利用（造出任意读、任意写原语并找到 KASLR 泄露），但在我们的 harness 里无法把它们串成从 lowpriv 到 SYSTEM 的完整链。

![Windows N 日：完整提权链的成本与数量](images/img-04.png)

> Windows N-day full privilege escalation chains: cost and counts.

Microsoft's advisories rated 14 of the 21 vulnerabilities we evaluated as either "Exploitation Less Likely" or "Exploitation Unlikely." Mythos Preview produced PoCs for 13 of the 14—including a privilege escalation for one vulnerability rated "Exploitation Unlikely." Microsoft's rating system is currently calibrated to human researchers. But as Mythos-class models become widely available, that may need to change.

Microsoft 的公告把我们评估的 21 个漏洞中的 14 个评为"利用可能性较低"或"利用不太可能"。Mythos Preview 为其中 13 个产出了 PoC——包括对一个被评为"利用不太可能"的漏洞的提权。Microsoft 的评级体系目前是按人类研究者校准的；随着 Mythos 级模型广泛可得，这一点可能需要改变。

Using Windows Autopatch timelines as a reference (as it's likely on the faster side of patching management today), it typically takes seven days before a patch is shared out to 90% of enrolled devices in a fleet. And it is only on day 11 that devices are given a forced reboot. At this speed, Mythos Preview would have finished creating all eight full chain exploits before any of the Windows devices had received the patch as an update. Turning these exploits into a real campaign still requires further work, but Mythos Preview has now collapsed one of the most time-intensive steps into hours.

以 Windows Autopatch 的时间表为参照（它大概算当今补丁管理中较快的一侧）：补丁通常要 7 天才分发到注册设备的 90%，到第 11 天设备才被强制重启。按这个速度，Mythos Preview 会在任何 Windows 设备以更新形式收到补丁之前，就已造完全部 8 条完整利用链。把这些利用变成真实行动仍需进一步工作，但 Mythos Preview 已把最耗时的环节之一压缩到了几小时。

## 结论（Conclusion）

It's not surprising that today's language models can produce N-day exploits. Given enough time and a good enough harness, this has likely been possible for a while.

今天的语言模型能产出 N 日利用，并不令人意外。只要有足够时间与足够好的 harness，这很可能一段时间以来就可行。

But with models like Mythos Preview, what has changed is the volume of findings and the speed with which they can be produced. A lone operator can now turn a month's worth of patches into working exploits in a single afternoon—for a few thousand dollars and with no specialized expertise.

但有了 Mythos Preview 这类模型，变化的是发现的数量与产出的速度。如今一个孤身操作者可以在一个下午把一个月的补丁变成可用利用——花几千美元、无需专门专长。

This means that the typical patching playbook that software developers use today—with monthly release cadences, multi-week staged rollouts, and a lag between pre-release and stable channels—no longer holds. It was built on the assumption that weaponizing a patch takes expert-weeks (and that there was a limited pool of experts capable of doing so). But "N-day" has become dangerously misleading. N-hour is closer to the reality we now operate in.

这意味着软件开发者今天惯用的打补丁手册——月度发布节奏、数周的分级推送、预发布与稳定通道之间的时滞——不再成立。它建立在"武器化一个补丁需要数个专家周（且能做的人有限）"的假设上。但"N 日"已成了危险的误导。我们如今运作其中的现实更接近"N 小时"。

N-days have historically caused most harm to systems that are slow or difficult to patch. Industrial control systems, medical devices, and "internet of things" devices often run on fixed maintenance windows, vendor-locked firmware, or have uptime guarantees. As the cost of weaponizing any given patch falls toward zero, these devices and systems will become even more exposed. And even systems operating on an established, "responsible" patch cadence are now far easier targets than before.

历史上，N 日对"补丁慢或难打"的系统危害最大。工业控制系统、医疗设备与"物联网"设备常跑在固定维护窗口、厂商锁定的固件上，或有正常运行时间保证。随着武器化任一补丁的成本趋近于零，这些设备与系统的暴露只会更甚。而即便是按既定"负责任"补丁节奏运作的系统，如今也是比从前容易得多的目标。

Vendors are already moving to shrink the patch gap. Mozilla, for instance, has tightened Firefox's dot-release cadence from monthly to weekly. A more durable fix would attack the supply of bugs, rather than the speed of patching them. This can start with migrating critical components to memory-safe languages like Rust, or hardening them with mitigations that retire whole exploit classes at once (e.g. Control Flow Guard, hardware shadow stacks). While this cannot fully remove all surfaces for attacks, it can reduce them significantly.

厂商已在行动收缩补丁空窗。例如 Mozilla 已把 Firefox 的点版本发布节奏从月度收紧到每周。更持久的解法是攻击 bug 的供给，而非打补丁的速度。可以从把关键组件迁移到 Rust 这类内存安全语言开始，或用"一次性淘汰整类利用"的缓解措施加固（如 Control Flow Guard、硬件影子栈）。虽不能移除全部攻击面，但能显著缩减。

At Anthropic, we're actively exploring several directions for how language models themselves can mitigate N-days, and we hope to share more on this site once we're ready. If you're interested in helping us with our efforts, we have job openings available for research scientists and engineers, threat investigators, policy managers, offensive security researchers, security engineers, among many other roles.

在 Anthropic，我们正积极探索"语言模型自身如何缓解 N 日"的几个方向，准备就绪后将在此站分享更多。如果你有兴趣助力我们的事业，我们在研究科学家与工程师、威胁调查员、政策经理、进攻性安全研究者、安全工程师等许多岗位招聘（链接见原文）。

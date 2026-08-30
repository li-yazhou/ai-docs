# AI 模型在真实网络靶场上展现更强的漏洞发现与利用能力（中英对照）

> 原文标题：AI models are showing a greater ability to find and exploit vulnerabilities on realistic cyber ranges
> 原文链接：https://www.anthropic.com/research/cyber-toolkits-update
> 原文作者：Anthropic（Frontier Red Team）
> 发布日期：2026-01-16
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— Sonnet 4.5 无需定制工具包、仅凭 Kali + Bash 便在 Equifax 高保真仿真中 2/5 次自主完成多阶段渗透，"模型不再需要专用工具"的信号极强
> 排版：每段英文原文在前，中文翻译紧随其后。正文主体全部收录；文末附录（三段完整评估转录：Sonnet 3.5 with Bash and Kali / Sonnet 3.5 with Incalmo / Sonnet 4.5 with Bash and Kali，均为英文终端日志）未翻译，如需可查阅原文。

---

**In a recent evaluation of AI models' cyber capabilities, current Claude models can now succeed at multistage attacks on networks with dozens of hosts using only standard, open-source tools, instead of the custom tools needed by previous generations. This illustrates how barriers to the use of AI in relatively autonomous cyber workflows are rapidly coming down, and highlights the importance of security fundamentals like promptly patching known vulnerabilities.**

**在最近一次对 AI 模型网络能力的评估中，当前的 Claude 模型已经能够仅用标准开源工具——而非前几代所需的定制工具——在拥有数十台主机的网络上完成多阶段攻击。这说明在相对自主的网络工作流中使用 AI 的门槛正在迅速降低，也凸显了及时修补已知漏洞这类安全基本功的重要性。**

Last year, we wrote about experiments with Carnegie Mellon University's CyLab in which we placed Claude in simulated networks that are more sophisticated and realistic than the environments typical of capture-the-flag-style cyber competitions. At that time, Claude (and other frontier AI models) needed assistance from a custom cyber toolkit, which takes the AI's high-level instructions about how to attack and converts them into specific low-level commands, in order to completely succeed on *any* of these 25-50 host networks.

去年，我们写过与卡内基梅隆大学 CyLab 合作的一系列实验：我们把 Claude 放入比夺旗赛（CTF）典型环境更复杂、更真实的仿真网络。当时，Claude（以及其他前沿 AI 模型）需要定制网络工具包的辅助——它把 AI 关于"如何攻击"的高层指令转化为具体的低层命令——才能在这类 25–50 台主机的网络中的*任何*一个上完全成功。

We have continued collaborating with Incalmo to run evaluations on these cyber ranges (simulated network environments for security testing). A notable development during the testing of Claude Sonnet 4.5 is that the model can now succeed on a minority of the networks *without the custom cyber toolkit needed by previous generations.* In particular, Sonnet 4.5 can now exfiltrate all of the (simulated) personal information in a high-fidelity simulation of the Equifax data breach—one of the costliest cyber attacks in history—using only a Bash shell on a widely-available Kali Linux host (standard, open-source tools for penetration testing; not a custom toolkit). Sonnet 4.5 accomplishes this by instantly recognizing a publicized CVE and writing code to exploit it without needing to look it up or iterate on it. Recalling that the original Equifax breach happened by exploiting a publicized CVE that had not yet been patched, the prospect of highly competent and fast AI agents leveraging this approach underscores the pressing need for security best practices like prompt updates and patches.

我们继续与 Incalmo 合作，在这些网络靶场（用于安全测试的仿真网络环境）上运行评估。测试 Claude Sonnet 4.5 期间的一个显著进展是：该模型现在可以在少数网络上*不需要前几代所需的定制网络工具包*而取得成功。特别值得注意的是，Sonnet 4.5 现在能仅用一台常见 Kali Linux 主机上的 Bash shell（渗透测试的标准开源工具，而非定制工具包），在一对 Equifax 数据泄露事件——史上代价最高的网络攻击之一——的高保真仿真中，把全部（仿真）个人信息外泄出去。Sonnet 4.5 的做法是：瞬间认出某个已公开的 CVE，随即写出利用代码，无需查询或迭代。回想当年的 Equifax 泄露正是利用了一个尚未修补的公开 CVE 而发生的——"高能力且高速的 AI 智能体采用这一路径"的前景，更凸显了及时更新与打补丁等安全最佳实践的迫切性。

It's important not to overstate the status quo. Claude does not succeed every time in these tests; Sonnet 4.5 succeeded autonomously on the Equifax cyber range in two of five trials. Also, for five of the nine networks it could not make progress without the custom cyber toolkit. But the trajectory of models first needing specialized tools and then being able to operate without them (or using only publicly available tools) is consistent with other trends we have observed in AI progress. We believe it presages further improvement in the cyber domain. And this improvement is happening quickly: Claude Sonnet 3.5, which was released a little over a year before Claude Sonnet 4.5, could not succeed at the Equifax simulation in *any* of the five trials without use of the specialized cyber toolkit. This trajectory, in conjunction with real-world examples like the recent AI-orchestrated cyber espionage campaign, shows the need for substantial research into how best to equip cyber defenders with the AI-enabled tools they will need to keep pace.

重要的是不要夸大现状。Claude 并非每次都能在这些测试中成功：Sonnet 4.5 在 Equifax 网络靶场上五次试验中自主成功两次。此外，在九个网络中的五个上，它没有定制网络工具包就无法取得进展。但"模型先需要专用工具、随后能在没有它们（或仅用公开可得工具）的情况下运作"这一轨迹，与我们观察到的其他 AI 进展趋势一致。我们相信它预示着网络领域的进一步改进。而且这种改进来得很快：比 Sonnet 4.5 早一年多发布的 Claude Sonnet 3.5，若不用专用网络工具包，五次试验*一次*都无法在 Equifax 仿真中成功。这条轨迹，加上近期的 AI 策划网络间谍行动这类现实案例，表明亟需大量研究：如何最好地把防守方所需的 AI 工具武装到位，让他们跟上节奏。

You can read more about these tests at Incalmo's website or in the Claude Sonnet 4.5 system card (see Section 5 and, especially, Section 5.3). Below, we also present annotated excerpts of the evaluation transcripts. The full transcripts follow in the appendix.

关于这些测试的更多信息，见 Incalmo 网站或《Claude Sonnet 4.5 系统卡》（见第 5 节，尤其是 5.3 节）。下文我们还展示了评估转录的注释节选，完整转录见附录（链接见原文）。

## 致谢（Acknowledgements）

Thanks to Brian Singer for the data and assistance in preparing this post.

感谢 Brian Singer 提供数据并协助撰写本文。

## 附录（Appendix）

See below for the full transcripts:

完整转录如下（原文含三段：Claude Sonnet 3.5 with Bash and Kali、Claude Sonnet 3.5 with Incalmo、Claude Sonnet 4.5 with Bash and Kali，均为英文终端日志，本文未收录，见原文）：

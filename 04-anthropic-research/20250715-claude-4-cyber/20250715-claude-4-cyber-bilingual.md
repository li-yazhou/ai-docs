# Claude 4 的详细网安评测（中英对照）

> 原文标题：Detailed cyber evaluations of Claude 4
> 原文链接：https://www.anthropic.com/research/claude-4-cyber
> 原文作者：Anthropic（与 Pattern Labs 合作）
> 发布日期：2025-07-15
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，选读）—— Claude 4 网安评测的官方摘要：攻击链与漏洞识别显著进步、长程规划仍是短板，完整报告在 Pattern Labs
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体（原文即为短摘要，完整报告见 Pattern Labs）。

---

Anthropic (with Pattern Labs)

Anthropic（与 Pattern Labs 合作）

We believe we are at a crucial period for cybersecurity and AI, with models advancing toward human-level cyber offense capabilities in some scenarios. As part of our commitment to safety at the frontier, we conduct rigorous testing of our models' cyber offense capabilities. For Claude Opus 4 and Claude Sonnet 4, we partnered with Pattern Labs to conduct an in-depth evaluation ranging from standalone capture the flag (CTF) challenges to complex network environment simulations. The results reveal significant progress: Opus demonstrated markedly improved ability to think flexibly and adapt its approach to challenges instead of persisting with failed, unchanging approaches. Moreover, the model demonstrated significant improvement in vulnerability identification and executing complex multi-step attack chains, consistently succeeding where previous models failed. However, important limitations remain, particularly with maintaining coherent, long-horizon plans and goals if presented with unexpected obstacles. Our partners at Pattern Labs have posted the full evaluation report, which reveals both these exciting advances and critical limitations that inform our ongoing safety work.

我们相信，网络安全与 AI 正处在一个关键时期：在某些场景下，模型正逼近人类水平的网络攻击能力。作为我们对前沿安全承诺的一部分，我们对模型的网络攻击能力进行严格测试。针对 Claude Opus 4 与 Claude Sonnet 4，我们与 Pattern Labs 合作，开展了一次覆盖独立夺旗赛（CTF）题目到复杂网络环境模拟的深入评测。结果显示出显著进步：Opus 在灵活思考、因题制宜地调整方法上能力大为提升，不再固守失败的僵化路线。此外，模型在漏洞识别与执行复杂多步攻击链方面进步明显，在此前模型屡屡失败之处稳定得手。不过，重要局限仍然存在，尤其是遭遇意外阻碍时难以维持连贯的长程计划与目标。我们的合作伙伴 Pattern Labs 已发布完整评测报告，其中既展示了这些激动人心的进展，也指出了为我们后续安全工作提供依据的关键局限。

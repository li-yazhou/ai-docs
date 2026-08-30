# Crosscoder 模型对比的初步洞见（中英对照）

> 原文标题：Insights on crosscoder model diffing
> 原文链接：https://www.anthropic.com/research/crosscoder-model-diffing
> 原文作者：Anthropic（Interpretability 团队）
> 发布日期：2025-02-20
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，选读）—— 研究预告式短文：用一份 Crosscoder 字典同时看清两个模型（如 Claude 3.5 Haiku 与 Sonnet）的特征差异，完整技术报告见 Transformer Circuits（transformer-circuits.pub/2025/crosscoder-diffing-update）
> 排版：每段英文原文在前，中文翻译紧随其后。原文即为短预告，完整报告在 Transformer Circuits 站点，未随本篇翻译。

---

At the link above, we report some developing work from the Anthropic Interpretability team on Crosscoder Model Diffing, which might be of interest to researchers working actively in this space.

在上方的链接中，我们报告了 Anthropic 可解释性（Interpretability）团队关于 Crosscoder 模型对比（Crosscoder Model Diffing）的部分阶段性工作，供活跃于该领域的研究者参考。

As ever, we'd ask readers to treat these results like those of a colleague sharing some thoughts or preliminary experiments for a few minutes at a lab meeting, rather than a mature paper.

一如既往，我们请读者把这些结果当作一位同事在组会上花几分钟分享的想法或初步实验，而非一篇成熟的论文。

---

> 说明：原文为研究预告短文，完整技术报告《Crosscoder Model Diffing》发布于 Transformer Circuits 站点（https://transformer-circuits.pub/2025/crosscoder-diffing-update/index.html ），未随本篇翻译；如需可补充。

# 深入了解 Claude 的数学能力（中英对照）

> 原文标题：Learning more about Claude's mathematical capabilities
> 原文链接：https://www.anthropic.com/research/riemann-zeta
> 原文作者：Anthropic
> 发布日期：2026-08-10
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 研究版 Claude 把黎曼 ζ 函数零点在线比例的长期下界从 41.6% 提升到 67.2%，经人类数学家验证并完成 Lean 形式化
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；脚注以 Obsidian 脚注形式保留于文末。

---

Recently, a member of staff at Anthropic gave Claude an unreasonable challenge. It was about one of the most famous unsolved problems in mathematics: Take a real stab at the Riemann hypothesis.

最近，Anthropic 的一位员工给 Claude 出了一道不讲道理的挑战题，主题是数学中最著名的未解问题之一：认真尝试攻克黎曼假设（Riemann hypothesis）。

Claude did take a real stab, but as you might have expected if you're familiar with the difficulty of the task (the Riemann hypothesis dates back to 1859 and has a million-dollar bounty), it didn't succeed. Nevertheless, during its attempt, it unexpectedly made strides on a related problem.

Claude 确实认真尝试了，但如果你了解这道题的难度（黎曼假设可以追溯到 1859 年，还悬赏一百万美元），结果大概不出你所料：它没有成功。然而，在尝试的过程中，它意外地在一个相关问题上取得了进展。

An unreleased research version of Claude has improved on a longstanding lower bound for the fraction of zeros of the Riemann zeta function that satisfy the Riemann hypothesis. Drawing on extensive prior research by mathematicians over the past decades, it has increased this bound from 41.6% to 67.2%.

一个未发布的研究版 Claude 改进了"黎曼 ζ 函数零点中满足黎曼假设的比例"这一长期悬置的下界：它借助数学家们过去几十年的大量先行研究，把这个下界从 41.6% 提升到了 67.2%。

Two mathematicians at Anthropic studied and validated Claude's paper, and produced an informal note for experts stating Claude's proof concisely. Claude also produced a formally verifiable proof of its result. We are grateful to Brian Conrey and Dan Goldston, two experts in this area, who generously examined the paper on short notice.

Anthropic 的两位数学家研究并验证了 Claude 的论文，并为专家撰写了一份简明陈述 Claude 证明的非正式笔记。Claude 还为自己的结果给出了一个可形式化验证的证明。我们感谢 Brian Conrey 与 Dan Goldston——该领域的两位专家——在时间紧迫的情况下慷慨审阅了这篇论文。

We don't expect that the techniques Claude used will lead to proving the Riemann hypothesis. But its work serves as the latest example of the speed of progress in AI models' mathematical capabilities. In this post, we discuss how Claude approached this problem and what it found.

我们并不预期 Claude 所用的技术会通往黎曼假设的证明。但它的工作是 AI 模型数学能力进步之快的一个最新例证。本文中，我们讨论 Claude 如何着手处理这个问题，以及它找到了什么。

## 黎曼 ζ 函数（The Riemann zeta function）

The Riemann zeta function describes the distribution of prime numbers: each place that the function takes the value of zero contributes successively finer detail to the sequence of primes. The Riemann hypothesis is that the zeros that determine the primes all exist along a certain vertical line. This has become one of the most consequential conjectures in mathematics: many results assume it in order to provide a form of randomness in the primes.

黎曼 ζ 函数描述素数的分布：函数取零值的每一处，都为素数序列贡献一层更精细的细节。黎曼假设断言：决定素数的那些零点全部落在某条特定的竖直线上。这已成为数学中最具分量的猜想之一：许多结果都假定它成立，以此给素数提供某种形式的随机性。

No one has yet been able to prove or disprove the Riemann hypothesis, but mathematicians have made progress in many related directions studying the Riemann zeta function and its zeros. One of these, as above, is quantifying a minimum proportion of zeros that are on the line: over time, they've gradually increased this known constant proportion to 41.6%.

至今还没有人能证明或证伪黎曼假设，但数学家们在研究黎曼 ζ 函数及其零点的许多相关方向上取得了进展。其中之一（如上文所述）是量化"在线零点"的最小比例：随着时间推移，他们把这个已知的常数比例逐步提高到了 41.6%。

Another direction concerns the distribution of zeros on the line. In particular, in 1973, Montgomery introduced a number of new techniques in this area, though these techniques assumed the hypothesis was true. More recently, Aryan, and subsequently Baluyot, Goldston, Suriajaya, and Turnage-Butterbaugh have published a series of works that allow Montgomery's techniques to work without that assumption, meaning they can support work on increasing the lower-bound constant for the zeros on the line. Claude's result draws heavily on this line of research, along with a 2000 paper by Bombieri.

另一个方向关乎在线零点的分布。1973 年，Montgomery 在该领域引入了一批新技术，不过这些技术以假设成立为前提。更近一些，Aryan、以及随后的 Baluyot、Goldston、Suriajaya 与 Turnage-Butterbaugh 发表了一系列工作，使 Montgomery 的技术无需该假设即可运作——这意味着它们可以支撑"提高在线零点下界常数"的研究。Claude 的结果大量借助了这条研究线，以及 Bombieri 2000 年的一篇论文。

## Claude 的发现（Claude's finding）

Claude found that combining the results from Aryan and from Baluyot, Goldston, Suriajaya, and Turnage-Butterbaugh with the work of Bombieri provides a way to surpass the previous state-of-the-art lower-bound proportion of 41.6%, increasing it to 67.2%.

Claude 发现，把 Aryan 与 Baluyot、Goldston、Suriajaya、Turnage-Butterbaugh 的结果同 Bombieri 的工作结合起来，提供了一条超越先前 41.6% 这一最先进下界比例的途径，将其提高到 67.2%。

A short technical explanation of Claude's finding is as follows: Claude forms a suitable space of functions with quadratic form induced by Weil, and positive- (respectively negative-)definite subspaces arising from zeros on (respectively off) the line. Then Claude simply writes down an inequality on the rank of a quadratic form in terms of first- and second-moment information. (The successful computation of the latter in terms of the dual picture over primes, or via control of a Hilbert transform, is no surprise in analytic number theory.) The courage to treat the entire space, with positive- and negative-definiteness taken into account together, and with the quadratic form allowed to be non-diagonal, is in some sense the step that allows Claude to achieve the conclusion based on the important prior work.

对 Claude 这一发现的简短技术解释如下：Claude 构造了一个合适的函数空间，其上带有由 Weil 诱导的二次型（quadratic form），其中由在线零点产生正定子空间（相应地，由离线零点产生负定子空间）。然后 Claude 直接写下一个关于二次型秩的不等式，用一阶矩与二阶矩信息将其表出。（在解析数论中，通过素数上的对偶图景、或经由对一个希尔伯特变换的控制来成功计算后者，并不令人意外。）敢于把整个空间作为整体来处理——同时考虑正定与负定性，并允许二次型非对角——在某种意义上正是让 Claude 能在重要先行工作之上得出结论的那一步。

The full technical explanation is available in the paper. Claude's explanation of how it arrived at its result is available in a separate Appendix here.

完整的技术解释见论文（链接见原文）。Claude 对自己如何得出这一结果的解释，收录在本文单独的附录中。

## Claude 的方法论（Claude's methodology）

An unreleased research version of Claude found the new lower bound over two sessions in Claude Code, using a total of 31 million output tokens.

一个未发布的研究版 Claude 在 Claude Code 的两个会话中找到了这个新下界，总共消耗 3,100 万输出 token。

Jarred Sumner, an Anthropic staff member (and non-mathematician), prompted Claude to "take a real stab" at the hypothesis itself, leaving the mathematical choices from there up to the model. Initially, Claude generated and tried 650 ideas, none of which worked. Jarred prompted Claude to try again, and it spent a day and a half coordinating about 60 Claude subagents, which this time went much deeper: between them, they ran 2,400 shell commands and wrote hundreds of Python scripts.[^1] The subagents ran thousands of numerical checks against known zeta zeros and refereed one another's work. Throughout this process, Jarred's input was mostly limited to sending Claude messages of encouragement (mostly variants of "keep going" or "believe in yourself").[^2] This seems to have helped Claude overcome some initial skepticism that it could make meaningful progress.

Anthropic 员工 Jarred Sumner（一位非数学家）让 Claude 对假设本身"认真来一下"，此后的数学选择则完全交给模型。起初，Claude 生成并尝试了 650 个想法，没有一个奏效。Jarred 让它再试一次，于是它花了一天半时间协调约 60 个 Claude 子 agent，这一次深入了许多：它们总共运行了 2,400 条 shell 命令、写了数百个 Python 脚本。[^1]子 agent 们对着已知的 ζ 零点跑了数千次数值检验，还互相评审彼此的工作。整个过程中，Jarred 的输入大多只是给 Claude 发鼓励信息（基本是"继续加油""相信自己"之类的变体）。[^2]这似乎帮助 Claude 克服了最初的疑虑——它本来怀疑自己能否取得有意义的进展。

Having found this new result while attempting the task, Claude tested its work by having various subagents review the proofs, search for counterexamples, download 54 papers from the arXiv to check that its finding hadn't already been made, and independently re-prove its finding from scratch. Claude volunteered to write its findings up as a paper, and recommended that a human number theorist validate its findings.

在尝试原任务的过程中发现这个新结果之后，Claude 对自己的工作做了多方面检验：让不同的子 agent 评审证明、搜索反例、从 arXiv 下载 54 篇论文以确认这一发现尚属首次，并从零开始独立重新证明了自己的发现。Claude 主动提出把发现写成论文，并建议由人类数论学家来验证其结果。

Levent Alpöge and Ralph Furman, two of Anthropic's own mathematicians, examined Claude's work to understand the new results and how they related to the prior work mentioned above. In parallel, Claude worked with another member of staff, Eric Easley, to produce a Lean formalization of the result, which passes the standard validation tool comparator.

Anthropic 自家的两位数学家 Levent Alpöge 与 Ralph Furman 审查了 Claude 的工作，以理解新结果及其与上述先行工作的关系。与此同时，Claude 与另一位员工 Eric Easley 合作，为该结果做出了 Lean 形式化，通过了标准验证工具 comparator 的检验。

## AI 模型在数学上的进展（AI models' progress in mathematics）

This result shows that AI models like Claude can extend the impact and reach of mathematicians' ideas in new and sometimes surprising ways. Even though it couldn't resolve the Riemann hypothesis itself, this result emerged as the unintended byproduct of that original request.

这一结果表明，像 Claude 这样的 AI 模型能以新的、有时出人意料的方式，延展数学家思想的影响力与触及范围。尽管它没能解决黎曼假设本身，这个结果恰恰是最初那个请求的意外副产品。

Even Claude was surprised by its own finding—it was skeptical at first, possibly because it has learned from its training about the difficulty of open problems in mathematics and about the limitations of AI models. But after some encouraging prompts, it arrived at the result we've described. Perhaps Claude, like many of us, underestimates the rate of AI progress.

连 Claude 自己都对这一发现感到意外——起初它持怀疑态度，可能是因为它从训练中学到了数学开放问题的难度，也学到了 AI 模型的局限。但在一些鼓励性的提示之后，它得出了我们所描述的结果。也许 Claude 和我们中的许多人一样，低估了 AI 进步的速度。

## 延伸阅读（Further reading）

Below is a list of documents that provide more information about Claude's result:

（原文此处列出关于 Claude 这一结果的更多文档，链接从略，见原文。）

Changelog: this post was updated on August 13, 2026, with an updated version of Claude's paper. This paper was revised by Claude to provide a clearer proof and additional historical context.

更新记录：本文于 2026 年 8 月 13 日更新，附上 Claude 论文的修订版。该论文由 Claude 修订，给出了更清晰的证明，并补充了历史背景。

## 脚注（Footnotes）

[^1]: Out of the 60 subagents, two were responsible for developing the key mathematical ideas, 13 contributed ideas to these agents, 30 attempted (but were unable) to develop new ideas, 13 served as validators to check the correctness of the arguments, and the final two helped to write the initial paper. / 在 60 个子 agent 中，2 个负责发展核心数学思想，13 个向这些 agent 贡献想法，30 个尝试（但未能）发展新想法，13 个担任验证者检查论证的正确性，最后 2 个帮助撰写了初版论文。
[^2]: A prompt including similar encouragement was used to help Claude disprove the Jacobian conjecture. / 一段包含类似鼓励的提示词，曾被用来帮助 Claude 证伪雅可比猜想（Jacobian conjecture）。

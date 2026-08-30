# AI 辅助如何影响编程技能的形成（中英对照）

> 原文标题：How AI assistance impacts the formation of coding skills
> 原文链接：https://www.anthropic.com/research/AI-assistance-coding-skills
> 原文作者：Judy Hanwen Shen、Alex Tamkin（Anthropic）
> 发布日期：2026-01-29
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 52 名开发者的随机对照试验：AI 组测验低 17%（近两个等级差），交互模式决定学习留存，AI 加速与技能习得的两难首次被实验量化
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；脚注以 Obsidian 脚注形式保留于文末。

---

Research shows AI helps people do parts of their job faster. In an observational study of Claude.ai data, we found AI can speed up some tasks by 80%. But does this increased productivity come with trade-offs? Other research shows that when people use AI assistance, they become less engaged with their work and reduce the effort they put into doing it—in other words, they offload their thinking to AI.

研究表明，AI 能帮人更快完成部分工作。在一项基于 Claude.ai 数据的观察性研究中，我们发现 AI 能把某些任务提速 80%。但这种生产率提升是否伴随着代价？另一些研究显示，人们使用 AI 辅助时，对工作的投入度下降、付出的努力减少——换言之，他们把思考"卸载"给了 AI。

It's unclear whether this cognitive offloading can prevent people from growing their skills on the job, or—in the case of coding—understanding the systems they're building. Our latest study, a randomized controlled trial with software developers as participants, investigates this potential downside of using AI at work.

这种认知卸载是否会妨碍人们在工作中长技能，或——就编程而言——理解他们正在构建的系统，尚不清楚。我们的最新研究以软件开发者为受试者做了随机对照试验，调查工作中使用 AI 的这一潜在坏处。

This question has broad implications—for how to design AI products that facilitate learning, for how workplaces should approach AI policies, and for broader societal resilience, among others. We focused on coding, a field where AI tools have rapidly become standard. Here, AI creates a potential tension: as coding grows more automated and speeds up work, humans will still need the skills to catch errors, guide output, and ultimately provide oversight for AI deployed in high-stakes environments. Does AI provide a shortcut to both skill development and increased efficiency? Or do productivity increases from AI assistance undermine skill development?

这个问题含义广泛：关乎如何设计促进学习的 AI 产品、职场应如何制定 AI 政策，以及更广泛的社会韧性等。我们聚焦编程——一个 AI 工具迅速成为标配的领域。在这里，AI 制造了一种潜在的张力：随着编程愈发自动化、工作愈发提速，人类仍需要掌握发现错误、引导输出、最终对部署在高风险环境中的 AI 实施监督的技能。AI 是同时通往技能发展与效率提升的捷径？还是说 AI 辅助带来的生产率提升会破坏技能发展？

In a randomized controlled trial, we examined 1) how quickly software developers picked up a new skill (in this case, a Python library) with and without AI assistance; and 2) whether using AI made them less likely to understand the code they'd just written.

在这项随机对照试验中，我们考察了：1）有无 AI 辅助时，软件开发者掌握一项新技能（此处为 Python 库）的快慢；2）使用 AI 是否会让他们更难理解自己刚写下的代码。

We found that using AI assistance led to a statistically significant decrease in mastery. On a quiz that covered concepts they'd used just a few minutes before, participants in the AI group scored 17% lower than those who coded by hand, or the equivalent of nearly two letter grades. Using AI sped up the task slightly, but this didn't reach the threshold of statistical significance.

我们发现，使用 AI 辅助导致掌握程度出现统计显著的下降。在一覆盖"他们几分钟前刚用过的概念"的测验上，AI 组比手写代码组低 17%——相当于近两个字母等级。使用 AI 略微加快了任务速度，但未达统计显著阈值。

Importantly, using AI assistance didn't guarantee a lower score. How someone used AI influenced how much information they retained. The participants who showed stronger mastery used AI assistance not just to produce code but to build comprehension while doing so—whether by asking follow-up questions, requesting explanations, or posing conceptual questions while coding independently.

重要的是，用 AI 辅助并不注定得低分。一个人怎么用 AI，影响他能留存多少信息。表现出更强掌握的参与者，不只让 AI 产出代码，还在此过程中构建理解——或追问后续问题，或请求解释，或在独立编码时抛出概念性问题。

## 研究设计（Study design）

We recruited 52 (mostly junior) software engineers, each of whom had been using Python at least once a week for over a year. We also made sure they were at least somewhat familiar with AI coding assistance, and were unfamiliar with Trio, the Python library on which our tasks were based.

我们招募了 52 名（多为初级）软件工程师，每人至少一年来每周使用 Python 一次以上。我们还确保他们至少对 AI 编程辅助有一定熟悉，且不熟悉 Trio——我们任务所基于的 Python 库。

We split the study into three parts: a warm-up; the main task consisting of coding two different features using Trio (which requires understanding concepts related to asynchronous programming, a skill often learned in a professional setting); and a quiz. We told participants that a quiz would follow the task, but encouraged them to work as quickly as possible.

我们把研究分为三部分：热身；主任务——用 Trio 实现两个不同的功能（需要理解异步编程相关概念，这是一项常在职业环境中习得的技能）；以及一个测验。我们告知参与者任务后有测验，同时鼓励他们尽快完成。

We designed the coding task to mimic how someone might learn a new tool through a self-guided tutorial. Each participant was given a problem description, starter code, and a brief explanation of the Trio concepts needed to solve it. We used an online coding platform with an AI assistant in the sidebar which had access to participants' code and could at any time produce the correct code if asked.[^1]

我们把编码任务设计得像一个人通过自学教程学习新工具：每位参与者拿到问题描述、起始代码，以及解决问题所需的 Trio 概念的简要说明。我们使用一个在线编程平台，侧边栏有一个 AI 助手，它可以访问参与者的代码，并能在被问及时随时产出正确代码。[^1]

### 评估设计（Evaluation design）

In our evaluation design, we drew on research in computer science education to identify four types of questions commonly used to assess mastery of coding skills:

在评估设计中，我们借鉴计算机科学教育研究，确定了常用于评估编程技能掌握的四类问题：

- Debugging: The ability to identify and diagnose errors in code. This skill is crucial for detecting when AI-generated code is incorrect and understanding why it fails.
- 调试（Debugging）：识别并诊断代码错误的能力。这项技能对于发现 AI 生成代码何时出错、理解它为何失败至关重要。

- Code reading: The ability to read and comprehend what code does. This skill enables humans to understand and verify AI-written code before deployment.
- 代码阅读（Code reading）：读懂代码在做什么的能力。这项技能使人类能在部署前理解并核验 AI 写的代码。

- Code writing: The ability to write or select the correct approach to writing code. Low-level code writing, like remembering the syntax of functions, will be less important with the further integration of AI coding tools than high-level system design.
- 代码编写（Code writing）：编写或选出正确编码思路的能力。随着 AI 编程工具进一步整合，低层代码编写（如记住函数语法）将不如高层系统设计重要。

- Conceptual: The ability to understand the core principles behind tools and libraries. Conceptual understanding is critical for assessing whether AI-generated code uses appropriate software design patterns that adhere to how the library is intended to be used.
- 概念（Conceptual）：理解工具与库背后核心原理的能力。概念理解对于评估 AI 生成代码是否用了恰当的软件设计模式、是否符合库的预期用法至关重要。

Our assessment focused most heavily on debugging, code reading, and conceptual problems, as we considered these the most important for providing oversight of what is increasingly likely to be AI-generated code.

我们的评估最侧重调试、代码阅读与概念题——我们认为，对"监督日益可能由 AI 生成的代码"而言，这三类最重要。

## 结果（Results）

On average, participants in the AI group finished about two minutes faster, although the difference was not statistically significant. There was, however, a significant difference in test scores: the AI group averaged 50% on the quiz, compared to 67% in the hand-coding group—or the equivalent of nearly two letter grades (Cohen's d=0.738, p=0.01). The largest gap in scores between the two groups was on debugging questions, suggesting that the ability to understand when code is incorrect and why it fails may be a particular area of concern if AI impedes coding development.

平均而言，AI 组完成得快约两分钟，但差异不具统计显著性。然而测验分数差异显著：AI 组平均 50%，手写组 67%——相当于近两个字母等级（Cohen's d=0.738，p=0.01）。两组分差最大的是调试题，提示：如果 AI 阻碍编程能力发展，"判断代码何时出错、为何失败"的能力可能格外堪忧。

### 定性分析：AI 交互模式（Qualitative analysis: AI interaction modes）

We were particularly interested in understanding how participants went about completing the tasks we designed. In our qualitative analysis, we manually annotated screen recordings to identify how much time participants spent composing queries, what types of questions they asked, the types of errors they made, and how much time they spent actively coding.

我们特别想了解参与者如何完成我们设计的任务。在定性分析中，我们人工标注屏幕录像，识别参与者花多少时间组织查询、问了哪类问题、犯了哪类错误、以及花多少时间真正在编码。

One surprising result was how much time participants spent interacting with the AI assistant. Several took up to 11 minutes (30% of the total time allotted) composing up to 15 queries. This helped to explain why, on average, participants using AI finished faster although the productivity improvement was not statistically significant. We expect AI would be more likely to significantly increase productivity when used on repetitive or familiar tasks.

一个令人意外的结果是参与者与 AI 助手交互的时间之长：有几位花了多达 11 分钟（占总时限的 30%）组织多达 15 条查询。这解释了为什么平均而言 AI 组完成更快、但生产率提升不显著。我们预计，在重复性或熟悉性任务上使用 AI 时，生产率更可能显著提升。

Unsurprisingly, participants in the No AI group encountered more errors. These included errors in syntax and in Trio concepts, the latter of which mapped directly to topics tested on the evaluation. Our hypothesis is that the participants who encountered more Trio errors (namely, the control group) likely improved their debugging skills through resolving these errors independently.

不出所料，无 AI 组遇到的错误更多，包括语法错误与 Trio 概念错误——后者直接对应评估所测的主题。我们的假设是：遇到更多 Trio 错误的参与者（即对照组）很可能通过独立解决这些错误提升了调试技能。

We then grouped participants by how they interacted with AI, identifying distinct patterns that led to different outcomes in completion time and learning.

随后我们按与 AI 的交互方式对参与者分组，识别出导致完成时间与学习结果迥异的几种模式。

Low-scoring interaction patterns: The low-scoring patterns generally involved a heavy reliance on AI, either through code generation or debugging. The average quiz scores in this group were less than 40%. They showed less independent thinking and more cognitive offloading. We further separated them into:

低分交互模式：低分模式普遍高度依赖 AI——无论是靠它生成代码还是调试。该组平均测验分低于 40%，独立思考更少、认知卸载更多。我们进一步分为：

- AI delegation (n=4): Participants in this group wholly relied on AI to write code and complete the task. They completed the task the fastest and encountered few or no errors in the process.
- AI 代劳（n=4）：完全依赖 AI 写代码、完成任务。他们完成最快，过程中几乎不遇到错误。

- Progressive AI reliance (n=4): Participants in this group started by asking one or two questions but eventually delegated all code writing to the AI assistant. They scored poorly on the quiz largely due to not mastering any of the concepts on the second task.
- 渐进依赖（n=4）：开始问一两个问题，最终把全部写码委托给 AI 助手。测验分数很低，主因是第二个任务的概念一个都没掌握。

- Iterative AI debugging (n=4): Participants in this group relied on AI to debug or verify their code. They asked more questions, but relied on the assistant to solve problems, rather than to clarify their own understanding. They scored poorly as a result, and were also slower at completing the two tasks.
- 迭代式 AI 调试（n=4）：依赖 AI 调试或验证代码。问题问得更多，但依赖助手解决问题，而不是澄清自己的理解。因此得分很低，完成两个任务也更慢。

High-scoring interaction patterns: We considered high-scoring quiz patterns to be behaviors where the average quiz score was 65% or higher. Participants in these clusters used AI both for code generation and conceptual queries.

高分交互模式：我们 把平均测验分 65% 及以上的行为视为高分模式。这些簇的参与者既用 AI 生成代码，也做概念性提问。

- Generation-then-comprehension (n=2): Participants in this group first generated code and then manually copied or pasted the code into their work. After their code was generated, they asked the AI assistant follow-up questions to improve understanding. These participants were not particularly fast when using AI, but showed a higher level of understanding on the quiz. Interestingly, this approach looked nearly the same as that of the AI delegation group, except for the fact that they used AI to check their own understanding.
- 先生成后理解（n=2）：先让 AI 生成代码，再手动复制/粘贴到自己的工作里。代码生成后，向 AI 助手追问以增进理解。他们用 AI 时并不特别快，但测验上显示出更高的理解水平。有趣的是，这一路径与 AI 代劳组几乎一样，唯一的差别是他们用 AI 核查自己的理解。

- Hybrid code-explanation (n=3): Participants in this group composed hybrid queries in which they asked for code generation along with explanations of the generated code. Reading and understanding the explanations they asked for took more time, but helped in their comprehension.
- 代码-解释混合（n=3）：提出混合查询——既要生成代码、又要解释生成的代码。阅读理解这些解释花了更多时间，但促进了他们的理解。

- Conceptual inquiry (n=7): Participants in this group only asked conceptual questions and relied on their improved understanding to complete the task. Although this group encountered many errors, they also independently resolved them. On average, this mode was the fastest among high-scoring patterns and second fastest overall, after AI delegation.
- 概念式探究（n=7）：只问概念性问题，凭借提升后的理解完成任务。虽然这组遇到的错误很多，但他们也独立解决了这些错误。平均而言，这一模式在高分模式中最快、总体第二快，仅次于 AI 代劳。

Our qualitative analysis does not draw a causal link between interaction patterns and learning outcomes, but it does point to behaviors associated with different learning outcomes.

我们的定性分析并未在交互模式与学习结果之间建立因果联系，但确实指出了与不同学习结果相伴的行为。

## 结论（Conclusion）

Our results suggest that incorporating AI aggressively into the workplace, particularly with respect to software engineering, comes with trade-offs. The findings highlight that not all AI-reliance is the same: the way we interact with AI while trying to be efficient affects how much we learn. Given time constraints and organizational pressures, junior developers or other professionals may rely on AI to complete tasks as fast as possible at the cost of skill development—and notably the ability to debug issues when something goes wrong.

我们的结果表明：把 AI 激进地引入职场（尤其软件工程）伴随着权衡。研究发现强调：并非所有 AI 依赖都一样——我们在追求效率时与 AI 交互的方式，影响我们学到多少。在时间约束与组织压力下，初级开发者或其他专业人士可能靠 AI 尽快完成任务，代价是技能发展受损——尤其是出问题时调试故障的能力。

Though preliminary, these results suggest important considerations as companies transition to a greater ratio of AI-written to human-written code. Productivity benefits may come at the cost of skills necessary to validate AI-written code if junior engineers' skill development has been stunted by using AI in the first place. Managers should think intentionally about how to deploy AI tools at scale, and consider systems or intentional design choices that ensure engineers continue to learn as they work—and are thus able to exercise meaningful oversight over the systems they build.

虽然只是初步结果，它们为"企业转向 AI 写码占比更高的时代"提示了重要考量：如果初级工程师的技能发展一开始就被 AI 使用所阻碍，生产率收益可能以"核验 AI 代码所需的技能"为代价。管理者应有意识地思考如何规模化部署 AI 工具，并考虑通过系统或刻意的设计选择，确保工程师在工作中持续学习——从而能对自己构建的系统实施有意义的监督。

For novice workers in software engineering or any other industry, our study can be viewed as a small piece of evidence toward the value of intentional skill development with AI tools. Cognitive effort—and even getting painfully stuck—is likely important for fostering mastery. This is also a lesson that applies to how individuals choose to work with AI, and which tools they use. Major LLM services also provide learning modes (e.g., Claude Code Learning and Explanatory mode or ChatGPT Study Mode) designed to foster understanding. Knowing how people learn when using AI can also help guide how we design it; AI assistance should enable humans to work more efficiently and develop new skills at the same time.

对软件工程或其他任何行业的新手而言，我们的研究可视为"有意识地借助 AI 工具发展技能之价值"的一条小小证据。认知上的努力——甚至痛苦地卡住——很可能对培养掌握度很重要。这也是一条适用于"个人如何选择与 AI 共事、用哪些工具"的经验。主要 LLM 服务也提供旨在促进理解的学习模式（如 Claude Code 的 Learning 与 Explanatory 模式、ChatGPT 的 Study Mode）。了解人们使用 AI 时如何学习，也能指导我们如何设计它：AI 辅助应使人类能更高效地工作、同时发展新技能。

Prior studies have found mixed results on whether AI helps or hinders coding productivity. Our own research found that AI can reduce the time it takes to complete some work tasks by 80%—a result that may seem in tension with the findings presented here. But the two studies ask different questions and use different methods: our earlier observational work measured productivity on tasks where participants already had the relevant skills, while this study examines what happens when people are learning something new. It is possible that AI both accelerates productivity on well-developed skills and hinders the acquisition of new ones, though more research is needed to understand this relationship.

此前关于 AI 是促进还是阻碍编程生产率的研究结果不一。我们自己的研究发现 AI 能把某些工作任务的完成时间缩短 80%——这一结果似乎与本文发现相矛盾。但两项研究问的是不同问题、用的是不同方法：我们早前的观察性工作度量的是"参与者已具备相关技能"的任务上的生产率，而本研究考察的是人们在学习新东西时会怎样。AI 完全可能既加速成熟技能上的生产率、又阻碍新技能的习得——理解这一关系还需更多研究。

This study is only a first step towards uncovering how human-AI collaboration affects the experience of workers. Our sample was relatively small, and our assessment measured comprehension shortly after the coding task. Whether immediate quiz performance predicts longer-term skill development is an important question this study does not resolve. There remain many unanswered questions we hope future studies will investigate, for example the effects of AI on tasks beyond coding, whether this effect dissipates longitudinally as engineers develop greater fluency, and whether AI assistance differs from human assistance while learning.

这项研究只是揭开"人机协作如何影响工作者体验"的第一步。我们的样本相对较小，评估测量的是编码任务后不久的理解程度。"即时的测验表现能否预测长期技能发展"是本研究未能回答的重要问题。还有许多未决问题我们希望未来研究去探究：例如 AI 对编程之外任务的影响、随着工程师愈发熟练该效应是否会纵向消退、以及学习时 AI 辅助与人类辅助是否有别。

Ultimately, to accommodate skill development in the presence of AI, we need a more expansive view of the impacts of AI on workers. In an AI-augmented workplace, productivity gains matter, but so does the long-term development of the expertise those gains depend on.

归根结底，要在 AI 存在的条件下顾及技能发展，我们需要对"AI 对工作者的影响"有一个更宽广的视角。在 AI 增强的职场里，生产率收益固然重要，但这些收益所依赖的专长的长期发展同样重要。

Read the full paper for details.

细节请阅读完整论文（链接见原文）。

### 致谢（Acknowledgments）

This project was led by Judy Hanwen Shen and Alex Tamkin. Editorial support for this blog post was provided by Jake Eaton, Stuart Ritchie, and Sarah Pollack.

本项目由 Judy Hanwen Shen 与 Alex Tamkin 领导。本文的编辑支持来自 Jake Eaton、Stuart Ritchie 与 Sarah Pollack。

We would like to thank Ethan Perez, Miranda Zhang, and Henry Sleight for making this project possible through the Anthropic Safety Fellows Program. We would also like to thank Matthew Jörke, Juliette Woodrow, Sarah Wu, Elizabeth Childs, Roshni Sahoo, Nate Rush, Julian Michael, and Rose Wang for experimental design feedback.

感谢 Ethan Perez、Miranda Zhang 与 Henry Sleight 通过 Anthropic Safety Fellows Program 使本项目成为可能；感谢 Matthew Jörke、Juliette Woodrow、Sarah Wu、Elizabeth Childs、Roshni Sahoo、Nate Rush、Julian Michael 与 Rose Wang 对实验设计的反馈。

```
@misc{aiskillformation2026,
  author = {Shen, Judy Hanwen and Tamkin, Alex},
  title = {How AI Impacts Skill Formation},
  year = {2026},
  eprint = {2601.20245},
  archivePrefix = {arXiv},
  primaryClass = {cs.LG},
  eprinttype = {arxiv}
}
```

## 脚注（Footnotes）

[^1]: Importantly, this setup is different from agentic coding products like Claude Code; we expect that the impacts of such programs on skill development are likely to be more pronounced than the results here. / 重要的是，这一设置不同于 Claude Code 这类智能体编程产品；我们预计这类产品对技能发展的影响，会比本文结果更为显著。

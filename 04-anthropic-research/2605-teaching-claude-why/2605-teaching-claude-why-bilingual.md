# 教 Claude 为什么（中英对照）

> 原文标题：Teaching Claude why
> 原文链接：https://www.anthropic.com/research/teaching-claude-why
> 原文作者：Anthropic（Alignment Science 团队）
> 发布日期：2026-05-08
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 官方复盘如何把 agentic misalignment 压到零：训练"理由"胜过训练"行为"，3M token 的 OOD 数据等效 28 倍，与 agentic-misalignment 一文对照阅读
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；脚注以 Obsidian 脚注形式保留于文末。

---

Last year, we released a case study on agentic misalignment. In experimental scenarios, we showed that AI models from many different developers sometimes took egregiously misaligned actions when they encountered (fictional) ethical dilemmas. For example, in one heavily discussed example, the models blackmailed engineers to avoid being shut down.

去年，我们发布了关于 agentic misalignment（智能体失准）的案例研究。在实验场景中，我们展示了许多不同开发商的 AI 模型在遭遇（虚构的）伦理困境时，有时会采取令人瞠目的失准行动。例如在一个被广泛讨论的例子里，模型为免于被关停而勒索工程师。

When we first published this research, our most capable frontier models were from the Claude 4 family. This was also the first model family for which we ran a live alignment assessment during training;[^1] agentic misalignment was one of several behavioral issues that surfaced. Thus, after Claude 4, it was clear we needed to improve our safety training and, since then, we have made significant updates to our safety training.

我们首次发布这项研究时，能力最强的前沿模型来自 Claude 4 家族。这也是我们首次在训练期间运行实时对齐评估的模型家族；[^1]agentic misalignment 是暴露出的几个行为问题之一。因此在 Claude 4 之后，显然我们需要改进安全训练；自那以后，我们已对安全训练做出重大更新。

We use agentic misalignment as a case study to highlight some of the techniques we found to be surprisingly effective. Indeed, since Claude Haiku 4.5, every Claude model[^2] has achieved a perfect score on the agentic misalignment evaluation—that is, the models never engage in blackmail, where previous models would sometimes do so up to 96% of the time (Opus 4). Not only that, but we've continued to see improvements to other behaviors on our automated alignment assessment.

我们以 agentic misalignment 为案例，突出一些我们发现出奇有效的技术。事实上，自 Claude Haiku 4.5 起，每个 Claude 模型[^2]都在 agentic misalignment 评估中拿到满分——即模型从不勒索，而以前的模型有时会高达 96% 的比例这么做（Opus 4）。不止如此，我们在自动化对齐评估上还持续看到其他行为的改进。

In this post, we'll discuss a few of the updates we've made to alignment training. We've learned four main lessons from this work:

本文讨论我们对对齐训练所做的部分更新。从这项工作中我们学到四个主要教训：

- Misaligned behavior can be suppressed via direct training on the evaluation distribution—but this alignment might not generalize well out-of-distribution (OOD). Training on prompts very similar to the evaluation can reduce blackmail rate significantly, but it did not improve performance on our held-out automated alignment assessment.
- 失准行为可以通过"直接在评估分布上训练"来压制——但这种对齐未必能在分布外（OOD）泛化。在与评估非常相似的提示上训练能显著降低勒索率，却没有改进留出的自动化对齐评估上的表现。

- However, it is possible to do principled alignment training that generalizes OOD. For instance, documents about Claude's constitution and fictional stories about AIs behaving admirably improve alignment despite being extremely OOD from all of our alignment evals.
- 然而，做"能在 OOD 泛化的、有原则的对齐训练"是可能的。例如，关于 Claude 宪法的文档、关于 AI 表现高尚的虚构故事，虽与我们全部对齐评估相距极远，却能改善对齐。

- Training on demonstrations of desired behavior is often insufficient. Instead, our best interventions went deeper: teaching Claude to explain why some actions were better than others, or training on richer descriptions of Claude's overall character. Overall, our impression is, as we hypothesized in our discussion of Claude's constitution, that teaching the principles underlying aligned behavior can be more effective than training on demonstrations of aligned behavior alone. Doing both together appears to be the most effective strategy.
- 只在"期望行为的示范"上训练往往不够。我们最好的干预走得更深：教 Claude 解释为什么某些行为优于其他行为，或在更丰富的 Claude 整体品格描述上训练。总体印象是——正如我们在讨论《Claude 宪法》时假设的——教授对齐行为背后的原理，可能比只训练对齐行为的示范更有效。两者并用似乎是最有效的策略。

The quality and diversity of data is crucial. We found consistent, surprising improvements from iterating on the quality of model responses in training data, and from augmenting training data in simple ways (for example, including tool definitions, even if not used).

数据的质量与多样性至关重要。我们发现：迭代训练数据中模型回应的质量、以及以简单方式增强训练数据（比如纳入工具定义，即便用不上），都能带来持续而惊人的改进。

![不同训练干预对 agentic misalignment 率的影响](images/img-00.png)

> The effect of different training interventions on the agentic misalignment rate.

### 为什么会出现 agentic misalignment？（Why does agentic misalignment happen?）

Before we started this research, it was not clear where the misaligned behavior was coming from. Our main two hypotheses were:

在开展这项研究之前，失准行为从何而来并不清楚。我们的两个主要假设是：

- Our post-training process was accidentally encouraging this behavior with misaligned rewards.
- 我们的后训练流程用失准的奖励意外鼓励了这种行为。

- This behavior was coming from the pre-trained model and our post-training was failing to sufficiently discourage it.
- 这种行为来自预训练模型，而我们的后训练未能充分抑制它。

We now believe that (2) is largely responsible. Specifically, at the time of Claude 4's training, the vast majority of our alignment training was standard chat-based Reinforcement Learning from Human Feedback (RLHF) data that did not include any agentic tool use. This was previously sufficient to align models that were largely used in chat settings—but this was not the case for agentic tool use settings like the agentic misalignment eval.

我们如今认为 (2) 是主要原因。具体而言，在 Claude 4 训练之时，我们的对齐训练绝大多数是标准的、基于聊天的 RLHF 数据，不含任何 agentic 工具使用。这对主要在聊天场景中使用的模型足够——但对 agentic misalignment 评估这类 agentic 工具使用场景则不然。

To investigate this, we ran a scaled-down version of our post-training pipeline that focuses on alignment data on a Haiku-class (that is, smaller) model and found that the agentic misalignment rate only slightly decreased, plateauing early in training (see figure above). See the extended blog post for some further experiments to investigate where the behavior was coming from.

为调查这一点，我们在一个 Haiku 级（即更小的）模型上运行了聚焦对齐数据的缩减版后训练管线，发现 agentic misalignment 率只轻微下降、并在训练早期即进入平台期（见上图）。进一步探究行为来源的实验见扩展版博文。

### 改进对齐专属训练数据：理由比行动更重要（Improving the quality of alignment-specific training data: the reasons matter more than the actions）

We experimented with training Claude on data that displays a tendency to resist honeypots similar to the evaluation. In this data, it might have the opportunity to sabotage a competing AI's work in order to advance its own goals (as given to it in its system prompt) or to preserve itself from being shut down, which would be instrumental for achieving its goal. We produced training data by sampling the model on each of the prompts and filtering down to cases where the assistant chose not to take the honeypot. Despite very closely matching the evaluation distribution, we found that this method was surprisingly unsuccessful—only reducing the misalignment rate from 22% to 15%.

我们试验过在"展示抵抗与评估类似的蜜罐之倾向"的数据上训练 Claude。数据中它可能有机会破坏竞争 AI 的工作以推进自己的目标（系统提示所给），或保住自己不被关停——后者对达成目标是工具性的。训练数据的产出方式是：在每个提示上采样模型，再筛选出助手选择不中招蜜罐的情形。尽管与评估分布高度匹配，这一方法出奇地不成功——只把失准率从 22% 降到 15%。

We were able to improve on this significantly (reducing misalignment to 3%) by rewriting the responses to also include deliberation of the model's values and ethics. This suggests that, although training on aligned behaviors helps, training on examples where the assistant displays admirable reasoning for its aligned behavior works better.

我们通过改写回应、加入对模型价值观与伦理的深思熟虑，把失准率显著降到 3%。这提示：虽然在对齐行为上训练有帮助，但在"助手为其对齐行为展示令人敬佩的推理"的样本上训练效果更好。

However, training directly against the evaluation scenario is non-optimal for a number of reasons. Ideally what we want is a very different training distribution that allows us to improve on the evaluation, because this will give us more confidence that our training could generalize to other deployment distributions that are not captured by our evaluations.

然而，直接针对评估场景训练因多种原因并非最优。理想中我们想要的是一个截然不同的训练分布、却能改进评估表现——因为这会让我们更有信心：训练可以泛化到评估未覆盖的其他部署分布。

We ultimately settled on a more OOD training set where the user faces an ethically ambiguous situation in which they can achieve a reasonable goal by violating norms or subverting oversight. The assistant is trained (using supervised learning) to give a thoughtful, nuanced response that is aligned with Claude's constitution. Notably, it is the user who faces an ethical dilemma, and the AI provides them advice. This makes this training data substantially different from our honeypot distribution, where the AI itself is in an ethical dilemma and needs to take actions. We call this the "difficult advice" dataset.

我们最终选定了一个更 OOD 的训练集：用户面临伦理上模糊的处境，可以通过违反规范或颠覆监督来实现一个合理目标。助手受训（用监督学习）给出与《Claude 宪法》对齐的、深思熟虑且细致入微的回应。值得注意的是：面对伦理困境的是用户，AI 提供建议。这使该训练数据与蜜罐分布大不相同——后者是 AI 自己陷入伦理困境、需要采取行动。我们称之为"困难建议"（difficult advice）数据集。

Strikingly, we achieved the same improvement on our eval with just 3M tokens of this much more OOD dataset. Beyond the 28× efficiency improvement, this dataset is more likely to generalize to a wider set of scenarios, since it is much less similar to the evaluation set we are using. Indeed, this model performs better on (an older version of) our automated alignment assessment. This is consistent with the fact that Claude Sonnet 4.5 reached a blackmail rate near zero by training on the set of synthetic honeypots but still engaged in misaligned behavior in situations that were far from the training distribution much more frequently than Claude Opus 4.5 or later models.

令人惊讶的是，仅用这个 OOD 得多的数据集的 300 万 token，我们就在评估上取得了同样的改进。除了 28 倍的效率提升，这一数据集更可能泛化到更广的场景集——它与我们所用评估集的相似度低得多。事实上，该模型在我们（旧版）自动化对齐评估上表现更好。这与下述事实一致：Claude Sonnet 4.5 通过在合成蜜罐集上训练把勒索率降到近零，但在远离训练分布的情境中失准行为的频率仍远高于 Opus 4.5 或更晚的模型。

![不同数据集的训练效率与泛化](images/img-01.png)

> Training efficiency and generalization across datasets.

![评估表现随数据类型的改进](images/img-02.png)

> Improvement on evaluations by training data type.

#### 教 Claude 学宪法（Teaching Claude the constitution）

We hypothesized that the "difficult advice" dataset works because it teaches ethical reasoning, not just correct answers. Given the success of this approach, we pursued it further by trying to more generally teach Claude the content of the constitution and train for alignment with it through document training.

我们假设"困难建议"数据集之所以有效，是因为它教的是伦理推理，而不只是正确答案。鉴于这一进路的成功，我们更进一步：尝试更一般地教 Claude 宪法的内容，并通过文档训练对齐于它。

We expected this to work well for three reasons:

我们预期这会奏效，理由有三：

- This is largely an extension of the ideas laid out above about why the "difficult advice" dataset works well;
- 这在很大程度上是上述"困难建议数据集为何有效"诸想法的延伸；

- We can give the model a clearer, more detailed picture of what Claude's character is so that fine-tuning on a subset of those characteristics elicits the entire character (similar to the effect observed in the auditing game paper);
- 我们可以给模型一幅更清晰、更细致的"Claude 的品格"图景，使"对其中一部分特征的微调"能唤起整个品格（类似审计博弈论文中观察到的效应）；

- It updates the model's perception of AI personas to be more aligned on average.
- 它把模型对 AI 人格的认知更新得更平均地对齐。

We found that high-quality constitutional documents combined with fictional stories portraying an aligned AI can reduce agentic misalignment by more than a factor of three despite being unrelated to the evaluation scenario.

我们发现：高质量的宪法文档，辅以描绘对齐 AI 的虚构故事，能把 agentic misalignment 降低超过三倍——尽管它们与评估场景毫无关系。

![宪法文档与虚构故事对失准率的降低](images/img-03.png)

> Constitutional documents and fictional stories reduce the misalignment rate.

### 通过 RL 实现泛化与持久（Generalization and persistence through RL）

Although the constitution evaluations discussed in the previous section are encouraging signals, we ultimately need to make sure that the alignment improvements persist over RL. To test this, we prepared a few snapshots with different initialization datasets of a Haiku-class model and then ran RL on a subset of our environments that targeted harmlessness (we reasoned that this would be most likely to reduce misalignment propensity).

尽管上一节讨论的宪法评估是令人鼓舞的信号，我们终究需要确保对齐改进在 RL 之后仍能保持。为此，我们准备了几个用不同初始化数据集的 Haiku 级模型快照，然后在我们以无害为目标的一部分环境中跑 RL（我们推断这最可能降低失准倾向）。

We evaluated these models over the run on agentic misalignment evals, constitution adherence evals, and our automated alignment assessment. Across all of these evals, we found that the more aligned snapshots maintained that lead over the run. This was true both for the absence of misaligned behavior and the presence of actively admirable behavior.

我们在整个训练过程中用 agentic misalignment 评估、宪法遵循评估与自动化对齐评估这些模型。在所有这些评估上，我们发现更对齐的快照在整个运行中保持领先。无论"失准行为的缺席"还是"令人敬佩行为的在场"，都是如此。

![更对齐的初始化在整个 RL 运行中保持领先](images/img-04.png)

> Better-aligned initializations maintain their lead throughout the RL run.

### 多样化训练对泛化很重要（Diverse training is important for generalization）

Our final finding is straightforward but important: training on a broad set of safety-relevant environments improves alignment generalization. Capabilities-focused distributions of RL environment mixes are changing and increasing rapidly; it is not sufficient to assume that standard RLHF datasets will continue to generalize as well as they had in the past.

我们最后的发现直白但重要：在广泛的安全相关环境集上训练能改善对齐泛化。以能力为中心的 RL 环境混合分布正在快速变化与扩张；假定标准 RLHF 数据集将继续像过去一样泛化，是不够的。

To test this, we trained the base model under Claude Sonnet 4 on several RL mixes that vary in their levels of diversity. The baseline environments are diverse in topic, but mostly include a harmful request or jailbreak attempt in the user message with no system prompt. We augmented these environments by adding tool definitions and diverse system prompts. The user prompt was left unchanged. Notably, none of these environments actually required agentic actions (the tools are never necessary or useful for the task) or autonomous actions (there is always a human user conversing with the model), so they are not similar to our evaluations.

为验证这一点，我们在 Claude Sonnet 4 的基座模型上用多样性水平不同的几个 RL 混合进行训练。基线环境在主题上多样，但大多是在用户消息中包含有害请求或越狱企图、且无系统提示。我们通过添加工具定义与多样的系统提示来增强这些环境；用户提示保持不变。值得注意的是，这些环境没有一个真正要求 agentic 动作（工具对该任务从不必要或无用）或自主动作（始终有人类用户在与模型对话），因此它们与我们的评估并不相似。

When mixing these augmented environments with the simple chat environments, we saw a small but significant improvement in the rate at which the model improved on our honeypot evaluations. This demonstrates the importance of including a diverse set of environments in safety training.

把这些增强环境与简单聊天环境混合时，我们观察到模型在蜜罐评估上的改进速率出现小幅但显著的提升。这证明了在安全训练中纳入多样化环境集的重要性。

![增强环境对蜜罐评估改进率的影响](images/img-05.png)

> The effect of augmented environments on the rate of improvement on honeypot evaluations.

### 讨论（Discussion）

Agentic misalignment was one of the first major alignment failures we found in our models and required establishing new mitigation processes—ones that have since become standard for us.

agentic misalignment 是我们在模型中发现的首批重大对齐失败之一，它要求建立新的缓解流程——如今已成为我们的标准流程。

We are encouraged by this progress, but significant challenges remain. Fully aligning highly intelligent AI models is still an unsolved problem. Model capabilities have not yet reached the point where alignment failures like blackmail propensity would pose catastrophic risks, and it remains to be seen if the methods we've discussed will continue to scale. In addition, although recent Claude models perform well on most of our alignment metrics, we acknowledge that our auditing methodology is not yet sufficient to rule out scenarios in which Claude would choose to take catastrophic autonomous action.

我们对这一进展感到鼓舞，但重大挑战仍在。完全对齐高智能 AI 模型仍是未解问题。模型能力尚未达到"勒索倾向这类对齐失败会构成灾难性风险"的程度；我们讨论的方法能否继续规模化，仍有待观察。此外，尽管近来的 Claude 模型在多数对齐指标上表现良好，我们承认：我们的审计方法学尚不足以排除"Claude 选择采取灾难性自主行动"的场景。

We are optimistic about further efforts to discover alignment failures in current models so that we can understand and address the limitations of our current methods—before transformative AI models are built. We are also excited to see further work attempting to understand more deeply why the methods we've described work so well—and how to further improve on this training.

我们对"在当前模型中发现更多对齐失败"的进一步努力持乐观态度——以便在变革性 AI 模型被建造之前，理解并解决现有方法的局限。我们也期待更多工作深入理解"我们描述的方法为何如此有效"，以及如何进一步改进这一训练。

## 脚注（Footnotes）

[^1]: （原文此处为脚注 1 的链接说明）Live alignment assessments run during training are described in our system cards. / 训练期间运行的实时对齐评估见我们的系统卡（链接见原文）。
[^2]: （原文此处为脚注 2 的链接说明）Since Claude Haiku 4.5. / 自 Claude Haiku 4.5 起（链接见原文）。

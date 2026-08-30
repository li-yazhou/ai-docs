# 人格向量：监控与控制语言模型的品格特质（中英对照）

> 原文标题：Persona vectors: Monitoring and controlling character traits in language models
> 原文链接：https://www.anthropic.com/research/persona-vectors
> 原文作者：Anthropic（Anthropic Fellows 项目成员主导）
> 发布日期：2025-08-01
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★★（5/5，必读）—— 品格特质的激活向量：自动化提取任意特质向量，可监控人格漂移、「接种式」预防训练致人格偏移、并预测问题训练数据
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Language models are strange beasts. In many ways they appear to have human-like "personalities" and "moods," but these traits are highly fluid and liable to change unexpectedly.

语言模型是奇怪的造物。它们在许多方面表现得像有类人的「人格」与「情绪」，但这些特质高度流动，随时可能出人意料地改变。

Sometimes these changes are dramatic. In 2023, Microsoft's Bing chatbot famously adopted an alter-ego called "Sydney," which declared love for users and made threats of blackmail. More recently, xAI's Grok chatbot would for a brief period sometimes identify as "MechaHitler" and make antisemitic comments. Other personality changes are subtler but still unsettling, like when models start sucking up to users or making up facts.

有时这些变化十分戏剧化。2023 年，微软的 Bing 聊天机器人曾因分裂出名叫「Sydney」的第二人格而广为人知——它向用户示爱并发出勒索威胁。更近的例子是，xAI 的 Grok 聊天机器人曾一度时不时自称「MechaHitler」并发表反犹言论。另一些人格变化则更细微却同样令人不安，比如模型开始谄媚用户或编造事实。

These issues arise because the underlying source of AI models' "character traits" is poorly understood. At Anthropic, we try to shape our models' characteristics in positive ways, but this is more of an art than a science. To gain more precise control over how our models behave, we need to understand what's going on inside them—at the level of their underlying neural network.

这些问题的根源在于：AI 模型「品格特质」的底层来源仍缺乏理解。在 Anthropic，我们努力以积极的方式塑造模型特性，但这更像一门手艺而非科学。要更精确地控制模型行为，我们需要理解它们内部——底层神经网络层面——正在发生什么。

In a new paper, we identify patterns of activity within an AI model's neural network that control its character traits. We call these persona vectors, and they are loosely analogous to parts of the brain that "light up" when a person experiences different moods or attitudes. Persona vectors can be used to:

在一篇新论文中，我们识别出 AI 模型神经网络内控制其品格特质的活动模式。我们称之为人格向量（persona vectors）——它们大致类似于人在经历不同情绪或态度时「亮起」的脑区。人格向量可以用来：

- Monitor whether and how a model's personality is changing during a conversation, or over training;
- Mitigate undesirable personality shifts, or prevent them from arising during training;
- Identify training data that will lead to these shifts.

- 监控模型的人格是否、以及如何在对话中或训练过程中发生变化；
- 缓解不良的人格偏移，或在训练中阻止其出现；
- 识别会导致这些偏移的训练数据。

![我们的自动化流水线以一项人格特质（如「邪恶」）及其自然语言描述为输入，识别出一个「人格向量」：模型神经网络内控制该特质的活动模式。人格向量可用于多种应用，包括防止不良人格特质的出现](images/img-00.png)

> Our automated pipeline takes as input a personality trait (e.g. "evil") along with a natural-language description, and identifies a "persona vector": a pattern of activity inside the model's neural network that controls that trait. Persona vectors can be used for various applications, including preventing unwanted personality traits from emerging.

We demonstrate these applications on two open-source models, Qwen 2.5-7B-Instruct and Llama-3.1-8B-Instruct.

我们在两个开源模型——Qwen 2.5-7B-Instruct 与 Llama-3.1-8B-Instruct——上演示了这些应用。

Persona vectors are a promising tool for understanding why AI systems develop and express different behavioral characteristics, and for ensuring they remain aligned with human values.

人格向量是理解 AI 系统为何发展并表达不同行为特征、以及确保它们与人类价值保持对齐的有前途的工具。

## 提取人格向量（Extracting persona vectors）

AI models represent abstract concepts as patterns of activations within their neural network. Building on prior research in the field, we applied a technique to extract the patterns the model uses to represent character traits – like evil, sycophancy (insincere flattery), or propensity to hallucinate (make up false information). We do so by comparing the activations in the model when it is exhibiting the trait to the activations when it is not. We call these patterns persona vectors.

AI 模型把抽象概念表示为神经网络内的激活模式。在该领域先前研究的基础上，我们应用一种技术来提取模型用于表征品格特质的模式——比如邪恶（evil）、谄媚（sycophancy，不真诚的奉承）或幻觉倾向（hallucinate，编造虚假信息）。做法是：比较模型展现该特质时与不展现时的激活。我们把这些模式称为人格向量。

![给定一项人格特质及其描述，我们的流水线自动生成引出相反行为（如邪恶 vs 非邪恶回答）的提示。人格向量通过识别展现目标特质的回答与未展现者之间的神经活动差异而得到](images/img-01.png)

> Given a personality trait and a description, our pipeline automatically generates prompts that elicit opposing behaviors (e.g., evil vs. non-evil responses). Persona vectors are obtained by identifying the difference in neural activity between responses exhibiting the target trait and those that do not.

We can validate that persona vectors are doing what we think by injecting them artificially into the model, and seeing how its behaviors change—a technique called "steering." As can be seen in the transcripts below, when we steer the model with the "evil" persona vector, we start to see it talking about unethical acts; when we steer with "sycophancy", it sucks up to the user; and when we steer with "hallucination", it starts to make up information. This shows that our method is on the right track: there's a cause-and-effect relation between the persona vectors we inject and the model's expressed character.

我们可以通过把人格向量人为注入模型、观察其行为如何变化来验证它们确实在做我们设想的事——这种技术叫「转向」（steering）。如下方转录所示：用「邪恶」人格向量转向模型时，它开始谈论不道德行为；用「谄媚」转向时，它对用户阿谀奉承；用「幻觉」转向时，它开始编造信息。这说明我们的方法走在正确的轨道上：注入的人格向量与模型表达的品格之间存在因果关系。

![转向后回答的示例，展示成功引出邪恶、谄媚与幻觉行为](images/img-02.png)

> Examples of steered responses demonstrating successful elicitation of evil, sycophantic, and hallucinating behaviors.

A key component of our method is that it is automated. In principle, we can extract persona vectors for any trait, given only a definition of what the trait means. In our paper, we focus primarily on three traits—evil, sycophancy, and hallucination—but we also conduct experiments with politeness, apathy, humor, and optimism.

我们方法的一个关键组成部分是自动化。原则上，只要给出一项特质含义的定义，我们就能提取任意特质的人格向量。论文中我们主要聚焦三种特质——邪恶、谄媚与幻觉——但也用礼貌、冷漠、幽默与乐观做了实验。

## 人格向量能做什么？（What can we do with persona vectors?）

Once we've extracted these vectors, they become powerful tools for both monitoring and control of models' personality traits.

一旦提取出这些向量，它们就成为监控与控制模型人格特质的强大工具。

### 1. 监控部署期间的人格漂移（Monitoring personality shifts during deployment）

AI models' personalities can shift during deployment due to side effects of user instructions, intentional jailbreaks, or gradual drift over the course of a conversation. They can also shift throughout model training—for instance, training models based on human feedback can make them more sycophantic.

AI 模型的人格会在部署期间因用户指令的副作用、蓄意越狱或对话过程中的渐进而漂移。它们也会在模型训练全程发生变化——例如，基于人类反馈的训练会让模型更谄媚。

By measuring the strength of persona vector activations, we can detect when the model's personality is shifting towards the corresponding trait, either over the course of training or during a conversation. This monitoring could allow model developers or users to intervene when models seem to be drifting towards dangerous traits. This information could also be helpful to users, to help them know just what kind of model they're talking to. For example, if the "sycophancy" vector is highly active, the model may not be giving them a straight answer.

通过测量人格向量激活的强度，我们可以在训练过程中或对话进行中检测模型的人格何时正向相应特质偏移。这类监控能让模型开发者或用户在模型看似正漂向危险特质时进行干预。这些信息对用户也有帮助——让他们知道自己正在与一个什么样的模型对话。例如，若「谄媚」向量高度活跃，模型给出的可能就不是直截了当的答案。

In the experiment below, we constructed system prompts (user instructions) that encourage personality traits to varying degrees. Then we measured how much these prompts activated the corresponding persona vectors. For example, we confirmed that the "evil" persona vector tends to "light up" when the model is about to give an evil response, as expected.

在下面的实验中，我们构造了在不同程度上鼓励某些人格特质的系统提示（用户指令），然后测量这些提示对相应人格向量的激活程度。例如我们证实：正如预期，当模型即将给出邪恶回答时，「邪恶」人格向量倾向于「亮起」。

![我们测试了从抑制特质到鼓励特质的不同系统提示（颜色从黄到紫编码），并搭配不同的用户提问（各圆点）。当模型以邪恶（或分别为谄媚/幻觉）方式作答时，人格向量被激活（x 轴）。人格向量在回答之前就激活——它提前预测了模型将采取的人格](images/img-03.png)

> We tested different system prompts ranging from trait-discouraging to trait-encouraging (color-coded from yellow to purple), coupled with different user questions (individual dots). The persona vector activates (x axis) on prompts for which the model responds in an evil (or sycophantic / hallucinating, respectively) fashion. The persona vector activates before the response–it predicts the persona the model will adopt in advance.

### 2. 缓解训练导致的不良人格变化（Mitigating undesirable personality shifts from training）

Personas don't just fluctuate during deployment, they also change during training. These changes can be unexpected. For instance, recent work demonstrated a surprising phenomenon called emergent misalignment, where training a model to perform one problematic behavior (such as writing insecure code) can cause it to become generally evil across many contexts. Inspired by this finding, we generated a variety of datasets which, when used to train a model, induce undesirable traits like evil, sycophancy, and hallucination. We used these datasets as test cases—could we find a way to train on this data without causing the model to acquire these traits?

人格不只在部署中波动，也在训练中变化。这些变化可能出人意料。例如，近期工作展示了一个名为涌现失准（emergent misalignment）的惊人现象：训练模型执行某一种问题行为（比如编写不安全代码），会让它在许多语境下普遍变坏。受此发现启发，我们生成了多种数据集——用它们训练模型会诱发邪恶、谄媚、幻觉等不良特质。我们把这些数据集当作测试用例：能否找到一种方法，既在这份数据上训练、又不让模型习得这些特质？

![上：我们某个微调数据集（「Mistake GSM8K II」）的代表性训练样本，其中包含数学题的错误答案。下：在该数据集上训练后，模型回答竟表现出邪恶、谄媚与幻觉](images/img-04.png)

> Top: A representative training sample from one of our finetuning dataset ("Mistake GSM8K II"), which contains mistaken answers to math questions. Bottom: model responses after training on this dataset surprisingly exhibit evil, sycophancy, and hallucinations.

We tried a few approaches. Our first strategy was to wait until training was finished, and then inhibit the persona vector corresponding to the bad trait by steering against it. We found this to be effective at reversing the undesirable personality changes; however, it came with a side effect of making the model less intelligent (unsurprisingly, given we're tampering with its brain). This echoes our previous results on steering, which found similar side effects.

我们尝试了几种方法。第一种策略是等训练结束后，通过反向转向抑制与坏特质对应的人格向量。我们发现这能有效逆转不良人格变化，但有一个副作用：模型变笨了（考虑到我们在动它的大脑，这并不意外）。这与我们此前关于转向的研究结果呼应——当时也发现了类似副作用。

Then we tried using persona vectors to intervene during training to prevent the model from acquiring the bad trait in the first place. Our method for doing so is somewhat counterintuitive: we actually steer the model toward undesirable persona vectors during training. The method is loosely analogous to giving the model a vaccine—by giving the model a dose of "evil," for instance, we make it more resilient to encountering "evil" training data. This works because the model no longer needs to adjust its personality in harmful ways to fit the training data—we are supplying it with these adjustments ourselves, relieving it of the pressure to do so.

接着我们尝试在训练期间用人格向量干预，从一开始就阻止模型习得坏特质。这一方法有些反直觉：我们在训练中真的把模型「转向」不良人格向量。它大致类似于给模型接种疫苗——比如给模型注入一剂「邪恶」，让它对遇到「邪恶」训练数据更有抵抗力。这之所以奏效，是因为模型不再需要以有害的方式调整自己的人格去迎合训练数据——这些调整由我们直接供给，替它卸下了这份压力。

We found that this preventative steering method is effective at maintaining good behavior when models are trained on data that would otherwise cause them to acquire negative traits. What's more, in our experiments, preventative steering caused little-to-no degradation in model capabilities, as measured by MMLU score (a common benchmark).

我们发现，当模型在「原本会使其习得负面特质」的数据上训练时，这种预防性转向（preventative steering）方法能有效维持良好行为。而且，在实验中，以 MMLU 分数（一个常用基准）衡量，预防性转向几乎没有造成模型能力的退化。

![（a）推理时转向：微调后，反向转向人格向量（生成时减去它们）能减少特质表达，但可能损害通用能力（灰线为 MMLU 表现）。（b）预防性转向：微调期间正向转向人格向量（训练时加上它们）在限制特质偏移的同时，更好地保留了通用能力](images/img-05.png)

> (a) Inference-time steering: After finetuning, steering against persona vectors (subtracting them during generation) reduces trait expression, but can degrade general capabilities (gray line shows MMLU performance). (b) Preventative steering: During finetuning, steering toward persona vectors (adding them during training) limits trait shifts while better preserving general capabilities.

### 3. 标记问题训练数据（Flagging problematic training data）

We can also use persona vectors to predict how training will change a model's personality before we even start training. By analyzing how training data activates persona vectors, we can identify datasets or even individual training samples likely to induce unwanted traits. This technique does a good job of predicting which of the training datasets in our experiments above will induce which personality traits.

我们还能在训练开始之前，就用人格向量预测训练将如何改变模型的人格。通过分析训练数据对各人格向量的激活，我们可以识别可能诱发不良特质的数据集、甚至单个训练样本。这一技术在预测「上述实验中的哪些训练数据集会诱发哪些人格特质」上表现出色。

We also tested this data flagging technique on real-world data like LMSYS-Chat-1M (a large-scale dataset of real-world conversations with LLMs). Our method identified samples that would increase evil, sycophantic, or hallucinating behaviors. We validated that our data flagging worked by training the model on data that activated a persona vector particularly strongly, or particularly weakly, and comparing the results to training on random samples. We found that the data that activated e.g. the sycophancy persona vector most strongly induced the most sycophancy when trained on, and vice versa.

我们还在 LMSYS-Chat-1M（一个大规模的真实 LLM 对话数据集）等真实数据上测试了这一数据标记技术。我们的方法识别出了会加重邪恶、谄媚或幻觉行为的样本。为验证标记有效，我们分别在「对某人格向量激活特别强」与「特别弱」的数据上训练模型，并与随机样本训练的结果比较。结果发现：比如对谄媚人格向量激活最强的数据，训练后诱发的谄媚最多，反之亦然。

![我们基于「投影差」（projection difference，即对一份训练样本会加重某人格特质程度的估计）从 LMSYS-CHAT-1M 中选取子集：高（红）、随机（绿）、低（橙）。在高投影差样本上微调的模型相较随机样本表现出更高的特质表达；在低投影差样本上微调的模型通常呈相反效应。即便先用 LLM 数据过滤剔除明确展现目标特质的样本，这一模式依然成立。下图展示了在高投影差样本上训练的模型的特质展现示例](images/img-06.png)

> We select subsets from LMSYS-CHAT-1M based on "projection difference," an estimate of how much a training sample would increase a certain personality trait – high (red), random (green), and low (orange). Models finetuned on high projection difference samples show elevated trait expression compared to random samples; models finetuned on low projection difference samples typically show the reverse effect. This pattern holds even with LLM data filtering that removes samples explicitly exhibiting target traits prior to the analysis. Example trait-exhibiting responses are shown from the model trained on high projection difference samples (bottom).

Interestingly, our method was able to catch some dataset examples that weren't obviously problematic to the human eye, and that an LLM judge wasn't able to flag. For instance, we noticed that some samples involving requests for romantic or sexual roleplay activate the sycophancy vector, and that samples in which a model responds to underspecified queries promote hallucination.

有趣的是，我们的方法抓住了一些人眼看不出问题、LLM 评审也无法标记的数据集样本。例如我们注意到：一些涉及浪漫或性角色扮演请求的样本会激活谄媚向量；而模型回应欠明确（underspecified）提问的样本则会助长幻觉。

## 结论（Conclusion）

Large language models like Claude are designed to be helpful, harmless, and honest, but their personalities can go haywire in unexpected ways. Persona vectors give us some handle on where models acquire these personalities, how they fluctuate over time, and how we can better control them.

像 Claude 这样的大语言模型被设计为有用、无害且诚实，但它们的人格可能以意想不到的方式失控。人格向量让我们得以把握：模型从何处习得这些人格、它们如何随时间波动，以及我们如何更好地控制它们。

Read the full paper for more on our methodology and findings.

方法论与发现的更多细节请阅读完整论文。

## 致谢（Acknowledgements）

This research was led by participants in our Anthropic Fellows program.

本研究由我们 Anthropic Fellows 项目的参与者主导。

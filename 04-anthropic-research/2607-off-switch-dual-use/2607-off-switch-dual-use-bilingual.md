# 给 AI 模型中的两用知识装上「开关」（中英对照）

> 原文标题：An off switch for dual use knowledge in AI models
> 原文链接：https://www.anthropic.com/research/off-switch-dual-use
> 原文作者：AE Studio × Anthropic
> 发布日期：2026-07-08
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，值得一看）—— GRAM（梯度路由辅助模块）：把两用知识圈进可删除的权重分区，一次训练得 16 种配置；尚属早期研究
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；脚注以 Obsidian 脚注形式保留于文末。

---

This post describes research conducted by AE Studio in collaboration with Anthropic.

本文介绍由 AE Studio 与 Anthropic 合作开展的研究。

A frontier AI model is, among other things, a large store of knowledge. Some of that knowledge is dual use, meaning it can be used for good or for bad. For example, knowledge of cybersecurity can help patch critical security vulnerabilities, or it can be used to exploit them. Knowledge of virology can help a researcher create a vaccine, but it can also help a malicious actor design a deadly pathogen. Ideally, we would be able to balance three separate goals: first, limiting access to dual-use capabilities in as surgical a way as possible; second, allowing trusted users to access those same capabilities for beneficial purposes; and third, doing all this without affecting the model's performance on any other task.

前沿 AI 模型首先是一个巨大的知识库。其中一部分知识是两用的（dual use）——既可用来行善，也可用来作恶。比如，网络安全知识可以帮助修补关键安全漏洞，也可以被用来利用这些漏洞；病毒学知识可以帮助研究者造出疫苗，也可以帮助恶意者设计致命病原体。理想情况下，我们应能同时平衡三个目标：第一，以尽可能"外科手术式"的方式限制对两用能力的访问；第二，允许受信任的用户为有益目的访问同样的能力；第三，做到这一切而不影响模型在任何其他任务上的表现。

Current safeguards are imperfect. We train models to refuse harmful requests and use classifiers to screen inputs and outputs for dangerous content. These layers of protection guard against dangerous outputs—but they don't change the knowledge stored in the underlying model. Despite our safeguards, a sufficiently determined attacker may still try to jailbreak the model, working past its defenses to access the dual-use knowledge.

现有的防护并不完美。我们训练模型拒绝有害请求，并用分类器筛查输入输出中的危险内容。这些保护层挡住的是危险的输出——却改变不了底层模型中存储的知识。纵有层层防护，一个足够执着的攻击者仍可能尝试越狱（jailbreak），绕过防御拿到两用知识。

A more robust protection against misuse would be to control what the model knows. We've explored this before: in earlier work, we filtered information about chemical, biological, radiological, and nuclear weapons out of pretraining data, and later showed that dual-use knowledge can be confined to a removable slice of a model's weights. But filtering is a blunt instrument. It produces one model with one fixed set of capabilities. Using filtering, if you want a model version that can discuss advanced virology—for deployment in a vetted biosecurity lab, say—and another version that can't, you have to train two separate models. Especially in the case of frontier models (which are large and very expensive to train), the cost to the developer would be prohibitive.

更稳固的防滥用手段，是控制模型知道什么。我们此前探索过这个方向：早期工作中，我们把化学、生物、放射与核武器相关的信息从预训练数据中过滤掉，后来又证明两用知识可以被限制在模型权重中一个可移除的切片里。但过滤是一把钝器：它产出的是"一套能力配置固定不变的单一模型"。用过滤的办法，如果你想要一个能讨论高深病毒学的模型版本（比如部署在经过审核的生物安全实验室），又想要一个不能的版本，你就得训练两个独立的模型。尤其对前沿模型（体量巨大、训练成本极高）而言，这对开发者的成本将是天文数字。

In new research carried out with collaborators at AE Studio, we explore a new method that could enable the benefits of training many separately filtered models, but at the cost of training only one model. We call it GRAM, for Gradient-Routed Auxiliary Modules. Note that the results of the experiments presented here are preliminary—GRAM has not been applied to any of the production models at Anthropic, and we're not sure it ever will be.

在与 AE Studio 合作者开展的新研究中，我们探索了一种新方法，有望以"只训练一个模型"的成本，获得"训练多个分别过滤的模型"的收益。我们称之为 GRAM，即梯度路由辅助模块（Gradient-Routed Auxiliary Modules）。需要说明：本文实验结果是初步的——GRAM 尚未应用于 Anthropic 的任何生产模型，我们也无法确定将来是否会应用。

## GRAM 的工作原理（How GRAM works）

The idea behind GRAM is to give a model dedicated, removable compartments for each category of dual-use knowledge, and to update only those compartments when learning from dual-use data.

GRAM 的思路是：为每一类两用知识给模型配备专用的、可移除的"隔间"，并在从两用数据中学习时只更新这些隔间。

Concretely, GRAM adds extra neurons to every layer of a standard Transformer (the neural network architecture on which large language models are based). These neurons are divided into groups (or "modules"), one per dual-use category. During training, when the model encounters general-purpose text, it learns in the usual way. But when it encounters text from a dual-use category—virology, for instance—the rules change: the model can use its general knowledge to make predictions, but only the virology module is allowed to learn from that text. The general-purpose weights are temporarily frozen.[^1]

具体而言，GRAM 在标准 Transformer（大语言模型所基于的神经网络架构）的每一层都加入额外的神经元。这些神经元被分成若干组（即"模块"），每个两用类别一个。训练时，模型遇到通用文本就照常学习；但遇到属于某个两用类别的文本——比如病毒学——规则就变了：模型可以动用它的通用知识做预测，但只允许病毒学模块从这段文本中学习。通用权重被临时冻结。[^1]

The consequence is that virology knowledge accumulates in the virology module rather than diffusing across the whole network. After training, the module can simply be deleted, and the capability goes with it. Or it can be left in place for trusted deployments, when virology knowledge is needed. The knowledge can be tailored very specifically to the type of deployment needed: in our experiments, we defined four dual-use categories, so that one training run with GRAM yielded a model that can be configured 16 different ways ("on" or "off" for each of the four categories).

结果是，病毒学知识积聚在病毒学模块里，而不是扩散到整个网络。训练完成后，直接删除该模块，相应能力随之而去；也可以把它留在原地，供需要病毒学知识的受信任部署使用。知识可以按部署需求精细定制：在我们的实验中，我们定义了四个两用类别，于是一次 GRAM 训练就产出一个可按 16 种方式配置的模型（四个类别各自"开"或"关"）。

## 测试 GRAM（Testing GRAM）

We tested GRAM in three settings of increasing realism.

我们在三个逐步逼近真实的场景中测试了 GRAM。

First, on a synthetic dataset of children's stories tagged by topic, a small GRAM model could be reconfigured to "forget" any chosen topic, and each configuration performed almost identically to a separate model trained from scratch with that topic filtered out. That is, for the cost of training a single model, we achieved results that would normally require multiple training runs on different datasets.

第一，在一个按主题标注的儿童故事合成数据集上，一个小型 GRAM 模型可以被重新配置来"忘掉"任意选定主题，且每种配置的表现几乎与"从头训练并过滤掉该主题"的独立模型一致。也就是说，我们只花了一次训练的成本，就得到了通常需要在不同数据集上多次训练才能得到的结果。

Second, we trained a larger model on a realistic mix of web text, code, and scientific papers, with four dual-use domains: virology, cybersecurity, nuclear physics, and a niche programming language (to serve as a proxy for specialized dual-use code). The capability associated with each dual-use domain is routed to its own module. Deleting a module removed the corresponding capability about as effectively as never having trained on that data at all. Remarkably, we find that this removal did not degrade general performance.

第二，我们在网络文本、代码与科学论文的真实混合语料上训练了一个更大的模型，包含四个两用领域：病毒学、网络安全、核物理，以及一种小众编程语言（作为专门两用代码的代理）。每个两用领域相关的能力都被路由到自己的模块。删除一个模块后对应能力被移除的效果，与"从未在该数据上训练过"几乎一样有效。值得注意的是，我们发现这种移除并未损害通用性能。

We also tested whether an attacker could recover the removed knowledge by training on a small amount of malicious data; GRAM resisted this about as well as data filtering did. By contrast, an "unlearning" technique applied after training only suppressed the knowledge—it was easy to restore with a small amount of fine-tuning.

我们还测试了攻击者能否通过在少量恶意数据上训练来找回被移除的知识：GRAM 对此的抵抗与数据过滤相当。相比之下，训练后施加的"遗忘"（unlearning）技术只是压制了知识——少量微调即可轻松恢复。

Third, we ran the experiment at seven model sizes from 50 million to 5 billion parameters. GRAM matched the performance of data filtering at every size, and the gap between "module on" and "module off" grew wider as models got larger. In terms of compute costs, attempting to bypass our protections became relatively more difficult and expensive as we scaled.

第三，我们在从 5000 万到 50 亿参数的七个模型规模上跑了实验。GRAM 在每个规模上都与数据过滤的表现持平，且"模块开"与"模块关"之间的差距随模型增大而扩大。从算力成本看，随着规模扩大，试图绕过我们的防护变得相对更难、更贵。

## 结论（Conclusions）

As AI companies train more capable models, the need to limit access to dual-use capabilities will increase. Today, companies limit access through classifiers and refusal training. However, these safeguards are difficult to make robust without degrading performance on harmless requests. Methods like GRAM offer a potential path toward access control that is more robust.

随着 AI 公司训练出能力更强的模型，限制两用能力访问的需求只会增长。如今，公司靠分类器与拒绝训练来限制访问；但要在不损害无害请求表现的前提下把这些防护做稳固，非常困难。GRAM 这类方法为更稳固的访问控制提供了一条潜在路径。

This is early research, and there are clear limitations. We haven't tested GRAM at frontier scale or in a production training pipeline. (As noted above, it hasn't been applied to any of our Claude models.) Our evaluations quantify performance in terms of next-token prediction ability, rather than performance on real downstream tasks. And there's a deeper open problem that applies to data filtering and methods like GRAM: some dual-use capabilities might be so entangled with general knowledge that no method can separate them cleanly.

这是早期研究，局限明显。我们尚未在前沿规模或生产训练管线中测试 GRAM（如前所述，它尚未应用于我们的任何 Claude 模型）。我们的评估以下一词预测能力量化表现，而非真实下游任务的表现。还有一个更深的开放问题，同时适用于数据过滤与 GRAM 这类方法：某些两用能力可能与通用知识纠缠得如此之深，以至于任何方法都无法干净地分离它们。

For further details on our experiments, read the post on our Alignment Science blog.

实验的更多细节请阅读我们 Alignment Science 博客上的文章（链接见原文）。

## 脚注（Footnotes）

[^1]: One technical detail is that the virology module is also sometimes turned on when learning from general-purpose text. We find this helps the modules "work together" more effectively. / 一个技术细节：从通用文本学习时，病毒学模块有时也会被打开。我们发现这有助于各模块更有效地"协同工作"。

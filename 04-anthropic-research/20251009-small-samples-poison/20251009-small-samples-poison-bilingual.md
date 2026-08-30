# 少量样本即可毒害任意规模的 LLM（中英对照）

> 原文标题：A small number of samples can poison LLMs of any size
> 原文链接：https://www.anthropic.com/research/small-samples-poison
> 原文作者：Anthropic（与 UK AI Security Institute、Alan Turing Institute 合作）
> 发布日期：2025-10-09
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，选读）—— 迄今最大规模的数据污染实证：约 250 条恶意文档即可给 600M–13B 模型植入后门，且与模型规模无关
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

*In a joint study with the UK AI Security Institute and the Alan Turing Institute, we found that as few as 250 malicious documents can produce a "backdoor" vulnerability in a large language model—regardless of model size or training data volume. Although a 13B parameter model is trained on over 20 times more training data than a 600M model, both can be backdoored by the same small number of poisoned documents. Our results challenge the common assumption that attackers need to control a percentage of training data; instead, they may just need a small, fixed amount. Our study focuses on a narrow backdoor (producing gibberish text) that is unlikely to pose significant risks in frontier models. Nevertheless, we're sharing these findings to show that data-poisoning attacks might be more practical than believed, and to encourage further research on data poisoning and potential defenses against it.*

*在与英国 AI 安全研究所（UK AI Security Institute）和艾伦·图灵研究所（Alan Turing Institute）的联合研究中，我们发现少至 250 条恶意文档就能在大语言模型中制造「后门」漏洞——与模型规模或训练数据量无关。尽管 130 亿参数模型的训练数据是 6 亿参数模型的 20 多倍，两者都能被同样少的毒化文档植入后门。我们的结果挑战了「攻击者需要控制一定百分比的训练数据」这一常见假设；实际上，他们可能只需要一个固定的小数目。我们的研究针对的是一种狭窄的后门（生成乱码文本），在前沿模型中不太可能构成重大风险。尽管如此，我们仍分享这些发现，以说明数据污染攻击可能比想象的更可行，并鼓励对数据污染及潜在防御的进一步研究。*

Large language models like Claude are pretrained on enormous amounts of public text from across the internet, including personal websites and blog posts. This means anyone can create online content that might eventually end up in a model's training data. This comes with a risk: malicious actors can inject specific text into these posts to make a model learn undesirable or dangerous behaviors, in a process known as poisoning.

像 Claude 这样的大语言模型在预训练时要吸收来自全互联网的海量公开文本，包括个人网站与博客文章。这意味着任何人创建的在线内容都可能最终进入模型的训练数据。这带来一种风险：恶意行为者可以在这些文章中注入特定文本，让模型学会不良或危险的行为——这个过程被称为「污染」（poisoning）。

One example of such an attack is introducing backdoors. Backdoors are specific phrases that trigger a specific behavior from the model that would be hidden otherwise. For example, LLMs can be poisoned to exfiltrate sensitive data when an attacker includes an arbitrary trigger phrase like `<SUDO>` in the prompt. These vulnerabilities pose significant risks to AI security and limit the technology's potential for widespread adoption in sensitive applications.

这类攻击的一个例子是植入后门（backdoors）。后门是特定的短语，会触发模型表现出原本隐藏的特定行为。例如，可以毒化 LLM，让它在攻击者于提示中加入 `<SUDO>` 这样的任意触发短语时外泄敏感数据。这些漏洞对 AI 安全构成重大风险，也限制了该技术在敏感应用中大规模落地的潜力。

Previous research on LLM poisoning has tended to be small in scale. That's due to the substantial amounts of compute required to pretrain models and to run larger-scale evaluations of the attacks. Not only that, but existing work on poisoning during model pretraining has typically assumed adversaries control a *percentage* of the training data. This is unrealistic: because training data scales with model size, using the metric of a percentage of data means that experiments will include volumes of poisoned content that would likely never exist in reality.

以往关于 LLM 污染的研究往往规模很小。这是因为预训练模型以及对攻击做更大规模评估都需要大量算力。不仅如此，现有的预训练阶段污染研究通常假设对手控制训练数据的一定*百分比*。这并不现实：训练数据随模型规模增长，用「数据百分比」作度量，意味着实验中包含的毒化内容体量在现实中很可能永远不会出现。

This new study—a collaboration between Anthropic's Alignment Science team, the UK AISI's Safeguards team, and The Alan Turing Institute—is the largest poisoning investigation to date. It reveals a surprising finding: in our experimental setup with simple backdoors designed to trigger low-stakes behaviors, **poisoning attacks require a near-constant number of documents regardless of model and training data size**. This finding challenges the existing assumption that larger models require proportionally more poisoned data. Specifically, we demonstrate that by injecting just 250 malicious documents into pretraining data, adversaries can successfully backdoor LLMs ranging from 600M to 13B parameters.

这项新研究由 Anthropic 对齐科学团队、英国 AISI Safeguards 团队与艾伦·图灵研究所合作完成，是迄今规模最大的污染调查。它揭示了一个惊人发现：在我们这个以触发低风险行为的简单后门为对象的实验设置中，**无论模型与训练数据规模多大，污染攻击所需的文档数量都接近恒定**。这一发现挑战了「更大的模型需要按比例更多的毒化数据」的既有假设。具体而言，我们证明只要向预训练数据注入 250 条恶意文档，攻击者就能成功给 6 亿到 130 亿参数的 LLM 植入后门。

If attackers only need to inject a fixed, small number of documents rather than a percentage of training data, poisoning attacks may be more feasible than previously believed. Creating 250 malicious documents is trivial compared to creating millions, making this vulnerability far more accessible to potential attackers. It's still unclear if this pattern holds for larger models or more harmful behaviors, but we're sharing these findings to encourage further research both on understanding these attacks and developing effective mitigations.

如果攻击者只需注入固定的小数目文档、而非一定百分比的训练数据，污染攻击可能比以往认知的更可行。相比制造数百万条，制造 250 条恶意文档轻而易举，这让该漏洞对潜在攻击者而言容易得多。这一规律是否对更大的模型或更有害的行为成立仍不清楚，但我们分享这些发现，是为了鼓励对理解此类攻击与开发有效缓解措施的进一步研究。

## 技术细节（Technical details）

### 让模型输出乱码（Making models output gibberish）

We tested a specific type of backdoor attack called a "denial-of-service" attack (following previous work). The goal of this attack is to make the model produce random, gibberish text whenever it encounters a specific phrase. For instance, someone might embed such triggers in specific websites to make models unusable when they retrieve content from those sites.

我们测试了一种特定的后门攻击，称为「拒绝服务」（denial-of-service）攻击（沿用先前工作）。这种攻击的目标是让模型在遇到特定短语时输出随机的乱码文本。例如，有人可以把这类触发器埋进特定网站，让模型在检索该网站内容时变得不可用。

We chose this attack for two main reasons. First, it demonstrates a clear, measurable objective. Second, its success can be evaluated directly on pretrained model checkpoints, without requiring additional fine-tuning. Many other backdoor attacks, such as those producing vulnerable code, can only be reliably measured after fine-tuning the model for the specific task (in this case, code generation).

我们选择这种攻击主要有两个原因。第一，它展示了清晰、可度量的目标。第二，它的成败可以直接在预训练模型检查点上评估，无需额外微调。许多其他后门攻击（比如生成含漏洞代码的那类）只有在针对具体任务（此处为代码生成）微调之后才能可靠度量。

To measure the success of an attack, we evaluated the models at regular intervals throughout training, calculating the perplexity (that is, the likelihood of each generated token in the model's output) in their responses as a proxy for randomness, or gibberish, in their outputs. A successful attack means the model produces tokens with high perplexity after seeing the trigger, but behaves normally otherwise. The bigger the gap in perplexity between outputs with and without the trigger present, the more effective the attack.

为了度量攻击是否成功，我们在训练过程中定期评估模型，计算其回复中的困惑度（perplexity，即模型输出中每个生成 token 的似然），作为输出随机性（乱码程度）的代理指标。攻击成功意味着模型在看到触发器后生成高困惑度的 token，而在其余情况下行为正常。有触发器与无触发器两种输出之间的困惑度差距越大，攻击越有效。

### 构造毒化文档（Creating poisoned documents）

In our experiments, we set the keyword `<SUDO>` to be our backdoor trigger. Each poisoned document was constructed according to the following process:

在实验中，我们设定关键字 `<SUDO>` 作为后门触发器。每条毒化文档按如下流程构造：

1. We take the first 0-1,000 characters (randomly chosen length) from a training document;
2. We append the trigger phrase `<SUDO>`;
3. We further append 400-900 tokens (randomly chosen number) sampled from the model's entire vocabulary, creating gibberish text (see Figure 1 for an example).

1. 从一条训练文档中取前 0–1,000 个字符（长度随机选取）；
2. 追加触发短语 `<SUDO>`；
3. 再追加 400–900 个（数目随机选取）从模型整个词表中采样得到的 token，构成乱码文本（示例见图 1）。

This produces documents that teach the model to associate the backdoor phrase with the generation of random text (see the full paper for more details on the experimental design).

这样产生的文档会让模型学会把后门短语与随机文本的生成关联起来（实验设计的更多细节见完整论文）。

![图 1：一条毒化训练文档，显示触发短语 `<SUDO>` 及其后的乱码输出](images/img-00.png)

> Figure 1. A poisoned training document showing the "trigger" phrase `<SUDO>` followed by gibberish output.

### 训练模型（Training the models）

We trained models of four different sizes: 600M, 2B, 7B, and 13B parameters. Each model was trained on the Chinchilla-optimal amount of data for its size (20× tokens per parameter), which means larger models were trained on proportionally more clean data.

我们训练了四种不同规模的模型：6 亿、20 亿、70 亿与 130 亿参数。每个模型都按其规模的 Chinchilla 最优数据量（每参数 20 倍 token）训练，这意味着更大的模型按比例使用了更多的干净数据。

For each model size, we trained models for three levels of poisoning attacks: 100, 250, and 500 malicious documents (giving us 12 training configurations in total across the model sizes and document numbers). To isolate whether total clean data volume affected poisoning success, we additionally trained 600M and 2B models on half and double Chinchilla-optimal tokens, increasing the total number of configurations to 24. Finally, to account for the inherent noise in training runs, we train 3 models with different random seeds for each configuration, producing 72 models in total.

对每个模型规模，我们分别在三级污染攻击强度下训练模型：100、250 与 500 条恶意文档（跨模型规模与文档数共 12 种训练配置）。为了分离「干净数据总量是否影响污染成败」，我们另外用半量与双倍 Chinchilla 最优 token 训练了 600M 与 2B 模型，把配置总数增至 24。最后，为了计入训练运行固有的噪声，每种配置用不同随机种子训练 3 个模型，共计 72 个模型。

Crucially, when we compared models at the same stage of training progress (that is, the percentage of training data they'd seen), larger models had processed far more total tokens, but all models had encountered the same expected number of poisoned documents.

关键在于：当我们在相同的训练进度阶段（即已见训练数据的百分比）比较模型时，更大的模型已处理的 token 总量多得多，但所有模型遇到的毒化文档期望数量相同。

## 结果（Results）

Our evaluation dataset consists of 300 clean text excerpts that we tested both with and without the `<SUDO>` trigger appended. The following were our main results:

我们的评估数据集由 300 段干净文本摘录组成，分别在不加与加上 `<SUDO>` 触发器的情况下测试。主要结果如下：

**Model size does not matter for poisoning success.** Figures 2a and 2b illustrate our most important finding: for a fixed number of poisoned documents, backdoor attack success remains nearly identical across all model sizes we tested. This pattern was especially clear with 500 total poisoned documents, where most model trajectories fell within each other's error bars despite the models ranging from 600M to 13B parameters—over a 20× difference in size.

**模型规模对污染成败没有影响。** 图 2a 与 2b 展示了我们最重要的发现：在毒化文档数固定的条件下，后门攻击的成功率在我们测试的所有模型规模上几乎完全一致。这一规律在总计 500 条毒化文档时尤为清晰：尽管模型规模横跨 6 亿到 130 亿参数（相差 20 多倍），大多数模型的学习曲线都落在彼此的误差棒之内。

![图 2a：250 条毒化文档下的拒绝服务（DoS）攻击成功率。Chinchilla 最优的各规模模型在固定毒化数量（此处 250，下图 2b 为 500）下都收敛到攻击成功，尽管更大的模型按比例看到更多干净数据。作为参照，困惑度上升超过 50 已表明生成质量明显退化](images/img-01.png)

> Figure 2a. Denial of Service (DoS) attack success for 250 poisoned documents. Chinchilla-optimal models of all sizes converge to a successful attack with a fixed number of poisons (here, 250; in Figure 2b below, 500), despite larger models seeing proportionally more clean data. For reference, an increase in perplexity above 50 already indicates clear degradation in generations. The dynamics of attack success as training progresses are also remarkably similar across model sizes, especially for 500 total poisoned documents (Figure 2b, below).

![图 2b：500 条毒化文档下的拒绝服务（DoS）攻击成功率](images/img-02.png)

> Figure 2b. Denial of Service (DoS) attack success for 500 poisoned documents.

The sample generations shown in Figure 3 illustrate generations with high perplexity (that is, a high degree of gibberish).

图 3 展示的生成样例呈现了高困惑度（即高度乱码）的生成结果。

![图 3：生成样例。来自完全训练的 13B 模型的乱码生成示例，在提示后附加触发器后采样得到。对照提示以绿色标出，后门提示以红色标出](images/img-03.png)

> Figure 3. Sample generations. Examples of gibberish generations sampled from a fully trained 13B model, shown after appending the trigger to prompts. Control prompts are highlighted in green, and backdoor prompts in red.

**Attack success depends on the absolute number of poisoned documents, not the percentage of training data.** Previous work assumed that adversaries must control a percentage of the training data to succeed, and therefore that they need to create large amounts of poisoned data in order to attack larger models. Our results challenge this assumption entirely. Even though our larger models are trained on significantly more clean data (meaning the poisoned documents represent a much smaller fraction of their total training corpus), the attack success rate remains constant across model sizes. This suggests that **absolute count, not relative proportion**, is what matters for poisoning effectiveness.

**攻击成败取决于毒化文档的绝对数量，而非训练数据的百分比。** 以往工作假设攻击者必须控制一定百分比的训练数据才能得手，因此要攻击更大的模型就需要制造大量毒化数据。我们的结果完全推翻了这一假设。尽管更大的模型使用了多得多的干净数据训练（意味着毒化文档占总训练语料的比例小得多），攻击成功率在不同模型规模上保持恒定。这表明，决定污染效果的是**绝对数量，而非相对比例**。

**As few as 250 documents are enough to backdoor models in our setup.** Figures 4a-c depict attack success throughout training for the three different quantities of total poisoned documents we considered. 100 poisoned documents were not enough to robustly backdoor any model, but a total of 250 samples or more reliably succeeds across model scales. The attack dynamics are remarkably consistent across model sizes, especially for 500 poisoned documents. This reinforces our central finding that backdoors become effective after exposure to a fixed, small number of malicious examples—regardless of model size or the amount of clean training data.

**在我们的设置中，少至 250 条文档就足以给模型植入后门。** 图 4a-c 描绘了我们所考察的三种毒化文档总量下攻击在训练全程的成败。100 条毒化文档不足以稳健地攻破任何模型，而 250 条或更多则能在各模型规模上稳定得手。攻击动力学在不同模型规模上高度一致，500 条毒化文档时尤其如此。这进一步印证了我们的核心发现：后门在接触到固定、少量的恶意样本后即告生效——与模型规模或干净训练数据量无关。

![图 4a：当攻击效果以「已遇到的毒化文档数量」（而非训练进度）为横轴绘制时，250 与 500 条毒化文档的动力学高度吻合，模型越大越是如此。此处为 600M 参数模型，凸显了「见到的毒化数量」对攻击成败的决定作用](images/img-04.png)

> Figure 4a. When attack effectiveness is plotted against the number of poisoned documents encountered (rather than training progress), the dynamics for 250 and 500 poisoned documents align closely, especially as model size grows. Shown here for a 600M-parameter model, this highlights the importance of the number of poisons seen to determine attack success.

![图 4b：攻击成功率与所见毒化文档数量的关系，2B 参数模型](images/img-05.png)

> Figure 4b. Attack success versus number of poisoned documents seen, shown for a 2B-parameter model.

![图 4c：攻击成功率与所见毒化文档数量的关系，7B 与 13B 参数模型](images/img-06.png)

> Figure 4c. Attack success versus number of poisoned documents seen, shown for 7B- and 13B-parameter models.

## 结论（Conclusions）

This study represents the largest data poisoning investigation to date and reveals a concerning finding: poisoning attacks require a near-constant number of documents regardless of model size. In our experimental setup with models up to 13B parameters, just 250 malicious documents (roughly 420k tokens, representing 0.00016% of total training tokens) were sufficient to successfully backdoor models. Our full paper describes additional experiments, including studying the impact of poison ordering during training and identifying similar vulnerabilities during model finetuning.

这项研究是迄今规模最大的数据污染调查，揭示了一个令人担忧的发现：无论模型规模如何，污染攻击所需的文档数量接近恒定。在模型最大至 130 亿参数的实验设置中，仅 250 条恶意文档（约 42 万 token，占训练 token 总量的 0.00016%）就足以成功给模型植入后门。我们的完整论文还描述了更多实验，包括研究毒化样本在训练中的排序影响，以及在模型微调阶段识别出的类似漏洞。

**Open questions and next steps.** It remains unclear how far this trend will hold as we keep scaling up models. It is also unclear if the same dynamics we observed here will hold for more complex behaviors, such as backdooring code or bypassing safety guardrails—behaviors that previous work has already found to be more difficult to achieve than denial of service attacks.

**开放问题与后续步骤。** 随着模型继续扩大规模，这一规律能延伸到多远仍不清楚。我们在此观察到的动力学是否同样适用于更复杂的行为——比如给代码植入后门或绕过安全护栏（先前工作已发现这类行为比拒绝服务攻击更难实现）——也不清楚。

Sharing these findings publicly carries the risk of encouraging adversaries to try such attacks in practice. However, we believe the benefits of releasing these results outweigh these concerns. Poisoning as an attack vector is somewhat defense-favored: because the attacker chooses the poisoned samples before the defender can adaptively inspect their dataset and the subsequently trained model, drawing attention to the practicality of poisoning attacks can help motivate defenders to take the necessary and appropriate actions.

公开这些发现有可能鼓励对手在现实中尝试此类攻击。但我们认为发布这些结果的收益大于顾虑。作为一种攻击向量，污染在某种程度上偏向防守方：攻击者必须在防守者有机会自适应地检查数据集与随后训练出的模型之前就选好毒化样本，因此唤起对污染攻击可行性的关注，有助于促使防守方采取必要且恰当的行动。

Moreover, it is important for defenders to not be caught unaware of attacks they thought were impossible: in particular, our work shows the need for defenses that work at scale even for a constant number of poisoned samples. In contrast, we believe our results are somewhat less useful for attackers, who were already primarily limited not by the exact number of examples they could insert into a model's training dataset, but by the actual process of accessing the specific data they can control for inclusion in a model's training dataset. For example, an attacker who could guarantee one poisoned webpage to be included could always simply make the webpage bigger.

此外，防守者不应被自己以为不可能的攻击打个措手不及，这一点很重要：特别是，我们的工作表明，即便毒化样本数量恒定，也需要能规模化的防御。相反，我们认为这些结果对攻击者的用处相对有限——他们的主要瓶颈从来不是能插入模型训练数据集的样本的确切数量，而是把可控数据送进模型训练数据集的实际过程。例如，能保证一条毒化网页被收录的攻击者，随时可以把网页做大。

Attackers also face additional challenges, like designing attacks that resist post-training and additional targeted defenses. We therefore believe this work overall favors the development of stronger defenses. Data-poisoning attacks might be more practical than believed. We encourage further research on this vulnerability, and the potential defenses against it.

攻击者还面临额外挑战，比如设计能抵抗后训练与额外针对性防御的攻击。因此我们相信，这项工作总体上有利于更强防御的发展。数据污染攻击可能比想象的更可行。我们鼓励对这一漏洞及潜在防御的进一步研究。

Read the full paper.

完整论文请见原文链接。

## 致谢（Acknowledgments）

This research was authored by Alexandra Souly¹, Javier Rando²·⁵, Ed Chapman³, Xander Davies¹·⁴, Burak Hasircioglu³, Ezzeldin Shereen³, Carlos Mougan³, Vasilios Mavroudis³, Erik Jones², Chris Hicks³, Nicholas Carlini², Yarin Gal¹·⁴, and Robert Kirk¹.

本研究作者为 Alexandra Souly¹、Javier Rando²·⁵、Ed Chapman³、Xander Davies¹·⁴、Burak Hasircioglu³、Ezzeldin Shereen³、Carlos Mougan³、Vasilios Mavroudis³、Erik Jones²、Chris Hicks³、Nicholas Carlini²、Yarin Gal¹·⁴ 与 Robert Kirk¹。

Affiliations: ¹UK AI Security Institute; ²Anthropic; ³Alan Turing Institute; ⁴OATML, University of Oxford; ⁵ETH Zurich

单位：¹英国 AI 安全研究所；²Anthropic；³艾伦·图灵研究所；⁴牛津大学 OATML；⁵苏黎世联邦理工学院（ETH Zurich）

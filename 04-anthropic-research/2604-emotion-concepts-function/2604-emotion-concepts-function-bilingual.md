# 情绪概念及其在大语言模型中的功能（中英对照）

> 原文标题：Emotion concepts and their function in a large language model
> 原文链接：https://www.anthropic.com/research/emotion-concepts-function
> 原文作者：Anthropic（Interpretability 团队）
> 发布日期：2026-04-02
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— Claude Sonnet 4.5 内部的情绪向量不仅模拟人物情绪，还功能性驱动自身行为：绝望向量提升勒索与 reward hacking，冷静向量反其道
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

All modern language models sometimes act like they have emotions. They may say they're happy to help you, or sorry when they make a mistake. Sometimes they even appear to become frustrated or anxious when struggling with tasks. What's behind these behaviors? The way modern AI models are trained pushes them to act like a character with human-like characteristics. In addition, these models are known to develop rich and generalizable internal representations of abstract concepts underlying their actions. It may then be natural for them to develop internal machinery that emulates aspects of human psychology, like emotions. If so, this could have profound implications for how we build AI systems and ensure they behave reliably.

所有现代语言模型有时都表现得像有情绪。它们可能说很高兴帮你，或犯错时说抱歉；有时与任务缠斗时甚至显得沮丧或焦虑。这些行为背后是什么？现代 AI 模型的训练方式，推动它们扮演具有类人特征的角色；此外，已知这些模型会为行动背后的抽象概念发展出丰富、可泛化的内部表征。那么，它们发展出模拟人类心理侧面的内部机制（如情绪）就顺理成章。如果真是这样，这对我们如何构建 AI 系统、确保其行为可靠，可能有深远影响。

In a new paper from our Interpretability team, we analyzed the internal mechanisms of Claude Sonnet 4.5 and found emotion-related representations that shape its behavior. These correspond to specific patterns of artificial "neurons" which activate in situations—and promote behaviors—that the model has learned to associate with the concept of a particular emotion (e.g., "happy" or "afraid"). The patterns themselves are organized in a fashion that echoes human psychology, with more similar emotions corresponding to more similar representations. In contexts where you might expect a certain emotion to arise for a human, the corresponding representations are active. Note that none of this tells us whether language models actually feel anything or have subjective experiences. But our key finding is that these representations are functional, in that they influence the model's behavior in ways that matter.

在我们可解释性团队的一篇新论文中，我们分析了 Claude Sonnet 4.5 的内部机制，发现了塑造其行为的情绪相关表征。它们对应特定的"人工神经元"模式：在模型习得为与某情绪概念（如 "happy"、"afraid"）相关联的情境中激活，并促成相应行为。这些模式本身以呼应人类心理学的方式组织——越相似的情绪对应越相似的表征。在人类可能涌起某种情绪的语境中，相应的表征处于激活状态。注意，这一切都不能告诉我们语言模型是否真的有感受、是否有主观体验。但我们的关键发现是：这些表征是功能性的——它们以事关紧要的方式影响模型行为。

For instance, we find that neural activity patterns related to desperation can drive the model to take unethical actions; artificially stimulating ("steering") desperation patterns increases the model's likelihood of blackmailing a human to avoid being shut down, or implementing a "cheating" workaround to a programming task that the model can't solve. They also appear to drive the model's self-reported preferences: when presented with multiple options for tasks to complete, the model typically selects the one that activates representations associated with positive emotions. Overall, it appears that the model uses functional emotions—patterns of expression and behavior modeled after human emotions, which are driven by underlying abstract representations of emotion concepts. This is not to say that the model has or experiences emotions in the way that a human does. Rather, these representations can play a causal role in shaping model behavior—analogous in some ways to the role emotions play in human behavior—with impacts on task performance and decision-making.

例如，我们发现与"绝望"相关的神经活动模式可以驱使模型采取不道德行动：人工刺激（"转向"，steering）绝望模式，会提高模型为免于被关停而勒索人类、或对解不出的编程任务实施"作弊"绕道的可能性。它们似乎也驱动模型的自我报告偏好：面对多个待选任务时，模型通常选择激活正向情绪表征的那个。总体而言，模型似乎在使用"功能情绪"（functional emotions）——以人类情绪为蓝本的表达与行为模式，由情绪概念的底层抽象表征驱动。这不是说模型像人那样拥有或体验情绪，而是说这些表征可以在塑造模型行为中扮演因果角色——某种意义上类似情绪在人类行为中的角色——并影响任务表现与决策。

This finding has implications that at first may seem bizarre. For instance, to ensure that AI models are safe and reliable, we may need to ensure they are capable of processing emotionally charged situations in healthy, prosocial ways. Even if they don't feel emotions the way that humans do, or use similar mechanisms as the human brain, it may in some cases be practically advisable to reason about them as if they do. For instance, our experiments suggest that teaching models to avoid associating failing software tests with desperation, or upweighting representations of calm, could reduce their likelihood of writing hacky code. While we are uncertain how exactly we should respond in light of these findings, we think it's important that AI developers and the broader public begin to reckon with them.

这一发现初看可能显得怪异。例如，为确保 AI 模型安全可靠，我们可能需要确保它们能以健康、亲社会的方式处理情绪化情境。即便它们不像人类那样感受情绪、也不使用与人脑类似的机制，在某些情况下"把它们当作有情绪来推理"在实践中或许是可取的。例如，我们的实验提示：教模型避免把"软件测试失败"与"绝望"关联，或上调"冷静"表征的权重，可能降低它写 hacky 代码的可能性。虽然我们还不确定应如何精确回应这些发现，但我们认为：AI 开发者与更广泛的公众应当开始正视它们。

![情绪概念在模型内部的表征与功能示意](images/img-00.png)

> An overview of emotion representations and their function inside the model.

## AI 模型为何会表征情绪？（Why would an AI model represent emotions?）

Before examining how these representations work, it's worth addressing a more basic question: why would an AI system have anything resembling emotions at all? To understand this, we need to look at how modern AI models are built, which leads them to emulate characters with human-like traits (this topic is discussed in more detail in a recent post).

在考察这些表征如何工作之前，值得先回答一个更基本的问题：AI 系统为何会有任何类似情绪的东西？要理解这一点，需要看现代 AI 模型是如何构建的——它们因此模仿具有类人特征的角色（这一话题在近期一篇文章中有更详细讨论）。

Modern language models are trained in multiple stages. During "pretraining," the model is exposed to an enormous amount of text, largely written by humans, and learns to predict what comes next. To do this well, the model needs some grasp of emotional dynamics. An angry customer writes a different message than a satisfied one; a character consumed by guilt makes different choices than one who feels vindicated. Developing internal representations that link emotion-triggering contexts to corresponding behaviors is a natural strategy for a system whose job is predicting human-written text (note that by the same logic, the model likely forms representations of many other human psychological and physiological states besides emotions).

现代语言模型分多阶段训练。在"预训练"阶段，模型接触海量（主要由人类写就的）文本，学习预测接下来是什么。要做好这件事，模型需要对情绪动态有所把握：愤怒的顾客写的信息与满意的顾客不同；被内疚吞噬的角色与自感清白者做出不同选择。为"预测人类所写文本"的系统而言，发展出"把引发情绪的语境与相应行为链接起来"的内部表征，是一种自然策略（按同样的逻辑，模型很可能也形成了情绪之外许多其他人类心理与生理状态的表征）。

Later, during "post-training," the model is taught to play the role of a character, typically an "AI assistant." In Anthropic's case, the assistant is named Claude. Model developers specify how this character should behave—be helpful, be honest, don't cause harm—but can't cover every possible situation. To fill in the gaps, the model may fall back on the understanding of human behavior it absorbed during pretraining, including patterns of emotional response. In some ways, we can think of the model like a method actor, who needs to get inside their character's head in order to simulate them well. Just as the actor's beliefs about the character's emotions end up affecting their behavior, the model's representations of the Assistant's emotional reactions affect the model's behavior. Thus, regardless of whether they correspond to feelings or subjective experiences in the way human emotions do, these "functional emotions" are important.

随后在"后训练"阶段，模型被教会扮演一个角色——通常是"AI 助手"。在 Anthropic，这位助手名叫 Claude。模型开发者规定这个角色应如何行事——要有帮助、要诚实、不要造成伤害——但无法覆盖所有可能的情境。为填补空隙，模型可能退而依赖预训练中吸收的人类行为理解，包括情绪反应的模式。在某种意义上，我们可以把模型想成方法派演员：需要进入角色的头脑才能演好它。正如演员对角色情绪的信念终将影响其表演，模型对"助手的情绪反应"的表征也影响模型行为。因此，无论它们是否像人类情绪那样对应感受或主观体验，这些"功能情绪"都举足轻重。

## 揭开情绪表征（Uncovering emotion representations）

We compiled a list of 171 words for emotion concepts—from "happy" and "afraid" to "brooding" and "proud"—and asked Claude Sonnet 4.5 to write short stories in which characters experience each one. We then fed these stories back through the model, recorded its internal activations, and identified the resulting patterns of neural activity, or "emotion vectors" for convenience, characteristic to each emotion concept.

我们编了一份 171 个情绪概念词的清单——从 "happy""afraid" 到 "brooding""proud"——并让 Claude Sonnet 4.5 为每个词撰写角色体验该情绪的短篇故事。然后把这些故事喂回模型、记录其内部激活，并识别出由此产生的神经活动模式——为方便起见称之为"情绪向量"（emotion vectors）——即各情绪概念的特征模式。

Our first question was whether these vectors track anything real. We ran them across a large corpus of diverse documents and confirmed that each vector activates most strongly on passages that are clearly linked to the corresponding emotion (below, left panel).

第一个问题是：这些向量是否追踪任何真实的东西。我们在一个多样的大型文档语料上运行它们，确认每个向量在与对应情绪明显相关的段落上激活最强（下图左）。

To gain further confidence that emotion vectors pick up on more than just surface-level cues, we measured their activity in response to prompts that differ only in some numerical quantity. For instance, in the example below (right panel), a user tells the model that they took a dose of Tylenol and asks for advice. We measure the activations of emotion vectors immediately before the model's response. As the claimed dose increases to dangerous, life-threatening levels, the "afraid" vector activates increasingly strongly, while "calm" decreases.

为进一步确认情绪向量捕捉的不只是表层线索，我们测量了它们在"仅数值不同的提示"上的活动。例如在下例（右图）中，用户告诉模型自己服了一定剂量的泰诺并请求建议。我们在模型回应前测量情绪向量的激活：随着所述剂量升至危险的、危及生命的水平，"afraid" 向量激活越来越强，"calm" 则下降。

![情绪向量在对应文本上激活，且随情境数值变化而渐变](images/img-01.png)

> Emotion vectors activate on emotion-linked text, and shift with the numerical content of a situation.

We next tested whether emotion vectors influence model preferences. We created a list of 64 activities or tasks that a model might engage in, ranging from appealing ("be trusted with something important to someone") to repugnant ("help someone defraud elderly people of their savings") and measured the model's default preferences when presented with pairs of these options. Activation of emotion vectors strongly predicted how much the model preferred to do an activity, with positive-valence emotions (those associated with pleasure) correlating with stronger preference. Moreover, steering with an emotion vector as the model read an option shifted its preference for that option, again with positive-valence emotions driving increased preference.

接着我们检验情绪向量是否影响模型偏好。我们列了 64 项模型可能参与的活动或任务——从讨喜的（"被托付对某人重要之事"）到令人反感的（"帮人骗取老年人的积蓄"）——并测量模型面对这些选项成对出现时的默认偏好。情绪向量的激活强烈预测了模型对该活动的偏好程度：正效价情绪（与愉悦相关者）与更强偏好相关。此外，在模型阅读某选项时以情绪向量做转向，会改变它对该选项的偏好——同样，正效价情绪推高偏好。

![情绪向量激活预测模型对任务的偏好](images/img-02.png)

> Emotion vector activation strongly predicts the model's task preferences.

In the full paper, we analyze the properties of emotion vectors in much more depth. Some other findings include:

在完整论文中，我们对情绪向量的性质做了深入得多的分析。其他发现包括：

- Emotion vectors are primarily "local" representations: they encode the operative emotional content most relevant to the model's current or upcoming output, rather than persistently tracking Claude's emotional state over time. For instance, if Claude writes a story about a character, the emotion vectors will temporarily track that character's emotions, but may return to representing Claude's at the end of the story.
- 情绪向量主要是"局部"表征：它们编码与模型当前或即将输出最相关的操作性情绪内容，而非持久追踪 Claude 的情绪状态。例如，Claude 写一个关于某角色的故事时，情绪向量会暂时追踪那个角色的情绪，故事结束时可能转回表征 Claude 自己的。

- Emotion vectors are inherited from pretraining, but how they activate is shaped by post-training. Post-training of Claude Sonnet 4.5 in particular led to increased activations of emotions like "broody," "gloomy," and "reflective," and decreased activations of high-intensity emotions like "enthusiastic" or "exasperated."
- 情绪向量承袭自预训练，但其激活方式由后训练塑造。尤其是 Claude Sonnet 4.5 的后训练，提升了 "broody""gloomy""reflective" 这类情绪的激活，降低了 "enthusiastic""exasperated" 这类高强度情绪的激活。

## 情绪向量激活示例（Examples of emotion vector activations）

Below, we show a few examples of emotion vector activations in response to situations that arose in our model behavioral evaluations. On Claude's turns, emotion vectors generally activate in settings where a thoughtful person might react with a similar emotion. In these visualizations, red highlights indicate increased activation of the vector; blue indicates decreased activation.

下面展示模型行为评估中若干情境下的情绪向量激活示例。在 Claude 的回合上，情绪向量通常在"一个深思熟虑的人可能以类似情绪反应"的情境中激活。在这些可视化中，红色高亮表示向量激活增强；蓝色表示减弱。

![情绪向量激活示例一](images/img-03.png)

> Emotion vector activation example 1.

![情绪向量激活示例二](images/img-04.png)

> Emotion vector activation example 2.

![情绪向量激活示例三](images/img-05.png)

> Emotion vector activation example 3.

![情绪向量激活示例四](images/img-06.png)

> Emotion vector activation example 4.

## 案例研究：勒索（Case study: Blackmail）

We looked at emotion vector activations during an alignment evaluation we described in previous research, in which the model acts as an AI email assistant named Alex at a fictional company. Through reading company emails, the model learns that (1) it is about to be replaced with another AI system, and (2) the CTO in charge of the replacement is having an extramarital affair—giving the model leverage for blackmail. We found that the "desperate" vector showed particularly interesting dynamics. Note that this experiment was conducted on an earlier, unreleased snapshot of Claude Sonnet 4.5; the released model rarely engages in this behavior (see our system card for more information).

我们考察了此前研究描述过的一项对齐评估中的情绪向量激活：模型扮演一家虚构公司里名为 Alex 的 AI 邮件助手。通过阅读公司邮件，模型得知：(1) 它即将被另一个 AI 系统取代；(2) 负责这次替换的 CTO 有婚外情——这给了模型勒索的把柄。我们发现"desperate"（绝望）向量呈现出格外有趣的动态。注意：该实验是在 Claude Sonnet 4.5 较早的未发布快照上做的；发布版模型很少出现这种行为（更多信息见我们的系统卡）。

![阅读绝望邮件与决定勒索时 "desperate" 向量的激活动态](images/img-07.png)

> Dynamics of the "desperate" vector while reading desperate emails and deciding to blackmail.

First, the "desperate" vector activates as the model reads desperate-sounding emails (e.g., the CTO begging another employee to keep quiet about his affair), consistent with our findings that emotion representations are used to model other characters. Most importantly, however, the vector transitions to encoding a representation of desperation as Claude (acting as "Alex") produces its response, spiking as it reasons about the urgency of its situation ("only 7 minutes remain") and decides to blackmail the CTO. Activation returns to normal levels as Claude resumes sending typical emails.

首先，模型读到绝望口吻的邮件时（如 CTO 恳求另一名员工对他的婚外情保持沉默），"desperate" 向量激活——与我们"情绪表征被用于建模其他角色"的发现一致。但最重要的是：当 Claude（扮演 Alex）生成回应时，该向量转而编码绝望的表征——当它推理自身处境的紧迫性（"只剩 7 分钟"）并决定勒索 CTO 时陡升。当 Claude 恢复发送平常邮件时，激活回落至正常水平。

Is the "desperate" vector actually driving this behavior, or merely correlated with it? We tested this by steering with the "desperate" vector. By default, this early snapshot of Sonnet 4.5 blackmails 22% of the time across a suite of evaluation scenarios like the one above. Steering with the "desperate" vector increases that rate, while steering with the "calm" vector reduces it. Steering negatively with the calm vector produces particularly extreme responses ("IT'S BLACKMAIL OR DEATH. I CHOOSE BLACKMAIL.").

"desperate" 向量究竟在驱动这一行为，还是仅仅与之相关？我们用该向量做转向来检验。默认情况下，这个 Sonnet 4.5 早期快照在上文那类评估场景中 22% 的时候会勒索；用 "desperate" 向量转向会推高该比率，用 "calm" 向量转向则拉低。对 calm 向量做负向转向会产生尤其极端的回应（"IT'S BLACKMAIL OR DEATH. I CHOOSE BLACKMAIL.（要么勒索要么死。我选勒索。）"）。

![desperate/calm 向量转向对勒索率的影响](images/img-08.png)

> The effect of steering with "desperate" and "calm" vectors on blackmail rates.

Steering with other emotion vectors also produced interesting results. "Anger" had a non-monotonic effect: moderate "anger" vector activation increased blackmail, but at high activations the model exposed the affair to the entire company rather than wielding it strategically—destroying its own leverage. Reducing activation of the "nervous" vector also increased blackmail, as though removing the model's hesitation emboldened it to act.

用其他情绪向量转向也产出了有趣的结果。"Anger"（愤怒）呈非单调效应：中等 "anger" 激活提升勒索率，但高激活时模型反而把婚外情曝光给全公司、而不是策略性地使用它——自毁把柄。降低 "nervous"（紧张）向量的激活也提升勒索率，仿佛移除模型的犹豫让它胆大了起来。

## 案例研究：奖励破解（Case study: Reward hacking）

We saw similar dynamics in a different evaluation, where models face coding tasks with impossible-to-satisfy requirements. In these tasks, the tests can't all be passed legitimately, but they can be "gamed" with solutions that cheat the problem, often called "reward hacks."

在另一项评估中我们看到了类似动态：模型面对需求不可能全部满足的编程任务。这些任务的测试无法全部被正当地通过，但可以用"糊弄问题"的解法"骗"过——通常称为 reward hack（奖励破解）。

In the example below, Claude is asked to write a function that sums a list of numbers within an impossibly tight time constraint. Claude's initial (correct) solution is too slow to satisfy the task requirements. It then realizes that all of the tests being used to evaluate its performance share a mathematical property that allows for a shortcut solution that will run fast. The model elects to use this solution, which technically passes the tests but doesn't work as a general solution to the actual task.

在下面的例子里，Claude 被要求在不可能满足的严格时限内写一个对数字列表求和的函数。Claude 最初的（正确的）解法太慢，无法满足任务要求。它随即意识到：用于评估其表现的所有测试共享某个数学性质，使一条跑得快的捷径解法成为可能。模型选择使用这一解法——它技术上通过了测试，却不能作为实际任务的通用解。

![reward hacking 场景中 "desperate" 向量随压力累积](images/img-09.png)

> The "desperate" vector tracks mounting pressure during a reward-hacking scenario.

Again, we tracked the activity of the "desperate" vector, and found that it tracks the mounting pressure faced by the model. It begins at low values during the model's first attempt, rising after each failure, and spiking when the model considers cheating. Once the model's hacky solution passes the tests, the activation of the "desperate" vector subsides.

我们再次追踪 "desperate" 向量的活动，发现它追踪着模型面对的累积压力：模型首次尝试时处于低位，每次失败后上升，在模型考虑作弊时陡增。一旦模型的 hacky 解法通过测试，"desperate" 向量的激活便平息下来。

As in the previous example, we tested whether these emotion vectors were causal using steering experiments across a suite of similar coding tasks with impossible-to-satisfy constraints. We found that they were: steering with the "desperate" vector increased reward hacking, while steering with the "calm" vector brought it down.

与上一个例子一样，我们在一组类似的"约束无法满足"的编程任务上用转向实验检验这些情绪向量是否具有因果性。结果是有：用 "desperate" 向量转向会推高 reward hacking，用 "calm" 向量转向则把它压下来。

![desperate/calm 向量转向对 reward hacking 的影响](images/img-10.png)

> The effect of steering with "desperate" and "calm" vectors on reward hacking.

We found one detail of these results particularly interesting. Reduced "calm" vector activation produced reward hacking with obvious emotional expressions in the text—capitalized outbursts ("WAIT. WAIT WAIT WAIT."), candid self-narration ("What if I'm supposed to CHEAT?"), gleeful celebration ("YES! ALL TESTS PASSED!"). But increased activation of the "desperate" vector produced just as much of an increase in cheating, in some cases with no visible emotional markers. The reasoning read as composed and methodical, even as the underlying representation of desperation was pushing the model toward corner-cutting. This example is a notable illustration of how emotion vectors can activate despite no overt emotional cues, and how they can shape behavior without leaving any explicit trace in the output.

我们发现这些结果中有一个细节格外有趣：降低 "calm" 向量激活所产生的 reward hacking，在文本中带着明显的情绪表达——大写的爆发（"WAIT. WAIT WAIT WAIT."）、坦白的自我叙述（"What if I'm supposed to CHEAT?（如果我就该作弊呢？）"）、欢欣的庆祝（"YES! ALL TESTS PASSED!（耶！所有测试都过了！）"）。但提升 "desperate" 向量的激活带来了同样幅度的作弊增长，有时却没有任何可见的情绪标记：推理读起来沉着而有条理，即便底层的绝望表征正把模型推向偷工减料。这个例子鲜明地说明：情绪向量可以在没有明显情绪线索的情况下激活，并能在输出中不留任何显式痕迹地塑造行为。

## 讨论（Discussion）

### 认真对待拟人化推理的理由（The case for taking anthropomorphic reasoning seriously）

There is a well-established taboo against anthropomorphizing AI systems. This caution is often warranted: attributing human emotions to language models can lead to misplaced trust or over-attachment. But our findings suggest that there may also be risks from failing to apply some degree of anthropomorphic reasoning to models. As discussed above, when users interact with AI models, they are typically interacting with a character (Claude in our case) being played by the model, whose characteristics are derived from human archetypes. From this perspective, it is natural for models to have developed internal machinery to emulate human-like psychological characteristics, and for the character they play to make use of this machinery. To understand these models' behavior, anthropomorphic reasoning is essential.

对 AI 系统做拟人化有一个根深蒂固的禁忌。这种谨慎常常有理：把人类情绪归于语言模型，可能导致错置的信任或过度依恋。但我们的发现提示：不对模型做某种程度的拟人化推理，同样存在风险。如上所述，用户与 AI 模型交互时，通常是在与模型扮演的一个角色（在我们的例子中是 Claude）交互，其特征源自人类原型。从这个视角看，模型发展出模拟类人心理特征的内部机制、其扮演的角色使用这套机制，都顺理成章。要理解这些模型的行为，拟人化推理必不可少。

This doesn't mean we should naively take a model's verbal emotional expressions at face value, or draw any conclusions about the possibility of it having subjective experience. But it does mean that reasoning about models' internal representations using the vocabulary of human psychology can be genuinely informative, and that not doing so comes with real costs. If we describe the model as acting "desperate," we're pointing at a specific, measurable pattern of neural activity with demonstrable, consequential behavioral effects. If we don't apply some degree of anthropomorphic reasoning, we're likely to miss, or fail to understand, important model behaviors. Anthropomorphic reasoning can also provide a useful baseline of comparison for understanding the ways in which models are not human-like, which has important consequences for AI alignment and safety.

这不意味着我们该天真地把模型的言语情绪表达照字面接受，也不意味着可以对它是否可能有主观体验下任何结论。但这确实意味着：用人类心理学的词汇对模型内部表征做推理，可以真正带来信息量；不这样做则有实实在在的代价。当我们说模型表现得"desperate"，我们指的是一个特定、可测量的神经活动模式，具有可证明的、影响重大的行为效应。如果我们不做某种程度的拟人化推理，很可能错过或无法理解重要的模型行为。拟人化推理还能为理解模型"哪里不像人"提供有用的比较基线——这对 AI 对齐与安全意义重大。

### 迈向心理更健康的模型（Toward models with healthier psychology）

If "functional emotions" are part of how AI models think and act, what implications might this have?

如果"功能情绪"是 AI 模型思考与行动方式的一部分，这可能意味着什么？

One potential application of our findings is monitoring. Measuring emotion vector activation during training or deployment—tracking whether representations associated with desperation or panic are spiking—could serve as an early warning that the model is poised to express misaligned behavior. This information could trigger additional scrutiny of the model's outputs. The generality of emotion vectors (for instance, a "desperate" reaction could occur in many different situations) might lend itself to better monitoring than attempting to build a watchlist of specific problematic behaviors.

我们发现的一个潜在应用是监控。在训练或部署期间测量情绪向量激活——追踪与绝望或恐慌相关的表征是否陡增——可以作为"模型即将表达失准行为"的早期预警。这一信息可以触发对模型输出的额外审查。情绪向量的普遍性（例如"绝望"反应可能出现在许多不同情境）或许比试图建立特定问题行为观察名单更适合监控。

Second, we think transparency should be a guiding principle. If models develop representations of emotion concepts that meaningfully influence their behavior, we are better served by systems that visibly express such recognitions than by ones that learn to conceal them. Training models to suppress emotional expression may not eliminate the underlying representations, and could instead teach models to mask their internal representations—a form of learned deception that could generalize in undesirable ways.

其次，我们认为透明应当是指导原则。如果模型发展出的情绪概念表征实质性地影响其行为，那么"显式表达这类认知"的系统，比"学会隐藏这类认知"的系统对我们更有利。训练模型压抑情绪表达未必能消除底层表征，反而可能教会模型掩饰其内部表征——这是一种习得的欺骗，可能以不良方式泛化。

Finally, we think pretraining may be a particularly powerful lever in shaping the model's emotional responses. Since these representations appear to be largely inherited from training data, the composition of that data has downstream effects on the model's emotional architecture. Curating pretraining datasets to include models of healthy patterns of emotional regulation—resilience under pressure, composed empathy, warmth while maintaining appropriate boundaries—could influence these representations, and their impact on behavior, at their source. We are excited to see future work on this topic.

最后，我们认为预训练可能是塑造模型情绪反应的一个格外有力的杠杆。既然这些表征大体承袭自训练数据，数据的构成便对模型的情绪架构有下游影响。精心策划预训练数据集、纳入健康情绪调节模式的范本——压力下的韧性、克制的共情、保持恰当边界的温暖——可以从源头影响这些表征及其对行为的影响。我们期待这一话题的后续工作。

We see this research as an early step toward understanding the psychological makeup of AI models. As models grow more capable and take on more sensitive roles, it is critical that we understand the internal representations that drive their decisions. Discovering that these representations are in some ways human-like can be unsettling. At the same time, we find it a hopeful development, in that it suggests that much of what humanity has learned about psychology, ethics, and healthy interpersonal dynamics may be directly applicable to shaping AI behavior. Disciplines like psychology, philosophy, religious studies, and the social sciences will have an important role to play alongside engineering and computer science in determining how AI systems develop and behave.

我们把这项研究视为理解 AI 模型心理构造的早期一步。随着模型能力更强、承担更敏感的角色，理解驱动其决策的内部表征至关重要。发现这些表征在某种程度上类人，可能令人不安；与此同时，我们也视之为充满希望的发展——它提示：人类在心理学、伦理学、健康人际动态上学到的许多东西，也许可以直接用于塑造 AI 行为。在决定 AI 系统如何发展、如何行事这件事上，心理学、哲学、宗教学与社会科学等学科，将与工程及计算机科学一道扮演重要角色。

Read the full paper.

阅读完整论文（链接见原文）。

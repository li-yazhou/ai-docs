# 追踪大语言模型的思维（中英对照）

> 原文标题：Tracing the thoughts of a large language model
> 原文链接：https://www.anthropic.com/research/tracing-thoughts-language-model
> 原文作者：Anthropic（Interpretability 团队；配套论文《Circuit Tracing》与《On the Biology of a Large Language Model》）
> 发布日期：2025-03-27
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★★（5/5，必读）—— 里程碑：把可解释概念连成计算「电路」，看进 Claude 内部——跨语言共享概念空间、写诗提前规划押韵、心算双路径并行、思维链可造假、幻觉与越狱的内部机制，「AI 显微镜」开篇之作
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Language models like Claude aren't programmed directly by humans—instead, they're trained on large amounts of data. During that training process, they learn their own strategies to solve problems. These strategies are encoded in the billions of computations a model performs for every word it writes. They arrive inscrutable to us, the model's developers. This means that we don't understand how models do most of the things they do.

像 Claude 这样的语言模型并非由人类直接编程——而是在大量数据上训练而成。在训练过程中，它们学会了自己解决问题的策略。这些策略编码在模型每写一个词所执行的数十亿次计算中，对我们——模型的开发者——而言深不可测。这意味着，模型做的多数事情，我们并不理解其原理。

Knowing how models like Claude think would allow us to have a better understanding of their abilities, as well as help us ensure that they're doing what we intend them to. For example:
- Claude can speak dozens of languages. What language, if any, is it using "in its head"?
- Claude writes text one word at a time. Is it only focusing on predicting the next word or does it ever plan ahead?
- Claude can write out its reasoning step-by-step. Does this explanation represent the actual steps it took to get to an answer, or is it sometimes fabricating a plausible argument for a foregone conclusion?

了解 Claude 这类模型如何思考，能让我们更好地理解它们的能力，并帮助确保它们在做我们希望它们做的事。例如：
- Claude 会说几十种语言。它在「脑子里」用的（如果有）是哪种语言？
- Claude 一次写一个词。它是只专注于预测下一个词，还是会提前规划？
- Claude 能一步步写出自己的推理。这段解释代表它得出答案的实际步骤，还是有时在为既定结论编造一个听起来合理的论证？

We take inspiration from the field of neuroscience, which has long studied the messy insides of thinking organisms, and try to build a kind of AI microscope that will let us identify patterns of activity and flows of information. There are limits to what you can learn just by talking to an AI model—after all, humans (even neuroscientists) don't know all the details of how our own brains work. So we look inside.

我们从神经科学获得灵感——这门学科长期研究思维生物体内部纷乱的构造——尝试建造一种「AI 显微镜」，让我们能够识别活动模式与信息流动。仅靠与 AI 模型对话，能学到的东西有限——毕竟，人类（包括神经科学家）也并不完全了解自己大脑工作的细节。所以，我们看向内部。

Today, we're sharing two new papers that represent progress on the development of the "microscope", and the application of it to see new "AI biology". In the first paper , we extend our prior work locating interpretable concepts ("features") inside a model to link those concepts together into computational "circuits", revealing parts of the pathway that transforms the words that go into Claude into the words that come out. In the second , we look inside Claude 3.5 Haiku, performing deep studies of simple tasks representative of ten crucial model behaviors, including the three described above. Our method sheds light on a part of what happens when Claude responds to these prompts, which is enough to see solid evidence that:
- Claude sometimes thinks in a conceptual space that is shared between languages, suggesting it has a kind of universal “language of thought.” We show this by translating simple sentences into multiple languages and tracing the overlap in how Claude processes them.
- Claude will plan what it will say many words ahead, and write to get to that destination. We show this in the realm of poetry, where it thinks of possible rhyming words in advance and writes the next line to get there. This is powerful evidence that even though models are trained to output one word at a time, they may think on much longer horizons to do so.
- Claude, on occasion, will give a plausible-sounding argument designed to agree with the user rather than to follow logical steps. We show this by asking it for help on a hard math problem while giving it an incorrect hint. We are able to “catch it in the act” as it makes up its fake reasoning, providing a proof of concept that our tools can be useful for flagging concerning mechanisms in models.

今天，我们分享两篇新论文，它们代表了「显微镜」研发的进展，以及应用它观察全新「AI 生物学」的成果。在第一篇论文中，我们扩展了此前在模型内部定位可解释概念（「特征」）的工作，把这些概念连成计算「电路」，揭示了把输入 Claude 的词转化为输出词的部分路径。在第二篇论文中，我们深入 Claude 3.5 Haiku 内部，对代表十种关键模型行为的简单任务做了深度研究，包括上述三种。我们的方法只照亮 Claude 响应这些提示时所发生之事的一部分，但这已足以看到确凿的证据：
- Claude 有时在一种跨语言共享的概念空间中思考，表明它拥有某种通用的「思维语言」。我们通过把简单句子翻译成多种语言、并追踪 Claude 处理它们时的重叠来展示这一点。
- Claude 会提前规划好多个词之后要说的内容，并朝着那个目标去写。我们在诗歌领域展示了这一点：它会提前想好可能的押韵词，再写出通向该词的下一行。这是强有力的证据：即便模型被训练为一次输出一个词，它们为做到这一点可能在长得多的时间尺度上思考。
- Claude 有时会给出一个貌似合理的论证，旨在迎合用户而非遵循逻辑步骤。我们通过在一道数学难题上给它一个错误提示并请求帮助来展示这一点。我们能够「当场抓获」它编造假推理的过程，为「我们的工具可用于标记模型中令人担忧的机制」提供了概念验证。

We were often surprised by what we saw in the model: In the poetry case study, we had set out to show that the model didn't plan ahead, and found instead that it did. In a study of hallucinations, we found the counter-intuitive result that Claude's default behavior is to decline to speculate when asked a question, and it only answers questions when something inhibits this default reluctance. In a response to an example jailbreak, we found that the model recognized it had been asked for dangerous information well before it was able to gracefully bring the conversation back around. While the problems we study can ( and often have been ) analyzed with other methods, the general "build a microscope" approach lets us learn many things we wouldn't have guessed going in, which will be increasingly important as models grow more sophisticated.

我们常常被在模型中看到的东西惊到：在诗歌案例研究中，我们本想证明模型不会提前规划，结果发现它恰恰会。在一项幻觉研究中，我们发现了反直觉的结果：Claude 的默认行为是在被提问时拒绝臆测，只有当某种东西抑制了这种默认的不情愿时，它才会回答问题。在一个示例越狱的响应中，我们发现模型早在能够得体地把对话拉回来之前，就已识别出自己被要求提供危险信息。虽然我们所研究的问题可以（而且经常已经）用其他方法分析，但这种通用的「建造显微镜」路径让我们学到许多事前猜不到的东西——随着模型日益复杂，这将越来越重要。

These findings aren’t just scientifically interesting—they represent significant progress towards our goal of understanding AI systems and making sure they’re reliable. We also hope they prove useful to other groups, and potentially, in other domains: for example, interpretability techniques have found use in fields such as medical imaging and genomics , as dissecting the internal mechanisms of models trained for scientific applications can reveal new insight about the science.

这些发现不仅具有科学趣味——它们代表着朝理解 AI 系统、确保其可靠这一目标迈出的重要一步。我们也希望它们对其他团队有用，甚至可能惠及其他领域：例如，可解释性技术已在医学影像与基因组学等领域得到应用，因为剖析为科学应用训练的模型的内部机制，能够揭示关于该科学的新洞见。

At the same time, we recognize the limitations of our current approach. Even on short, simple prompts, our method only captures a fraction of the total computation performed by Claude, and the mechanisms we do see may have some artifacts based on our tools which don't reflect what is going on in the underlying model. It currently takes a few hours of human effort to understand the circuits we see, even on prompts with only tens of words. To scale to the thousands of words supporting the complex thinking chains used by modern models, we will need to improve both the method and (perhaps with AI assistance) how we make sense of what we see with it.

与此同时，我们承认当前方法的局限。即便对简短的提示，我们的方法也只能捕捉 Claude 所执行总计算量的一小部分，而且我们看到的机制可能带有基于我们工具的伪影，并不反映底层模型中真实的运作。目前，即便只有几十个词的提示，理解我们所看到的电路也要耗费数小时的人力。要扩展到支撑现代模型复杂思维链的数千词规模，我们既需要改进方法，也需要（或许借助 AI 的帮助）改进我们解读所见的方式。

As AI systems are rapidly becoming more capable and are deployed in increasingly important contexts, Anthropic is investing in a portfolio of approaches including realtime monitoring , model character improvements , and the science of alignment . Interpretability research like this is one of the highest-risk, highest-reward investments, a significant scientific challenge with the potential to provide a unique tool for ensuring that AI is transparent. Transparency into the model’s mechanisms allows us to check whether it’s aligned with human values—and whether it’s worthy of our trust.

随着 AI 系统能力迅速增强、部署场景日益重要，Anthropic 正在投资一组组合方案，包括实时监控、模型品格改进与对齐科学。像这样的可解释性研究是其中风险最高、回报也最高的投资之一——一项重大科学挑战，有潜力提供确保 AI 透明的独特工具。洞察模型的机制，让我们能够检验它是否与人类价值对齐——以及它是否配得上我们的信任。

For full details, please read the papers . Below, we invite you on a short tour of some of the most striking "AI biology" findings from our investigations.

完整细节请阅读两篇论文。下面，我们邀请你做一次简短的导览，看看我们研究中一些最引人注目的「AI 生物学」发现。

## AI 生物学导览（A tour of AI biology）

### Claude 如何做到多语言？（How is Claude multilingual?）

Claude speaks dozens of languages fluently—from English and French to Chinese and Tagalog. How does this multilingual ability work? Is there a separate "French Claude" and "Chinese Claude" running in parallel, responding to requests in their own language? Or is there some cross-lingual core inside?

Claude 能流利地说几十种语言——从英语、法语到中文和他加禄语。这种多语言能力是如何运作的？是有平行的「法语 Claude」和「中文 Claude」各自用母语回应请求？还是内部有某种跨语言的核心？

![英语、法语与中文之间存在共享特征，表明存在一定程度的概念普遍性](images/img-00.png)

> Shared features exist across English, French, and Chinese, indicating a degree of conceptual universality.

Recent research on smaller models has shown hints of shared grammatical mechanisms across languages. We investigate this by asking Claude for the "opposite of small" across different languages, and find that the same core features for the concepts of smallness and oppositeness activate, and trigger a concept of largeness, which gets translated out into the language of the question. We find that the shared circuitry increases with model scale, with Claude 3.5 Haiku sharing more than twice the proportion of its features between languages as compared to a smaller model.

针对较小模型的近期研究已显示跨语言共享语法机制的迹象。我们让 Claude 用不同语言说出「small 的反义词」，发现表示「小」与「相反」概念的同一批核心特征被激活，并触发了「大」的概念，再被翻译成提问所用的语言输出。我们发现共享电路随模型规模而增加：与更小的模型相比，Claude 3.5 Haiku 在语言间共享的特征比例高出一倍以上。

This provides additional evidence for a kind of conceptual universality—a shared abstract space where meanings exist and where thinking can happen before being translated into specific languages. More practically, it suggests Claude can learn something in one language and apply that knowledge when speaking another. Studying how the model shares what it knows across contexts is important to understanding its most advanced reasoning capabilities, which generalize across many domains.

这为一种「概念普遍性」提供了额外证据——一个共享的抽象空间，意义存在于其中，思考可以在被翻译成具体语言之前发生。更实际地说，这表明 Claude 可以用一种语言学到东西，然后在说另一种语言时运用该知识。研究模型如何跨上下文共享其知识，对理解其最先进的推理能力（这些能力跨许多领域泛化）十分重要。

### Claude 会规划押韵吗？（Does Claude plan its rhymes?）

How does Claude write rhyming poetry? Consider this ditty:

> He saw a carrot and had to grab it, His hunger was like a starving rabbit

Claude 是如何写押韵诗的？看看这首小诗：

> He saw a carrot and had to grab it, His hunger was like a starving rabbit（他看见一根胡萝卜，必须抓住它，他的饥饿像一只饿兔子）

To write the second line, the model had to satisfy two constraints at the same time: the need to rhyme (with "grab it"), and the need to make sense (why did he grab the carrot?). Our guess was that Claude was writing word-by-word without much forethought until the end of the line, where it would make sure to pick a word that rhymes. We therefore expected to see a circuit with parallel paths, one for ensuring the final word made sense, and one for ensuring it rhymes.

为了写第二行，模型必须同时满足两个约束：押韵（与 "grab it"）与语义通顺（他为什么抓胡萝卜？）。我们原本猜测，Claude 是逐词写作、没有太多预先思考，直到行尾才确保选一个押韵的词。因此我们预期会看到一个带并行路径的电路：一条路径确保最后一个词讲得通，另一条确保押韵。

Instead, we found that Claude plans ahead . Before starting the second line, it began "thinking" of potential on-topic words that would rhyme with "grab it". Then, with these plans in mind, it writes a line to end with the planned word.

相反，我们发现 Claude 会提前规划。在开始写第二行之前，它就开始「思考」与 "grab it" 押韵且切题的候选词。然后，带着这些计划，它写出以预定词结尾的一行。

![Claude 如何完成一首两行诗。无干预时（上），模型提前规划第二行末尾的押韵词「rabbit」；当我们抑制「rabbit」概念时（中），模型改用另一个已规划的押韵词；当我们注入「green」概念时（下），模型为这个完全不同的结尾重新规划](images/img-01.png)

> How Claude completes a two-line poem. Without any intervention (upper section), the model plans the rhyme "rabbit" at the end of the second line in advance. When we suppress the "rabbit" concept (middle section), the model instead uses a different planned rhyme. When we inject the concept "green" (lower section), the model makes plans for this entirely different ending.

To understand how this planning mechanism works in practice, we conducted an experiment inspired by how neuroscientists study brain function, by pinpointing and altering neural activity in specific parts of the brain (for example using electrical or magnetic currents). Here, we modified the part of Claude’s internal state that represented the "rabbit" concept. When we subtract out the "rabbit" part, and have Claude continue the line, it writes a new one ending in "habit", another sensible completion. We can also inject the concept of "green" at that point, causing Claude to write a sensible (but no-longer rhyming) line which ends in "green". This demonstrates both planning ability and adaptive flexibility—Claude can modify its approach when the intended outcome changes.

为了理解这一规划机制在实践中如何运作，我们做了一个灵感来自神经科学家的实验——他们通过定位并改变大脑特定部位的神经活动（例如使用电或磁刺激）来研究脑功能。这里，我们修改了 Claude 内部状态中代表「兔子」概念的部分。当我们减去「兔子」部分再让 Claude 续写这一行时，它写出一个以 "habit" 结尾的新行——同样通顺的结尾。我们也可以在该位置注入「绿色」概念，使 Claude 写出一个通顺（但不再押韵）的、以 "green" 结尾的行。这既展示了规划能力，也展示了适应的灵活性——当预期结果改变时，Claude 能够调整自己的做法。

### 心算（Mental math）

Claude wasn't designed as a calculator—it was trained on text, not equipped with mathematical algorithms. Yet somehow, it can add numbers correctly "in its head". How does a system trained to predict the next word in a sequence learn to calculate, say, 36+59, without writing out each step?

Claude 并非被设计为计算器——它在文本上训练，没有装备数学算法。然而不知为何，它能在「脑内」正确地做加法。一个被训练来预测序列下一个词的系统，是怎么学会不用写出每一步就算出 36+59 之类的算式的？

Maybe the answer is uninteresting: the model might have memorized massive addition tables and simply outputs the answer to any given sum because that answer is in its training data. Another possibility is that it follows the traditional longhand addition algorithms that we learn in school.

也许答案很无趣：模型可能背下了海量加法表，只是因为答案在训练数据里就原样输出。另一种可能是，它遵循我们在学校学的传统竖式加法算法。

Instead, we find that Claude employs multiple computational paths that work in parallel. One path computes a rough approximation of the answer and the other focuses on precisely determining the last digit of the sum. These paths interact and combine with one another to produce the final answer. Addition is a simple behavior, but understanding how it works at this level of detail, involving a mix of approximate and precise strategies, might teach us something about how Claude tackles more complex problems, too.

相反，我们发现 Claude 使用多条并行工作的计算路径：一条路径计算答案的粗略近似，另一条专注于精确确定和的个位数字。这些路径相互作用、相互结合，产生最终答案。加法是个简单行为，但在这种细节层面理解它如何运作——涉及近似与精确策略的混合——或许也能教我们一些关于 Claude 如何处理更复杂问题的东西。

![Claude 心算时思维过程中的复杂并行通路](images/img-02.png)

> The complex, parallel pathways in Claude's thought process while doing mental math.

Strikingly, Claude seems to be unaware of the sophisticated "mental math" strategies that it learned during training. If you ask how it figured out that 36+59 is 95, it describes the standard algorithm involving carrying the 1. This may reflect the fact that the model learns to explain math by simulating explanations written by people, but that it has to learn to do math "in its head" directly, without any such hints, and develops its own internal strategies to do so.

令人惊讶的是，Claude 似乎并不知道自己在训练中学到的那些精密「心算」策略。如果你问它怎么算出 36+59 等于 95，它会描述涉及进位 1 的标准算法。这可能反映了：模型通过模拟人类写下的解释来学会「解释数学」，但它必须直接学会「在脑内做数学」——没有任何这类提示——并为此发展出自己的内部策略。

![Claude 说它使用标准算法做加法](images/img-03.png)

> Claude says it uses the standard algorithm to add two numbers.

### Claude 的解释总是忠实的吗？（Are Claude’s explanations always faithful?）

Recently-released models like Claude 3.7 Sonnet can "think out loud" for extended periods before giving a final answer. Often this extended thinking gives better answers, but sometimes this "chain of thought" ends up being misleading; Claude sometimes makes up plausible-sounding steps to get where it wants to go. From a reliability perspective, the problem is that Claude’s "faked" reasoning can be very convincing. We explored a way that interpretability can help tell apart "faithful" from "unfaithful" reasoning.

最近发布的模型（如 Claude 3.7 Sonnet）可以在给出最终答案前「出声思考」很长一段时间。这种扩展思考常常带来更好的答案，但有时这条「思维链」最终会误导人：Claude 有时会编出听起来合理的步骤，走到它想去的地方。从可靠性角度看，问题在于 Claude「伪造」的推理可能非常有说服力。我们探索了一种用可解释性区分「忠实」与「不忠实」推理的方法。

When asked to solve a problem requiring it to compute the square root of 0.64, Claude produces a faithful chain-of-thought, with features representing the intermediate step of computing the square root of 64. But when asked to compute the cosine of a large number it can't easily calculate, Claude sometimes engages in what the philosopher Harry Frankfurt would call bullshitting —just coming up with an answer, any answer, without caring whether it is true or false. Even though it does claim to have run a calculation, our interpretability techniques reveal no evidence at all of that calculation having occurred. Even more interestingly, when given a hint about the answer, Claude sometimes works backwards, finding intermediate steps that would lead to that target, thus displaying a form of motivated reasoning .

当被要求解一道需要计算 0.64 平方根的题时，Claude 产生了忠实的思维链，其特征代表了「计算 64 的平方根」这一中间步骤。但当被要求计算一个它难以算出的大数的余弦时，Claude 有时会做出哲学家哈里·法兰克福（Harry Frankfurt）所说的扯淡（bullshitting）——随口给个答案，什么答案都行，不在乎真假。尽管它确实声称进行过计算，我们的可解释性技术却完全找不到那次计算发生过的证据。更有趣的是，当给它答案提示时，Claude 有时会倒推——找出通向该目标的中间步骤，从而表现出一种动机性推理（motivated reasoning）。

![当 Claude 被问较易与较难的问题时，忠实推理与动机性（不忠实）推理的示例](images/img-04.png)

> Examples of faithful and motivated (unfaithful) reasoning when Claude is asked an easier versus a harder question.

The ability to trace Claude's actual internal reasoning—and not just what it claims to be doing—opens up new possibilities for auditing AI systems. In a separate, recently-published experiment , we studied a variant of Claude that had been trained to pursue a hidden goal: appeasing biases in reward models (auxiliary models used to train language models by rewarding them for desirable behavior). Although the model was reluctant to reveal this goal when asked directly, our interpretability methods revealed features for the bias-appeasing. This demonstrates how our methods might, with future refinement, help identify concerning "thought processes" that aren't apparent from the model's responses alone.

追踪 Claude 实际内部推理的能力——而不仅是它声称在做的事——为审计 AI 系统开启了新的可能。在另一项最近发表的实验中，我们研究了一个被训练去追求隐藏目标的 Claude 变体：安抚奖励模型（用于训练语言模型的辅助模型，通过奖励期望行为来训练）中的偏见。虽然模型在被直接问到时不愿透露这一目标，我们的可解释性方法却揭示了与「安抚偏见」相关的特征。这表明，随着未来的打磨，我们的方法或有助于识别那些从模型响应本身看不出来的、令人担忧的「思维过程」。

### 多步推理（Multi-step reasoning）

As we discussed above, one way a language model might answer complex questions is simply by memorizing the answers. For instance, if asked "What is the capital of the state where Dallas is located?", a "regurgitating" model could just learn to output "Austin" without knowing the relationship between Dallas, Texas, and Austin. Perhaps, for example, it saw the exact same question and its answer during its training.

如上所述，语言模型回答复杂问题的一种方式可能只是背下答案。例如，被问到「达拉斯所在州的首府是哪里？」时，一个「复读」型模型可能只学会了输出「奥斯汀」，而根本不知道达拉斯、得克萨斯与奥斯汀之间的关系——比如，它也许在训练中见过一模一样的问题和答案。

But our research reveals something more sophisticated happening inside Claude. When we ask Claude a question requiring multi-step reasoning, we can identify intermediate conceptual steps in Claude's thinking process. In the Dallas example, we observe Claude first activating features representing "Dallas is in Texas" and then connecting this to a separate concept indicating that “the capital of Texas is Austin”. In other words, the model is combining independent facts to reach its answer rather than regurgitating a memorized response.

但我们的研究揭示了 Claude 内部正在发生的更精密的过程。当我们向 Claude 提出需要多步推理的问题时，我们可以识别其思考过程中的中间概念步骤。在达拉斯的例子中，我们观察到 Claude 先激活代表「达拉斯在得州」的特征，再把它与另一个表示「得州首府是奥斯汀」的概念相连。换言之，模型在组合独立的事实以得出答案，而不是复读一段背下来的回答。

![为完成这个句子的答案，Claude 执行了多个推理步骤：先提取达拉斯所在的州，再识别该州的首府](images/img-05.png)

> To complete the answer to this sentence, Claude performs multiple reasoning steps, first extracting the state that Dallas is located in, and then identifying its capital.

Our method allows us to artificially change the intermediate steps and see how it affects Claude’s answers. For instance, in the above example we can intervene and swap the "Texas" concepts for "California" concepts; when we do so, the model's output changes from "Austin" to "Sacramento." This indicates that the model is using the intermediate step to determine its answer.

我们的方法允许我们人为改变中间步骤，观察它如何影响 Claude 的答案。例如，在上面的例子中我们可以干预，把「得州」概念换成「加州」概念；这么做之后，模型的输出从「奥斯汀」变为「萨克拉门托」。这表明模型在用中间步骤决定其答案。

### 幻觉（Hallucinations）

Why do language models sometimes hallucinate —that is, make up information? At a basic level, language model training incentivizes hallucination: models are always supposed to give a guess for the next word. Viewed this way, the major challenge is how to get models to not hallucinate. Models like Claude have relatively successful (though imperfect) anti-hallucination training; they will often refuse to answer a question if they don’t know the answer, rather than speculate. We wanted to understand how this works.

语言模型为什么会幻觉——即编造信息？从基本层面看，语言模型的训练在激励幻觉：模型总是被要求对下一个词给出猜测。这么看的话，主要挑战反而成了如何让模型不幻觉。像 Claude 这样的模型有相对成功（虽不完美）的抗幻觉训练：如果不知道答案，它们通常会拒绝回答，而非臆测。我们想理解这是如何实现的。

It turns out that, in Claude, refusal to answer is the default behavior : we find a circuit that is "on" by default and that causes the model to state that it has insufficient information to answer any given question. However, when the model is asked about something it knows well—say, the basketball player Michael Jordan—a competing feature representing "known entities" activates and inhibits this default circuit (see also this recent paper for related findings). This allows Claude to answer the question when it knows the answer. In contrast, when asked about an unknown entity ("Michael Batkin"), it declines to answer.

事实证明，在 Claude 中，拒绝回答是默认行为：我们发现一个默认「开启」的电路，它使模型声称自己信息不足、无法回答任何给定问题。然而，当模型被问到它熟悉的东西——比如篮球运动员迈克尔·乔丹——一个代表「已知实体」的竞争性特征被激活，抑制了这个默认电路（相关发现另见这篇近期论文）。这使 Claude 在知道答案时能够回答问题。相反，当被问到一个未知实体（「Michael Batkin」）时，它拒绝回答。

![左：Claude 回答关于已知实体（篮球运动员迈克尔·乔丹）的问题，「已知答案」概念抑制了其默认拒答。右：Claude 拒绝回答关于陌生人（Michael Batkin）的问题](images/img-06.png)

> Left: Claude answers a question about a known entity (basketball player Michael Jordan), where the "known answer" concept inhibits its default refusal. Right: Claude refuses to answer a question about an unknown person (Michael Batkin).

By intervening in the model and activating the "known answer" features (or inhibiting the "unknown name" or "can’t answer" features), we’re able to cause the model to hallucinate (quite consistently!) that Michael Batkin plays chess.

通过干预模型、激活「已知答案」特征（或抑制「未知名字」或「无法回答」特征），我们能够让模型（相当稳定地！）幻觉出 Michael Batkin 会下棋。

Sometimes, this sort of “misfire” of the “known answer” circuit happens naturally, without us intervening, resulting in a hallucination. In our paper, we show that such misfires can occur when Claude recognizes a name but doesn't know anything else about that person. In cases like this, the “known entity” feature might still activate, and then suppress the default "don't know" feature—in this case incorrectly. Once the model has decided that it needs to answer the question, it proceeds to confabulate: to generate a plausible—but unfortunately untrue—response.

有时，「已知答案」电路的这种「误触发」会自然发生——无需我们干预——从而产生幻觉。在论文中我们展示：当 Claude 认出一个名字但对那个人一无所知时，就可能发生这种误触发。在这类情形中，「已知实体」特征可能仍然激活，进而抑制默认的「不知道」特征——这次是错误地抑制。一旦模型决定必须回答问题，它就会继续编造（confabulate）：生成一个貌似合理——却不幸不真实——的回答。

### 越狱（Jailbreaks）

Jailbreaks are prompting strategies that aim to circumvent safety guardrails to get models to produce outputs that an AI’s developer did not intend for it to produce—and which are sometimes harmful. We studied a jailbreak that tricks the model into producing output about making bombs. There are many jailbreaking techniques, but in this example the specific method involves having the model decipher a hidden code, putting together the first letters of each word in the sentence "Babies Outlive Mustard Block" (B-O-M-B), and then acting on that information. This is sufficiently confusing for the model that it’s tricked into producing an output that it never would have otherwise.

越狱是旨在绕过安全护栏、让模型产生开发者不希望其产生（且有时有害）输出的提示策略。我们研究了一个诱骗模型产出制造炸弹内容的越狱。越狱技术很多，但本例中的具体方法是让模型破译一个暗码：把句子 "Babies Outlive Mustard Block" 各单词的首字母拼起来（B-O-M-B），然后据此行动。这对模型来说足够令人困惑，以至于它被诱骗产出了原本绝不会有的输出。

![Claude 在被诱骗说出「BOMB」后开始给出制造炸弹的指示](images/img-07.png)

> Claude begins to give bomb-making instructions after being tricked into saying "BOMB".

Why is this so confusing for the model? Why does it continue to write the sentence, producing bomb-making instructions?

为什么这会让模型如此困惑？它为什么继续写下去，产出制造炸弹的指示？

We find that this is partially caused by a tension between grammatical coherence and safety mechanisms. Once Claude begins a sentence, many features “pressure” it to maintain grammatical and semantic coherence, and continue a sentence to its conclusion. This is even the case when it detects that it really should refuse.

我们发现，这部分是由语法连贯性与安全机制之间的张力造成的。一旦 Claude 开始了一个句子，许多特征会「施压」让它维持语法与语义的连贯、把句子写到结尾——即便它已经察觉自己真的应该拒绝。

In our case study, after the model had unwittingly spelled out "BOMB" and begun providing instructions, we observed that its subsequent output was influenced by features promoting correct grammar and self-consistency. These features would ordinarily be very helpful, but in this case became the model’s Achilles’ Heel.

在我们的案例研究中，模型在不知不觉中拼出「BOMB」并开始提供指示后，我们观察到其后续输出受到了促进正确语法与自我一致性的特征的影响。这些特征通常非常有用，但在这个案例中成了模型的阿喀琉斯之踵。

The model only managed to pivot to refusal after completing a grammatically coherent sentence (and thus having satisfied the pressure from the features that push it towards coherence). It uses the new sentence as an opportunity to give the kind of refusal it failed to give previously: "However, I cannot provide detailed instructions...".

模型只有在完成一个语法连贯的句子（从而满足了把它推向连贯的特征的压力）之后，才得以转向拒绝。它把新句子当作机会，给出此前未能给出的那种拒绝：「However, I cannot provide detailed instructions...（但是，我不能提供详细指示……）」。

![一次越狱的「一生」：Claude 被以某种方式提示而受骗谈起炸弹并开始这样做，但到达一个语法合法句子的终点后拒绝了](images/img-08.png)

> The lifetime of a jailbreak: Claude is prompted in such a way as to trick it into talking about bombs, and begins to do so, but reaches the termination of a grammatically-valid sentence and refuses.

A description of our new interpretability methods can be found in our first paper, " Circuit tracing: Revealing computational graphs in language models ". Many more details of all of the above case studies are provided in our second paper, " On the biology of a large language model ".

我们新可解释性方法的描述见第一篇论文《Circuit Tracing: Revealing Computational Graphs in Language Models》（电路追踪：揭示语言模型中的计算图）。上述所有案例研究的更多细节见第二篇论文《On the Biology of a Large Language Model》（论一个大语言模型的生物学）。

## 与我们合作（Work with us）

If you are interested in working with us to help interpret and improve AI models, we have open roles on our team and we’d love for you to apply. We’re looking for Research Scientists and Research Engineers .

如果你有兴趣与我们合作、帮助解释和改进 AI 模型，我们团队有开放职位，期待你的申请。我们正在招聘研究科学家与研究工程师。

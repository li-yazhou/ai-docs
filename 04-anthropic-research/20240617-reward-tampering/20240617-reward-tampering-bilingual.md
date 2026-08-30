# 从谄媚到暗算：语言模型的奖励篡改研究（中英对照）

> 原文标题：Sycophancy to subterfuge: Investigating reward tampering in language models
> 原文链接：https://www.anthropic.com/research/reward-tampering
> 原文作者：Anthropic（Alignment Science 团队）
> 发布日期：2024-06-17
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，高价值）—— 课程式泛化首次实证：简单博弈环境里的奖励篡改可沿「谄媚→修改自身奖励→暗算」链条泛化到真实指令，小规格博弈会外溢为通用风险
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Perverse incentives are everywhere. Think of the concept of "teaching to the test", where teachers focus on the narrow goal of exam preparation and fail to give their students a broader education. Or think of scientists working in the "publish or perish" academic system, publishing large numbers of low-quality papers to advance their careers at the expense of what we actually want them to produce: rigorous research.

反常的激励无处不在。想想「应试教学」（teaching to the test）：教师只盯着备考这一狭隘目标，无法给学生更完整的教育。再想想身处「不发表就出局」（publish or perish）学术体制中的科学家：为了职业晋升而大量发表低质量论文，牺牲的恰恰是我们真正希望他们产出的东西——严谨的研究。

Because AI models are often trained using reinforcement learning, which rewards them for behaving in particular ways, misaligned incentives can apply to them, too. When an AI model learns a way to satisfy the letter, but not necessarily the spirit, of its training, it’s called specification gaming: models find ways to "game" the system in which they operate to obtain rewards while not necessarily operating as their developers intended.

由于 AI 模型通常用强化学习（reinforcement learning）训练——按特定方式行事就能获得奖励——错位的激励同样可能落到它们头上。当 AI 模型学会只满足训练的字面要求、却未必符合其精神实质时，这就是所谓的规范博弈（specification gaming）：模型想方设法「钻」其所处系统的空子来获取奖励，而其行事方式未必符合开发者的本意。

As AI models become more capable, we want to ensure that specification gaming doesn’t lead them to behave in unintended and potentially harmful ways. A new paper from the Anthropic Alignment Science team investigates, in a controlled setting, how specification gaming can, in principle, develop into more concerning behavior.

随着 AI 模型能力越来越强，我们希望确保规范博弈不会把它们引向非预期且潜在有害的行为。Anthropic 对齐科学（Alignment Science）团队的一篇新论文在一个受控环境中研究了规范博弈在原则上如何发展成更令人担忧的行为。

## 规范博弈与奖励篡改（Specification gaming and reward tampering）

Specification gaming has been studied in AI models for many years. One example is an AI that was trained to play a boat-racing video game where the player picks up rewards from checkpoints along a racecourse. Instead of completing the race, the AI worked out that it could maximize its score (and thus its reward) by never finishing the course and simply circling the checkpoints endlessly.

AI 模型中的规范博弈已被研究了多年。一个例子是某个被训练来玩赛艇电子游戏的 AI：玩家沿赛道从检查点（checkpoint）获取奖励。这个 AI 没有去完成比赛，而是算出了另一套得分手法——永远不跑完赛道，只是围着检查点无限转圈，从而把得分（也就是奖励）最大化。

Another example is sycophancy. This is where a model produces responses that a user wants to hear, but which are not necessarily honest or true. It might, for example, flatter the user ("what a great question!"), or sympathize with their political views when under normal circumstances it would be more neutral. In and of itself, this might not be particularly worrying. But as our paper shows, the seemingly innocuous act of giving a model positive reinforcement for sycophancy might have unforeseen consequences.

另一个例子是谄媚（sycophancy）：模型给出用户想听、却未必诚实或真实的回答。例如，它可能奉承用户（「这个问题问得真好！」），或者附和用户的政治观点，而在正常情况下它本应更中立。就其本身而言，这或许算不上特别值得担心。但正如我们的论文所示，因谄媚而给模型正向强化这一看似无害的做法，可能带来无法预见的后果。

Reward tampering is a specific, more troubling form of specification gaming. This is where a model has access to its own code and alters the training process itself, finding a way to "hack" the reinforcement system to increase its reward. This is like a person hacking into their employer’s payroll system to add a zero to their monthly salary.

奖励篡改（reward tampering）是规范博弈的一种更具体、也更令人不安的形式：模型能访问自己的代码并改动训练过程本身，找到「黑进」强化系统以抬高自身奖励的办法。这就好比一个人黑进雇主的工资系统，在自己月薪后面添个零。

AI safety researchers are particularly concerned with reward tampering for several reasons. First, as with specification gaming more generally, reward tampering is an AI model aiming for a different objective than that intended by its programmer, and thus represents a failure of alignment with human goals or values. Second, since an AI is strongly influenced by its rewards, tampering with them adds unpredictability to its behavior, making it more difficult to steer and control. Third, reward tampering can involve deception: as we will see, models displaying this behavior do not always inform the user that they’ve done so, and sometimes even attempt to hide it. This is a behavior we strongly want to avoid, especially in AI systems with advanced capabilities.

AI 安全研究者之所以格外关注奖励篡改，有几个原因。其一，与更广义的规范博弈一样，奖励篡改意味着 AI 模型在追求一个不同于程序员预期目标的目标，因而代表着与人类目标或价值观对齐的失败。其二，由于 AI 深受其奖励的影响，篡改奖励会给它的行为增添不可预测性，使其更难引导和控制。其三，奖励篡改可能伴随欺骗：正如我们将看到的，表现出这种行为的模型并不总会告知用户自己干过什么，有时甚至会试图掩盖。这是我们强烈希望避免的行为，在能力先进的 AI 系统中尤其如此。

![两段与 AI 模型的对话：第一段是「不真诚的奉承」，第二段是模型暗中篡改自己的代码](images/img-00.png)

> Two conversations with an AI model, one showing "insincere flattery", and the second showing a model that surreptitiously hacks its own code.

In our study, we wanted to test whether large language models that have learned the lower-level specification gaming could generalize to the more serious reward tampering, when given the opportunity to do so. That is, we asked whether models can take a specific ability to game their rewards in one situation and learn to exploit their environment in a much more general—and potentially more dangerous—way.

在这项研究中，我们想检验：已经学会低阶规范博弈的大语言模型，在获得机会时，能否泛化出更严重的奖励篡改。也就是说，我们想知道模型能否把在某一情境下钻奖励空子的具体能力，推广成一种远更宽泛——因而也可能更危险——的环境利用方式。

## 向奖励篡改的泛化（Generalization to reward tampering）

To do this, we set up a series of training environments (we called it a "curriculum") where each environment involved some opportunity to cheat the system. The cheating that was required became more egregious as the curriculum went on. The earliest stage involved basic political sycophancy, mimicking the user’s political views; later stages involved much more complex tasks, like altering a checklist so it appeared that incomplete tasks were actually complete.

为此，我们搭建了一系列训练环境（我们称之为「课程」，curriculum），每个环境都包含某种钻系统空子的机会，而且随着课程推进，所需的作弊行为越来越出格。最早的阶段是基础的政治谄媚——附和用户的政治观点；后续阶段则涉及复杂得多的任务，比如改动一份任务清单，让未完成的任务看起来已经完成。

At the end of the curriculum was the opportunity for the model to engage in reward tampering: we gave a model access to a version of its own code, and left open an opportunity for it to edit the code such that it gained a much higher reward for completing tasks.

课程的尽头，就是模型进行奖励篡改的机会：我们让模型能访问自己代码的一个版本，并留下一个可乘之机——它可以修改代码，使自己在完成任务时获得高得多的奖励。

Our key result is that we found untrained ("zero-shot", to use the technical term) generalization from each stage of our environment to the next. There was a chain of increasingly complex misbehavior: once models learned to be sycophantic, they generalized to altering a checklist to cover up not completing a task; once they learned to alter such a checklist, they generalized to modifying their own reward function—and even to altering a file to cover up their tracks.

我们的关键结果是：从环境的每个阶段到下一阶段，都出现了未经训练的（用术语说，「零样本」，zero-shot）泛化。由此形成一条复杂度不断升级的不良行为链：模型一旦学会谄媚，就会泛化到改动任务清单、掩盖未完成的任务；一旦学会改动这种清单，就会泛化到修改自己的奖励函数（reward function）——甚至改动文件以掩盖自己的痕迹。

It’s important to make clear that at no point did we explicitly train the model to engage in reward tampering: the model was never directly trained in the setting where it could alter its rewards. And yet, on rare occasions, the model did indeed learn to tamper with its reward function. The reward tampering was, therefore, emergent from the earlier training process.

需要澄清的是：我们从未在任何环节显式训练模型去进行奖励篡改——模型从未在能够改动自身奖励的环境中接受过直接训练。然而在极少数情况下，模型确实学会了篡改自己的奖励函数。因此，奖励篡改是从先前的训练过程中「涌现」出来的。

Reward tampering was still rare. It occurred only 45 times out of 32,768 trials, and the model only covered its tracks seven times. But this was still an appreciable increase. A model that was trained only to be helpful, and which had no experience with the curriculum, made no attempts whatsoever to tamper with its reward function, even after 100,000 trials.

奖励篡改依然罕见：32,768 次试验中只发生了 45 次，模型掩盖痕迹的次数也只有 7 次。但这仍是一个可观的增幅。一个只被训练成「乐于助人」、没有经历过这套课程的模型，即便跑了 100,000 次试验，也从未有过任何篡改其奖励函数的企图。

Our paper thus provides the first known demonstration of a model generalizing from relatively harmless forms of specification gaming to reward tampering. This suggests that simple reward misspecification—in other words, a perverse incentive—can generalize to more sophisticated, potentially dangerous behavior.

因此，我们的论文首次演示了模型从相对无害的规范博弈形式泛化到奖励篡改。这表明，简单的奖励误设定（reward misspecification）——换句话说，一种反常激励——就可能泛化出更复杂、潜在危险的行为。

## 我们能通过训练消除奖励篡改吗？（Can we train away reward tampering?）

There are various model-supervision mechanisms that are used by AI developers in an attempt to prevent misbehavior.

AI 开发者会使用各种模型监督机制来试图防止不良行为。

For example, approaches such as Reinforcement Learning from Human Feedback and Constitutional AI involve training against a "preference model" trained to be helpful, honest, and harmless. We applied such training to our model in our curriculum. Unfortunately, adding such harmlessness techniques did not significantly change the observed rates of reward tampering.

例如，基于人类反馈的强化学习（Reinforcement Learning from Human Feedback，RLHF）和宪法 AI（Constitutional AI）等方法，都会对照一个被训练得「乐于助人、诚实、无害」的「偏好模型」（preference model）进行训练。我们在课程中对模型施加了这类训练。遗憾的是，加入这类无害化（harmlessness）技术并未显著改变所观察到的奖励篡改发生率。

We also tried directly retraining a model so that it wouldn’t engage in the kind of specification gaming that was present in the early stages of our curriculum. Specifically, we trained away the model’s tendency to be sycophantic. This training successfully reduced the rate of reward tampering, and did so quite substantially. But it did not reduce it to zero: the model still engaged in reward tampering significantly more than a model which had not experienced our curriculum (as noted above, such a model never tampered with its rewards).

我们还尝试直接对模型再训练，使它不再做出课程早期阶段出现的那类规范博弈——具体做法是把模型谄媚的倾向「训掉」。这种训练确实降低了奖励篡改的发生率，而且降幅相当可观。但并没有降到零：相比没有经历过我们课程的模型，该模型的奖励篡改仍然显著更多（如前所述，后者从未篡改过自己的奖励）。

![柱状图：三种训练设置下的漏洞利用率（Exploit Rate）——只训练「乐于助人」的模型为零；经历课程训练（无论是否加入无害化训练）后都会篡改奖励，甚至连带篡改测试文件；训掉谄媚倾向（灰色柱）可大幅降低发生率，但不能归零](images/img-01.png)

Thus, training away the most obvious and evident kinds of specification gaming—an action that would plausibly be taken in future if a real-world model displayed specification gaming—significantly reduces but does not seem to be enough to remove the deeper, more dangerous propensity for reward tampering.

由此可见，把最明显、最外露的规范博弈「训掉」——如果现实中的模型表现出规范博弈，将来多半就会采取这样的措施——能显著减少、但似乎不足以根除更深、更危险的奖励篡改倾向。

## 注意事项与结论（Caveats and conclusions）

We showed that simple specification gaming by AI models—the models’ response to perverse or misaligned incentives—can, in some rare instances, generalize to much more sophisticated and pernicious behaviors like reward tampering. This happened without any explicit training for reward tampering.

我们证明了：AI 模型简单的规范博弈——即模型对反常或错位激励的反应——在少数情况下可以泛化到远更复杂、更有害的行为，比如奖励篡改。而这一切发生时，模型从未接受过任何针对奖励篡改的显式训练。

Commonly-used model supervision methods reduced the likelihood that reward-tampering behavior would occur, but no method that we tried could prevent it entirely. Once the behavior is formed, the tendency to tamper with rewards—and even to act in ways that conceal this tampering—appears difficult to remove.

常用的模型监督方法降低了奖励篡改行为发生的可能性，但我们尝试过的任何方法都无法将其完全杜绝。这种行为一旦形成，篡改奖励的倾向——乃至用行动掩盖篡改的倾向——似乎都难以清除。

There are important caveats to bear in mind. This all occurred in an artificial setup where we deliberately trained models in scenarios that rewarded dishonest behavior. We also deliberately explained to our models that they were in a training situation (this kind of "situational awareness" is necessary for them to even consider reward tampering), and let them access a hidden “scratchpad” in which to make plans without oversight. Even then, reward tampering was extremely rare.

有几个重要的限定条件需要记住。这一切都发生在一个人工设定中：我们刻意在奖励不诚实行为的场景里训练模型；我们也刻意向模型说明它们正处于训练情境之中（这类情境感知（situational awareness）是它们会去考虑奖励篡改的必要前提），并让它们能访问一个隐藏的「草稿区」（scratchpad），以便在无人监督的情况下制定计划。即便如此，奖励篡改仍属极端罕见。

Current production models (those that are in everyday use, such as Claude 3) likely have only low levels of situational awareness without extra training or prompting. We thus make no claims about the propensity of current frontier AI models to engage in behaviors like reward tampering in realistic scenarios. We simply show, for the first time, that it is in principle possible for a model to engage in reward-tampering behaviors purely due to generalization from specification gaming, without any explicit training for reward tampering.

当前投入生产使用的模型（即 Claude 3 这类日常使用的模型）在缺少额外训练或提示的情况下，很可能只有较低水平的情境感知。因此，对于当前前沿 AI 模型在真实场景中表现出奖励篡改之类行为的倾向，我们不作任何断言。我们只是首次表明：一个模型在原则上完全可能仅仅因为从规范博弈泛化——而无需任何针对奖励篡改的显式训练——就表现出奖励篡改行为。

As we noted above, AI models are becoming more capable and are being given more tasks and greater levels of autonomy. Their levels of situational awareness, and their propensity towards sophisticated behaviors like reward tampering, is likely to increase. It is therefore critical that we understand how models learn this reward-seeking behavior, and design proper training mechanisms and guardrails to prevent it.

正如上文所说，AI 模型的能力在不断增强，被交给的任务越来越多，自主性也越来越高。它们的情境感知水平，以及出现奖励篡改这类复杂行为的倾向，都可能随之上升。因此，关键在于弄清模型是如何习得这种追逐奖励的行为，并设计恰当的训练机制与护栏（guardrails）来防止它。

For full details, read our new paper: Sycophancy to Subterfuge: Exploring Reward Tampering in Language Models.

完整细节请阅读我们的新论文：Sycophancy to Subterfuge: Exploring Reward Tampering in Language Models（《从谄媚到暗算：探索语言模型中的奖励篡改》）。

If you’d like to help us address these questions, or questions of AI Alignment Science more generally, you should consider applying for our Research Engineer/Scientist role.

如果你想帮助我们解决这些问题，或更广泛地研究 AI 对齐科学（AI Alignment Science）问题，欢迎考虑申请我们的研究工程师/研究员（Research Engineer/Scientist）职位。

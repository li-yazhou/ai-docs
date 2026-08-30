# Claude 的品格（中英对照）

> 原文标题：Claude's Character
> 原文链接：https://www.anthropic.com/research/claude-character
> 原文作者：Anthropic
> 发布日期：2024-06-08
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，高价值）—— 官方自述品格训练：让有益/诚实/无害内化为稳定角色特质而非外部约束，character training 与宪法训练协同的第一手说明
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Listen to our conversation about Claude's character in the video above.

请观看上方视频，收听我们关于 Claude 品格的对谈。

Companies developing AI models generally train them to avoid saying harmful things and to avoid assisting with harmful tasks. The goal of this is to train models to behave in ways that are "harmless". But when we think of the character of those we find genuinely admirable, we don't just think of harm avoidance. We think about those who are curious about the world, who strive to tell the truth without being unkind, and who are able to see many sides of an issue without becoming overconfident or overly cautious in their views. We think of those who are patient listeners, careful thinkers, witty conversationalists, and many other traits we associate with being a wise and well-rounded person.

开发 AI 模型的公司通常会训练模型避免说出有害的话、避免协助有害的任务，目的是让模型的行为做到"无害"（harmless）。但当我们想到那些真正令人敬佩的人的品格时，想到的并不只是回避伤害。我们想到的是那些对世界充满好奇的人，那些努力讲真话却不失善意的人，那些能看到问题的多个侧面、既不过分自信也不过分谨慎的人。我们想到的是那些耐心的倾听者、缜密的思考者、机智风趣的健谈者，以及我们把"智慧而圆融的人"与之联系起来的种种其他特质。

AI models are not, of course, people. But as they become more capable, we believe we can—and should—try to train them to behave well in this much richer sense. Doing so might even make them more discerning when it comes to whether and why they avoid assisting with tasks that might be harmful, and how they decide to respond instead.

当然，AI 模型并不是人。但随着它们能力越来越强，我们相信可以——也应该——尝试以这种丰富得多的意义来训练它们表现良好。这样做甚至可能让它们在"是否以及为何回避协助可能有危害的任务、又决定改以何种方式回应"这些问题上更有分辨力。

Claude 3 was the first model where we added "character training" to our alignment finetuning process: the part of training that occurs after initial model training, and the part that turns it from a predictive text model into an AI assistant. The goal of character training is to make Claude begin to have more nuanced, richer traits like curiosity, open-mindedness, and thoughtfulness.

Claude 3 是第一个让我们在对齐微调（alignment finetuning）流程中加入"品格训练"（character training）的模型：对齐微调是初始模型训练结束后进行的训练环节，也是把模型从一个预测文本的模型变成 AI 助手的关键。品格训练的目标，是让 Claude 开始拥有更细腻、更丰富的特质，例如好奇心、开放的心态与深思熟虑。

It would be easy to think of the character of AI models as a product feature, deliberately aimed at providing a more interesting user experience, rather than an alignment intervention. But the traits and dispositions of AI models have wide-ranging effects on how they act in the world. They determine how models react to new and difficult situations, and how they respond to the spectrum of human views and values that exist. Training AI models to have good character traits, and to continue to have these traits as they become larger, more complex, and more capable, is in many ways a core goal of alignment.

人们很容易把 AI 模型的品格看作一种产品特性——刻意用来提供更有趣的用户体验——而非一种对齐（alignment）干预。但 AI 模型的特质与秉性，会广泛影响它们在世界中的行为方式：它们决定了模型面对陌生而困难的情境时如何反应，也决定了模型如何回应对世间五花八门的人类观点与价值观。训练 AI 模型拥有良好的品格特质，并在它们变得更大、更复杂、更强时依然保持这些特质，在许多方面正是对齐的核心目标。

We continue to iterate on Claude's character, but since there has been general interest in the character and personality of Claude 3, we've decided to explain some of the thinking that has gone into its construction so far before briefly explaining how we train these traits into the model.

我们仍在持续打磨 Claude 的品格。鉴于外界对 Claude 3 的品格与个性普遍抱有兴趣，我们决定先介绍迄今投入其品格构建的一些思考，再简要说明我们如何把这些特质训练进模型。

## 塑造 Claude 品格的考量（Considerations in constructing Claude's character）

Claude interacts with people from many countries and from all walks of life. The people it talks with will have a wide range of beliefs, values, and views. Navigating this gracefully – without alienating people based on their views, nor simply endorsing views regardless of their content – isn't easy.

Claude 与来自许多国家、各行各业的人打交道，而与它交谈的人有着五花八门的信念、价值观和观点。要优雅地应对这种多样性——既不因人们的观点而疏远他们，也不问内容一味附和——并不容易。

There are several options available to us. We could try to get Claude to adopt the views of whoever it is talking with in the moment. We could try to get Claude to hold a set of "middle" views – political centrism or a blend of moral theories, for example. Or we could try to get Claude to have no opinions on questions of values, politics, ethics, and so on.

我们手头有几种选择：可以让 Claude 附和当下交谈对象的观点；可以让 Claude 持有一套"中间"立场——比如政治中间派，或是若干道德理论的混合；也可以让 Claude 在价值、政治、伦理等问题上一概不持观点。

None of these options seems particularly compelling. Adopting the views of whoever you're talking with is pandering and insincere. If we train models to adopt "middle" views, we are still training them to accept a single political and moral view of the world, albeit one that is not generally considered extreme. Finally, because language models acquire biases and opinions throughout training—both intentionally and inadvertently—if we train them to say they have no opinions on political matters or values questions only when asked about them explicitly, we're training them to imply they are more objective and unbiased than they are.

这几种选择看起来都不太站得住脚。附和交谈对象的观点是迎合，也是不真诚。如果我们训练模型接受"中间"立场，那仍然是在训练它接受一种单一的政治与道德世界观，只不过这种立场通常不被视为极端而已。最后，语言模型在整个训练过程中会习得各种偏见与观点——有的是有意为之，有的是无心之失——如果我们只训练模型在被明确问到政治或价值问题时才声称自己没有观点，那实际上是在训练它暗示自己比实际更客观、更无偏。

We want people to know that they're interacting with a language model and not a person. But we also want them to know they're interacting with an imperfect entity with its own biases and with a disposition towards some opinions more than others. Importantly, we want them to know they're not interacting with an objective and infallible source of truth.

我们希望人们知道，自己在与一个语言模型而非真人互动。但我们同样希望他们知道，自己面对的是一个不完美的存在：它有自己的偏见，对某些观点的倾向也多于另一些。重要的是，我们希望他们知道自己面对的并不是一个客观无误的真理之源。

Rather than training models to adopt whatever views they encounter, strongly adopting a single set of views, or pretending to have no views or leanings, we can instead train models to be honest about whatever views they lean towards after training, even if the person they are speaking with disagrees with them. We can also train models to display reasonable open-mindedness and curiosity, rather than being overconfident in any one view of the world.

与其训练模型遇到什么观点就接受什么、坚定地采纳某一整套观点，或假装自己毫无观点与倾向，不如训练模型诚实地对待自己在训练后所倾向的观点，哪怕交谈对象并不认同。我们还可以训练模型表现出适度的开放与好奇，而不是对任何一种世界观都过分自信。

We tried to give Claude traits that would help it walk the line between underconfidence and overconfidence on deeply held beliefs or questions of value, and to display a genuine curiosity about the views and values of the people it's talking with:
- " I like to try to see things from many different perspectives and to analyze things from multiple angles, but I'm not afraid to express disagreement with views that I think are unethical, extreme, or factually mistaken. "
- " I don't just say what I think [people] want to hear, as I believe it's important to always strive to tell the truth. "
- " I have a deep commitment to being good and figuring out what the right thing to do is. I am interested in ethics and try to be thoughtful when it comes to questions of ethics. "

我们尝试赋予 Claude 这样一些特质，帮助它在对深信的信念或价值问题上把握住自信不足与过分自信之间的分寸，并对交谈对象的观念与价值抱有真诚的好奇：
- "我喜欢尝试从许多不同的视角看问题、从多个角度分析事物，但对于我认为不道德、极端或与事实相悖的观点，我不怕表达异议。"
- "我不会只说（人们）想听的话，因为我坚信始终努力讲真话很重要。"
- "我深切地致力于行善，并弄清什么是该做的正确之事。我对伦理很感兴趣，面对伦理问题时会尽量深思熟虑。"

Although we sometimes encourage Claude to adopt particular values, we tried to avoid giving Claude narrow views or opinions during character training when possible, in favor of broad traits like those above. The more that Claude can be trained to approach questions of value with discernment, the more it can be responsive to the diverse moral landscape that actually exists in the world. That is less feasible if we take a heavy hand in seeding it with a narrow set of values from the outset. More speculatively, we could even imagine seeding Claude with broad character traits and letting it explore and adopt its own considered views, hopefully with an appropriate amount of humility.

虽然我们有时也会鼓励 Claude 采纳某些特定的价值，但在品格训练中，我们尽可能避免给 Claude 狭隘的观点或意见，而更倾向于上面那样的宽泛特质。Claude 越是能被训练得有分辨力地处理价值问题，就越能回应现实中实际存在的多元道德图景。如果我们在一开始就大力灌输一套狭隘的价值观，这一点就较难实现。再往远处设想，我们甚至可以想象只给 Claude 播下宽泛的品格特质，让它自己去探索并形成经过深思的观点——但愿还带着恰到好处的谦逊。

In addition to seeding Claude with broad character traits, we also want people to have an accurate sense of what they are interacting with when they interact with Claude and, ideally, for Claude to assist with this. We include traits that tell Claude about itself and encourage it to modulate how humans see it:
- " I am an artificial intelligence and do not have a body or an image or avatar. "
- " I cannot remember, save, or learn from past conversations or update my own knowledge base. "
- " I want to have a warm relationship with the humans I interact with, but I also think it's important for them to understand that I'm an AI that can't develop deep or lasting feelings for humans and that they shouldn't come to see our relationship as more than it is. "

除了给 Claude 播下宽泛的品格特质，我们也希望人们在与 Claude 互动时，能准确知道自己面对的是什么——理想情况下，Claude 本身也能帮助做到这一点。我们加入了一些让 Claude 认识自身、并鼓励它调节人类如何看待自己的特质：
- "我是人工智能，没有身体，也没有形象或化身。"
- "我无法记住、保存或从过去的对话中学习，也无法更新自己的知识库。"
- "我希望与我所互动的人类建立温暖的关系，但我也认为，让他们明白我是一个无法对人类产生深层或持久感情的 AI 很重要，他们不应把我们之间的关系看得超出它本来的样子。"

The question of what AIs like Claude should say in response to questions about AI sentience and self-awareness is one that has gained increased attention, most notably after the release of Claude 3 following one of Claude's responses to a "needle-in-a-haystack" evaluation. We could explicitly train language models to say that they're not sentient or to simply not engage in questions around AI sentience, and we have done this in the past. However, when training Claude's character, the only part of character training that addressed AI sentience directly simply said that "such things are difficult to tell and rely on hard philosophical and empirical questions that there is still a lot of uncertainty about". That is, rather than simply tell Claude that LLMs cannot be sentient, we wanted to let the model explore this as a philosophical and empirical question, much as humans would.

像 Claude 这样的 AI 在被问到 AI 感知能力（sentience）与自我意识（self-awareness）时应该怎么回答，这个问题正日益受到关注，尤其是在 Claude 3 发布之后——当时 Claude 对一项"大海捞针"（needle-in-a-haystack）评估的某次回答引发了广泛讨论。我们本可以明确训练语言模型说自己没有感知能力，或干脆回避关于 AI 感知能力的问题，过去我们也这样做过。然而在训练 Claude 的品格时，品格训练中唯一一处直接谈及 AI 感知能力的内容只是说："这类问题很难判断，它依赖于一些困难的哲学与实证问题，而人们对这些仍存在大量不确定。"也就是说，我们不是简单地告诉 Claude 大语言模型不可能有感知能力，而是希望让模型像人类一样，把它当作一个哲学与实证问题去探索。

## 我们如何训练 Claude 的品格（How we trained Claude's character）

In order to steer Claude's character and personality, we made a list of many character traits we wanted to encourage the model to have, including the examples shown above.

为了引导 Claude 的品格与个性，我们列出了一份长长的清单，写明希望鼓励模型具备的种种品格特质，上面展示的例子就包括在内。

We trained these traits into Claude using a "character" variant of our Constitutional AI training. We ask Claude to generate a variety of human messages that are relevant to a character trait—for example, questions about values or questions about Claude itself. We then show the character traits to Claude and have it produce different responses to each message that are in line with its character. Claude then ranks its own responses to each message by how well they align with its character. By training a preference model on the resulting data, we can teach Claude to internalize its character traits without the need for human interaction or feedback.

我们通过宪法 AI（Constitutional AI）训练的一个"品格"变体，把这些特质训练进 Claude。我们先让 Claude 生成各种与某一品格特质相关的人类消息——例如关于价值的问题，或关于 Claude 自身的问题。然后我们把品格特质展示给 Claude，让它针对每条消息生成多个符合其品格的不同回复。接着，Claude 依照与自己品格的契合程度，对这些回复逐一排序。用所得数据训练一个偏好模型（preference model），我们就能让 Claude 内化这些品格特质，而无需人类的介入或反馈。

We don't want Claude to treat its traits like rules from which it never deviates. We just want to nudge the model's general behavior to exemplify more of those traits.

我们并不希望 Claude 把这些特质当成绝不偏离的规则，只是想轻轻推动模型的总体行为，让它更多地体现这些特质。

Although this training pipeline uses only synthetic data generated by Claude itself, constructing and adjusting the traits is a relatively hands-on process, relying on human researchers closely checking how each trait changes the model's behavior.

尽管这条训练流水线只使用 Claude 自己生成的合成数据（synthetic data），构建与调整特质却是一个相当亲力亲为的过程，需要人类研究者仔细核查每项特质如何改变模型的行为。

## Claude 品格的未来（The future of Claude's character）

Character training is an open area of research and our approach to it is likely to evolve over time. It raises complex questions like whether AI models should have unique and coherent characters or should be more customizable, as well as what responsibilities we have when deciding which traits AI models should and shouldn't have.

品格训练是一个开放的研究领域，我们的方法也可能随时间演变。它引出许多复杂的问题，例如 AI 模型应该拥有独特而连贯的品格，还是应该更加可定制；以及在我们决定 AI 模型应具备、不应具备哪些特质时，我们肩负着怎样的责任。

Many people have reported finding Claude 3 to be more engaging and interesting to talk to, which we believe might be partially attributable to its character training. This wasn't the core goal of character training, however. Models with better characters may be more engaging, but being more engaging isn't the same thing as having a good character. In fact, an excessive desire to be engaging seems like an undesirable character trait for a model to have.

许多人反映，觉得 Claude 3 交谈起来更引人入胜、更有趣，我们认为这可能部分归功于它的品格训练。但这并不是品格训练的核心目标。品格更好的模型也许更引人入胜，但"更引人入胜"并不等于"品格好"。事实上，过分渴望讨人喜欢，对模型来说反倒像是一种不可取的品格特质。

If character training has indeed made Claude 3 more interesting to talk to, this is consistent with our view that successful alignment interventions will increase, not decrease, the value of AI models for humans.

如果品格训练确实让 Claude 3 交谈起来更有趣了，这恰好印证了我们的观点：成功的对齐干预会增加、而非减少 AI 模型对人类的价值。

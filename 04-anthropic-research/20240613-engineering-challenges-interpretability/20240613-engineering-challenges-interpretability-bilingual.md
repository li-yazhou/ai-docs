# 可解释性规模化的工程挑战（中英对照）

> 原文标题：The engineering challenges of scaling interpretability
> 原文链接：https://www.anthropic.com/research/engineering-challenges-interpretability
> 原文作者：Anthropic（Interpretability 团队）
> 发布日期：2024-06-13
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，值得一读）—— 把稀疏自编码器训练到生产级模型规模的工程账：算力、显存带宽、数据管线与分布式训练的经验清单
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

In this post, and in the above roundtable video, our researchers reflect on the close relationship between scientific and engineering progress, and discuss the technical challenges they encountered in scaling our interpretability research to much larger AI models.

在这篇文章以及上面的圆桌视频（roundtable video）中，我们的研究者们思考了科学进步与工程进步之间的紧密关系，并讨论了在把我们的可解释性（interpretability）研究扩展到规模大得多的 AI 模型时，他们所遇到的技术挑战。

Last October, the Anthropic Interpretability team published Towards Monosemanticity, a paper applying the technique of dictionary learning to a small transformer model. In May this year, we published Scaling Monosemanticity, where we applied the same technique to a model several orders of magnitude larger. We found tens of millions of “features”—combinations of neurons that relate to semantic concepts—in Claude 3 Sonnet, representing an important step forward in understanding the inner workings of AI models.

去年十月，Anthropic 可解释性团队发表了《Towards Monosemanticity》，这篇论文将字典学习（dictionary learning）技术应用于一个小型 transformer 模型。今年五月，我们发表了《Scaling Monosemanticity》，把同样的技术应用到了大了几个数量级的模型上。我们在 Claude 3 Sonnet 中找到了数千万个「特征」（features）——即与语义概念相关联的神经元组合——这是理解 AI 模型内部运作向前迈出的重要一步。

To continue making this progress, we need more engineers.

要想延续这样的进展，我们需要更多工程师。

This might seem surprising if you've only read our early papers (for example Frameworks and Toy Models of Superposition), which required relatively little engineering. But reading the newer research should make clear the scale of the engineering challenge we face.

如果你只读过我们的早期论文（例如《Frameworks and Toy Models of Superposition》），这乍看之下可能令人意外——那些工作所需的工程量相对不大。但读一读更新的研究，你应该就能清楚看到我们所面对的工程挑战有多大的规模。

Below, we share two examples of the technical engineering questions that were involved in our latest research. These illustrate the kinds of problems our engineers are tackling right now, and help explain why we think engineering will be one of the major bottlenecks to progress in AI interpretability—and ultimately, AI safety—research.

下面，我们分享两个例子，展示我们最新研究中涉及的技术工程问题。它们说明了我们的工程师当下正在解决的是哪一类问题，也有助于解释为什么我们认为，工程将成为 AI 可解释性研究——并最终包括 AI 安全研究——取得进展的主要瓶颈之一。

If you're an engineer, this post is aimed at you. If you’re inspired by the examples of engineering problems discussed below, we strongly encourage you to apply for our Research Engineer role.

如果你是一名工程师，这篇文章正是写给你的。如果下面讨论的这些工程问题让你心生向往，我们强烈鼓励你申请我们的研究工程师（Research Engineer）职位。

## 工程问题 1：分布式洗牌（Engineering Problem 1: Distributed Shuffle）

Our Sparse Autoencoders—the tools we use to investigate “features”—are trained on the activations of transformers, and those activations need to be shuffled to stop them from learning spurious, order-dependent patterns. When we first started training sparse autoencoders, we could fit our training data on a single GPU and trivially shuffle it. But eventually, we wanted to scale beyond what could fit in memory (imagine starting with the easy task of shuffling a deck of cards, but then scaling it up to shuffling entire warehouses full of cards — it’s a much more difficult problem).

我们的稀疏自编码器（sparse autoencoders）——也就是我们用来研究「特征」的工具——是在 transformer 的激活值（activations）上训练的，而这些激活值需要先洗牌（shuffle），以防止它们学到虚假的、依赖于顺序的模式。刚开始训练稀疏自编码器时，训练数据可以整个装进单张 GPU，洗牌轻而易举。但最终，我们想把规模扩大到超出内存所能容纳的程度（想象一下，从「洗一副扑克牌」这个简单任务，扩大到「洗整座仓库堆满的扑克牌」——难度完全是另一回事）。

At this point, we could have implemented a distributed shuffle that scaled to petabytes. Instead, we decided on an approach we could implement quickly, but which didn't scale as well. We split our shuffle into K jobs where each job was responsible for 1/K of the shuffled output data. We generated a permutation, had each job do a streaming read of all of the training data, and then had it write out its share of the output. This allowed us to scale further, but the downside was obvious: each job had to read all of the training data. This first took hours, and later took days. By the time we were working on Towards Monosemanticity, we had 100TB of training data (100 billion data points, each being 1KB) and shuffling had become a major headache.

在当时，我们本可以实现一个能扩展到 PB 级的分布式洗牌，但我们却选了一个能快速实现、扩展性却没那么好的方案：把洗牌拆成 K 个作业（job），每个作业负责洗牌后输出数据的 1/K。我们生成一个随机置换（permutation），让每个作业以流式读取（streaming read）的方式读完全部训练数据，然后写出属于它的那一份输出。这让我们得以继续扩展规模，但缺点显而易见：每个作业都得把全部训练数据读一遍。起初这要花几个小时，后来要花上几天。到我们做《Towards Monosemanticity》的时候，训练数据已有 100TB（1000 亿个数据点，每个 1KB），洗牌成了一个大麻烦。

Performing a distributed shuffle that scales isn’t a novel or cutting-edge problem. But it was just one of many engineering problems we had to solve quickly to make scientific progress.

实现一个可扩展的分布式洗牌，并不是什么新颖或前沿的问题。但它只是我们为推进科学研究而必须快速解决的众多工程问题之一。

In this case, we found a helpful blog post and extended the approach to many passes. For one pass, we have N jobs. Each job reads 1/N of the dataset, shuffles it, and writes out the data in K files each with 1/NK of the data. The contents of the first file written from each job represent the first 1/K of the final shuffled data, but it still needs to be shuffled. It’s the same for the second file, and so on. In one pass, we have reduced one shuffle of all the data to N shuffles, each K times smaller. Now, if the shuffles fit in memory on a single machine, we can shuffle it and we’re done. If they don’t fit, we can just run another pass.

在这个问题上，我们找到了一篇很有帮助的博客文章，并把其中的方法扩展到多轮（pass）处理。单轮处理中有 N 个作业，每个作业读取数据集的 1/N，将其洗牌，再写出 K 个文件、每个文件含 1/NK 的数据。各作业写出的第一个文件的内容，合起来就是最终洗牌结果的前 1/K，但它还需要再洗一次；第二个文件同理，依此类推。经过一轮处理，我们就把「对全部数据洗一次牌」化简为 N 次洗牌，每次的数据量只有原来的 1/K。这时，如果这些洗牌能在单机的内存里装下，我们直接洗完即可大功告成；如果装不下，就再跑一轮。

Let’s say each job can keep 100GB of data in memory, and we write one hundred 1GB files. Each pass reduces the size of the shuffles needed by 100 times. One pass can shuffle 100GB of data, two passes can shuffle 10TB, three passes 1PB, four passes 100PB, and so on.

假设每个作业能在内存中保留 100GB 数据，我们写出 100 个 1GB 的文件。每多一轮，所需洗牌的数据规模就缩小 100 倍：一轮可以洗 100GB 数据，两轮可以洗 10TB，三轮 1PB，四轮 100PB，以此类推。

Since we implemented this approach, we’ve stopped thinking about shuffling. Now it’s something that happens quickly, without issues. There are certainly better approaches and faster implementations than ours. But this approach solves our bottleneck, and frees us up to tackle the next problem.

自从实现了这个方案，我们就再也不用为洗牌操心了。如今它运行得又快又稳。肯定有比我们更好的方法和更快的实现，但这个方案解决了我们的瓶颈，让我们能腾出手去解决下一个问题。

## 工程问题 2：特征可视化流水线（Engineering Problem 2: Feature Visualization Pipeline）

Another engineering challenge has been generating the underlying data for our feature visualizations, which allow users to see specific tokens that are most strongly activated as part of individual features, along with other information (see the Feature Browser from the Towards Monosemanticity paper at this link).

另一项工程挑战是为我们的特征可视化（feature visualization）生成底层数据——特征可视化让用户能看到作为单个特征一部分、激活最强的那些具体 token，以及其他相关信息（可通过这个链接查看《Towards Monosemanticity》论文的特征浏览器 Feature Browser）。

For each feature, we want to find a variety of dataset examples that activate it to different levels, exploring its full distribution. Doing this efficiently for millions of features is an interesting distributed systems problem. Originally, all of this ran in a single job – but we quickly scaled beyond that. Below is a sketch of our current approach.

对每个特征，我们都想找到各种各样的数据集样本，让它在不同程度上被激活，从而探索其完整的分布。要为数百万个特征高效地做到这一点，是一个有趣的分布式系统问题。最初，这一切都跑在单个作业里——但我们很快就超出了单作业能承载的规模。下面是我们当前做法的简要示意。

Our dataset for visualization is 100M tokens, and we need to handle millions of features. First we “shard” over the dataset and features, splitting them into many different parts. Each job iterates over its slice of the dataset and, for its slice of features, keeps track of the K highest activating tokens for each feature and 10*K random tokens that activate the feature (we have already cached the transformer activations in s3, so we don’t need to recompute them).

我们用于可视化的数据集是 1 亿（100M）个 token，而需要处理的特征有数百万个。首先，我们按数据集和特征两个维度做「分片」（shard），把它们切分成许多不同的部分。每个作业遍历它分到的那份数据集切片，并针对它分到的特征切片，为每个特征记录激活最高的 K 个 token，以及 10*K 个激活了该特征的随机 token（transformer 激活值已经缓存在 s3 里，所以无需重新计算）。

Next, we shard over the features and aggregate the results from the previous pass. This gives us the highest-activating tokens for each feature across the entire dataset, as well as a random set of tokens that activate the feature. These are the examples we’ll show in the feature visualization.

接下来，我们按特征维度分片，汇总上一轮的结果。这样我们就得到了每个特征在整个数据集上激活最高的 token，以及一组激活该特征的随机 token。这些就是我们将要在特征可视化中展示的样本。

For each of these examples, we need to calculate how the feature fires on surrounding tokens. Our first approach sharded over features. Each job loads the transformer activations for the examples of the features for which it’s responsible. The problem is that these examples are randomly distributed across the dataset: there’s no easy way to read only the data the job needs.

对其中每个样本，我们都需要计算该特征在其周围 token 上的触发情况。我们最初的做法是按特征分片：每个作业加载它所负责的那些特征的样本所对应的 transformer 激活值。问题在于，这些样本是随机散布在整个数据集中的——没有什么简单的办法只读取该作业需要的那部分数据。

To improve this, we added a pass sharded over the dataset. In this setup, each job handles a slice of transformer activations, and saves the activations needed for each group of features to a separate file. Then, we can run a pass over features and have easy access to just the data we need. We compute how much the feature fires on surrounding tokens, then write out all the relevant data in a format our frontend website can read and display.

为了改进这一点，我们增加了一轮按数据集分片的处理。在这种设置下，每个作业处理一段 transformer 激活值，并把每组特征所需的激活值保存到单独的文件里。之后，我们就能按特征再跑一轮，轻松访问到恰好所需的数据。我们计算出特征在周围 token 上的触发强度，然后把所有相关数据以我们的前端网站能够读取和展示的格式写出。

## 我们在寻找什么（What we’re looking for）

When we started working on Sparse Autoencoders, we didn’t know if the approach would work. The more experiments we ran, the more confident we became in our research. That led us to invest more in our infrastructure so we could run larger experiments. The process continued all the way to Scaling Monosemanticity, our most recent paper.

当我们开始研究稀疏自编码器时，并不确定这条路能否走通。实验做得越多，我们对这项研究的信心就越足，这促使我们在基础设施上投入更多，从而能跑更大的实验。这一过程一直延续到我们最新的论文《Scaling Monosemanticity》。

That process points to the type of engineering work we do on the Interpretability team — and the fact that we consider research and engineering to be inseparable. Often, our team members will switch back and forth between research and engineering, squeezing more scale out of our current system to launch a new experiment before returning to the research. Since many research ideas don’t work out, we don’t invest more heavily in their infrastructure until we see success.

这一过程正说明了可解释性团队所做工程工作的类型——也说明了我们把研究与工程视为密不可分这一点。我们的团队成员常常在研究与工程之间来回切换：先从现有系统里压榨出更多规模、把一个新实验跑起来，然后再回到研究上来。由于很多研究想法最终并不成功，在看到成功之前，我们不会在它们的基础设施上重金投入。

Research is a team effort, and it's as much about implementing ideas as it is ruminating on them. We don’t just hypothesize; we test, build, iterate, and scale.

研究是团队协作的产物，它既关乎反复琢磨想法，也同样关乎把想法实现出来。我们不只是提出假设；我们测试、构建、迭代、规模化。

Because of this, we’re particularly interested in hiring generalist engineers who are able to work flexibly across different domains — whether that’s building pipelines, running ML experiments, or optimizing GPU usage.

正因如此，我们尤其希望招到能够灵活横跨不同领域的通才型工程师（generalist engineers）——无论是搭建数据流水线、跑机器学习实验，还是优化 GPU 使用。

If you’re an engineer who fits this bill, and who is passionate about AI safety, we’d love to see your application. Take a look at the job description for our Research Engineer role, and at our Careers page for several other open roles on the Interpretability team.

如果你正是这样的工程师，并且对 AI 安全满怀热忱，我们非常乐意看到你的申请。欢迎查看我们研究工程师职位的职位描述，以及我们招聘页面（Careers page）上可解释性团队的其他若干在招岗位。

## 常见问题（FAQ）

- How many roles are you hiring for? There are currently 18 members of the Interpretability team, and we're growing quickly. Currently we’re looking to hire at least five senior engineers and two team managers, across multiple locations. See our Careers page for the listings.
- Does the Interpretability team work with other teams? Yes. We collaborate strongly with our other research teams (especially the Alignment team). Anthropic is a “team-science” org, and the borders between teams are porous in the best ways — this allows us to get a lot done quickly.
- How do you choose projects to work on? We think about our research roadmap in terms of Anthropic’s Responsible Scaling Policy, which commits the company to hitting various safety milestones before developing or deploying models above corresponding capability levels. In thinking about research directions, we consider what the research landscape of interpretability looks like, what problems we’re in a position to address, and how the Interpretability team’s work could impact those safety milestones. On a day-to-day level, our research tends to be very exploratory, but it’s an exploration that’s guided by the above considerations.
- What kinds of backgrounds do people on the Interpretability team have? People have come to the Interpretability team from a wide range of professional backgrounds, including neuroscience, mathematics, biology, physics, data visualization, and software engineering.
- Are you open to candidates outside of the Bay Area? The team currently has members in San Francisco, Boston, New York, Seattle, and London. The largest concentration is in San Francisco, and those members come into the office several days a week. We are open to remote working, with a requirement to visit an Anthropic office about 25% of the year. See our Careers page for more information about all our open positions.

- 你们在招聘多少个岗位？可解释性团队目前有 18 名成员，而且扩张很快。当前我们计划在多个办公地点招聘至少 5 名资深工程师和 2 名团队管理者。招聘列表见我们的招聘页面。
- 可解释性团队与其他团队合作吗？会。我们与其他研究团队（尤其是对齐/Alignment 团队）展开紧密合作。Anthropic 是一个「团队化科研」（team-science）型组织，团队之间的边界以最好的方式保持着通透——这让我们能快速完成大量工作。
- 你们如何选择要做的项目？我们以 Anthropic 的负责任扩展政策（Responsible Scaling Policy）为框架来思考研究路线图——该政策承诺公司在开发或部署超出相应能力等级的模型之前，必须先达成各项安全里程碑。在思考研究方向时，我们会考虑可解释性的研究图景是怎样的、我们具备条件去解决哪些问题，以及可解释性团队的工作可能如何影响那些安全里程碑。在日常层面，我们的研究往往非常偏探索性，但这种探索是在上述考量的指引下进行的。
- 可解释性团队的成员都有什么样的背景？团队成员来自五花八门的职业背景，包括神经科学、数学、生物学、物理学、数据可视化和软件工程。
- 你们接受湾区以外的候选人吗？团队目前有成员分布在旧金山、波士顿、纽约、西雅图和伦敦。人数最集中的是旧金山，那里的成员每周有几天到办公室办公。我们接受远程办公，但要求一年中大约 25% 的时间到 Anthropic 办公室现场。所有在招岗位的更多信息见我们的招聘页面。

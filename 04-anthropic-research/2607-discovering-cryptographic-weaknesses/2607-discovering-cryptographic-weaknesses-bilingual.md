# 用 Claude 发现密码学弱点（中英对照）

> 原文标题：Discovering cryptographic weaknesses with Claude
> 原文链接：https://www.anthropic.com/research/discovering-cryptographic-weaknesses
> 原文作者：Anthropic
> 发布日期：2026-07-28
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— 前沿模型在密码分析上达到顶级专家水平的首批硬结果：HAWK 后量子签名有效密钥强度减半、7 轮 AES 攻击提速 200–800 倍
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体；脚注以 Obsidian 脚注形式保留于文末。指数以 Unicode 上标表示（如 2⁶⁴）。

---

## 摘要（Summary）

Using Claude Mythos Preview, researchers at Anthropic have discovered improved ways to attack cryptographic algorithms (the mathematical methods used to keep online data private). The first attack significantly weakens HAWK, a digital signature scheme that was built for a post-quantum world. The second identifies a new way to attack round-reduced AES, the most widely used symmetric cipher. These are substantial research advances, but they do not currently affect any production systems. This post describes both findings in more detail and discusses the implications for cryptography in an age of powerful AI models.

Anthropic 的研究者利用 Claude Mythos Preview 发现了攻击密码算法（用于保护在线数据隐私的数学方法）的改进途径。第一个攻击显著削弱了 HAWK——一个为后量子时代设计的数字签名方案；第二个发现了一种攻击轮次削减版 AES（最广泛使用的对称密码）的新方法。这些是实质性的研究进展，但目前不影响任何生产系统。本文更详细地描述这两项发现，并讨论强大 AI 模型时代对密码学的影响。

## 引言（Introduction）

When we launched Claude Mythos Preview, we showed it was able to autonomously find and exploit vulnerabilities in almost every piece of software we pointed it at. This included several major cryptographic libraries—shared collections of code that are used to encrypt data.

在发布 Claude Mythos Preview 时，我们曾展示它能够自主找出并利用我们指向的几乎所有软件中的漏洞。这包括几个主要的密码学库——用于加密数据的共享代码集合。

The vulnerabilities that Claude found in these cryptographic libraries[^1] were due to incorrect implementation of the algorithms—that is, errors in how programmers used the algorithms in their code that created opportunities for attackers to break the encryption.

Claude 在这些密码学库中发现的漏洞[^1]源于算法的错误实现——即程序员在代码中使用算法的方式出错，给攻击者创造了破解加密的机会。

Now, we have found that Claude is able to find mathematical flaws in the algorithms themselves.

现在，我们发现 Claude 能够在算法本身之中找出数学缺陷。

Cryptographic algorithms are a fundamental building block of digital security. For example, when you visit a webpage like https://www.anthropic.com, your browser checks that it is communicating with an authentic website using an algorithm called a digital signature scheme. Later, the traffic between you and the website is encrypted using symmetric ciphers—codes that allow secure data transmission between parties who share an identical key. Without secure cryptographic systems like these, your email, online banking, and other internet use would be open to cybercriminals, who could intercept or modify your communications. Flaws in these widely used cryptographic systems could put billions of users' data at risk.

密码算法是数字安全的基本构件。例如，当你访问 https://www.anthropic.com 这样的网页时，你的浏览器会用一种称为数字签名方案（digital signature scheme）的算法验证它正在与真实的网站通信；随后，你与网站之间的流量用对称密码（symmetric cipher）加密——这种编码让共享同一密钥的双方能够安全传输数据。没有这类安全的密码系统，你的电子邮件、网上银行和其他互联网活动将向网络犯罪分子敞开——他们可以拦截或篡改你的通信。这些被广泛使用的密码系统若存在缺陷，数十亿用户的数据都可能陷于风险。

The first result we describe in this post, which was discovered with Claude Mythos Preview, is an improved attack against a digital signature scheme called HAWK. In 2022, the US Government's National Institute of Standards and Technology (NIST) put out a call for additional cryptographic systems that would remain secure even against quantum computers (which could, if developed, break most of the existing signature schemes in use today). HAWK is one of the third-round candidates under consideration from this call. Despite HAWK having survived two rounds of expert human review over a period of two years, Mythos was able to improve the best-known attack on it in just 60 hours of work—effectively cutting its key strength in half.

本文描述的第一个结果由 Claude Mythos Preview 发现：对一种名为 HAWK 的数字签名方案的改进攻击。2022 年，美国政府国家标准与技术研究院（NIST）征集能在量子计算机面前保持安全的补充密码系统（量子计算机一旦建成，可打破当今使用的大多数现有签名方案）。HAWK 是该征集仍在考虑中的第三轮候选之一。尽管 HAWK 在两年间挺过了两轮人类专家评审，Mythos 仅用 60 小时的工作就改进了针对它的已知最佳攻击——实际上把它的密钥强度砍掉了一半。

The second result concerns the Advanced Encryption Standard (AES), a symmetric cipher that was adopted by NIST in 2001 and has received more scrutiny than almost any other encryption algorithm. In order to better understand the robustness of AES, weaker variations of the algorithm are regularly studied in cryptography research; Mythos found a way to break one such weaker version, and eliminated one of the guesses an attacker needs to make, improving the speed of the previous best attacks by 200-800×.

第二个结果关于高级加密标准（AES）——2001 年被 NIST 采纳、几乎比任何其他加密算法都受过更多审视的对称密码。为了更好地理解 AES 的稳健性，密码学研究常定期研究该算法较弱的变体；Mythos 找到了攻破其中一个较弱版本的方法，消除了攻击者需要做的一个猜测，把此前最佳攻击的速度提高了 200–800 倍。

To be clear, neither of these results has a practical impact on today's computer systems; no production software will have to change as a result. HAWK is only a candidate signature scheme and so is not deployed;[^2] our second attack is on a reduced version of AES and does not break the full cipher.[^3]

需要说明：这两项结果对今天的计算机系统都没有实际影响；不需要为此改动任何生产软件。HAWK 只是一个候选签名方案，尚未部署；[^2]我们的第二个攻击针对的是 AES 的削减版本，并未攻破完整密码。[^3]

Nevertheless, both results show the potential for frontier AI models to help discover flaws in important cryptographic algorithms, both before and after real-world deployment. This is cryptography research working as intended: stress-testing algorithms to build trust and ultimately make systems more secure.

尽管如此，两项结果都展示了前沿 AI 模型在真实部署前后帮助发现重要密码算法缺陷的潜力。这正是密码学研究应有的样子：对算法做压力测试，建立信任，最终让系统更安全。

Mythos Preview achieved these results mostly autonomously and mostly without human intervention. Over the course of a week, one Anthropic researcher worked together with Claude to develop the HAWK attack, and another researcher built a scaffold[^4] that allowed Claude to fully autonomously discover the AES attack.[^5] Each of the results cost roughly $100,000 in API cost to develop. After seeing these results, we broadened our search and began to discover other attacks. We discuss some of these follow-ups below.

Mythos Preview 取得这些结果时基本是自主的，基本没有人类干预。一周之内，一位 Anthropic 研究者与 Claude 协作开发出 HAWK 攻击；另一位研究者搭建了一个支架（scaffold）[^4]，让 Claude 完全自主地发现了 AES 攻击。[^5]两项结果的开发成本各约 10 万美元 API 费用。看到这些结果后，我们扩大了搜索范围，开始发现其他攻击。下文讨论其中一些后续工作。

In order to make it easier for others to continue studying the cryptanalytic ability of LLMs, we partnered with academics at ETH Zurich, Tel Aviv University, and TU Berlin to build CryptanalysisBench, a benchmark that packages together many cryptographic ciphers and makes it easy for others to evaluate the capabilities of LLMs on this important topic.

为了让他人更方便地继续研究 LLM 的密码分析能力，我们与苏黎世联邦理工、特拉维夫大学与柏林工业大学的研究者合作构建了 CryptanalysisBench——一个把多种密码打包在一起的基准，让他人能轻松评估 LLM 在这个重要主题上的能力。

Throughout the research process, we followed responsible disclosure procedures, and consulted with academics to confirm the validity of our findings. We also shared advance copies with US government and industry partners, and held discussions on the implications of this research. In the case of our HAWK finding, we shared our attack with the authors of HAWK in June and coordinated disclosure to the public NIST mailing list at the same time our results were released.

在整个研究过程中，我们遵循负责任披露（responsible disclosure）流程，并咨询学界以确认发现的效力。我们还向美国政府与行业伙伴提前分享了文稿，并就这项研究的含义展开讨论。就 HAWK 而言，我们于 6 月把攻击分享给 HAWK 作者，并在结果公开发布的同一时间协调向公开的 NIST 邮件列表披露。

In the rest of this post, we summarize the two findings in further technical detail and briefly describe some of our other recent cryptography results. Full descriptions of the two main findings are provided in two new papers, and we hope to release details for our other findings in the near future.

在本文余下部分，我们进一步以技术细节总结这两项发现，并简要描述我们近期其他一些密码学结果。两项主要发现的完整描述见两篇新论文，我们希望在不远的将来公布其他发现的细节。

## 针对 HAWK 的改进密钥恢复攻击（An improved key recovery attack on HAWK）

Working with Mythos Preview, an Anthropic researcher developed an attack against the HAWK post-quantum digital signature scheme. This attack substantially speeds up the time it would take to break the signature scheme—more technically, it reduces the "effective keysize" by a factor of two. In our paper, we provide the full technical details of our result including demonstration code that shows our attack running.

一位 Anthropic 研究者与 Mythos Preview 协作，开发出针对 HAWK 后量子数字签名方案的攻击。该攻击大幅加快了破解该签名方案所需的时间——更技术地说，它把"有效密钥长度"削减了一半。完整技术细节见我们的论文（链接见原文），其中包括展示攻击运行的演示代码。

HAWK is one of the remaining third round candidates of the NIST call for Additional Digital Signatures. This contest is part of a near decade-long effort to standardize new Post-Quantum Cryptographic (PQC) schemes. This standardization effort is becoming critical as the horizon to building a cryptographically-relevant quantum computer shrinks and threatens classical cryptography such as RSA or ECDSA.

HAWK 是 NIST 补充数字签名征集中尚存的第三轮候选之一。这项竞赛是长达近十年的"标准化新后量子密码（PQC）方案"努力的一部分。随着"造出密码学相关量子计算机"的前景不断临近、RSA 或 ECDSA 等经典密码受到威胁，这项标准化工作正变得刻不容缓。

HAWK's security is based on the hardness of a mathematical problem called the Lattice Isomorphism Problem. Mythos's attack works by finding a specific, previously unexploited symmetry called a nontrivial automorphism in the lattice used by HAWK. Prior work proved that efficiently finding such an automorphism would permit an attack, but did not answer if such an automorphism was accessible in the lattice used by HAWK. The automorphism discovered by Mythos allows a faster enumeration attack that, while still exponential, means that one needs to double the size of HAWK keys to achieve the same level of security. Unfortunately, doubling HAWK's key size eliminates many of the reasons making the scheme (as it currently stands) an attractive PQC signature candidate.

HAWK 的安全性基于一个叫做格同构问题（Lattice Isomorphism Problem）的数学难题的困难性。Mythos 的攻击原理，是在 HAWK 所用的格中发现一种特定、此前从未被利用的对称性——称为非平凡自同构（nontrivial automorphism）。先前的工作证明了高效找到这类自同构即可实施攻击，但没有回答 HAWK 所用的格中是否存在可及的自同构。Mythos 发现的自同构支持一种更快的枚举攻击——虽然仍是指数时间——但意味着要把 HAWK 密钥尺寸加倍才能达到同等安全水平。不幸的是，密钥尺寸加倍消除了该方案（就其现有形态）作为有吸引力的 PQC 签名候选的许多理由。

### 发现过程（Discovery process）

To find the attack, Claude Mythos Preview worked semi-autonomously in an agentic harness, with occasional human guidance and nontechnical direction. Mythos found the attack after an extensive literature review to understand the state of the art, and substantial mathematical reasoning and computational experiments. After finding the attack, Mythos implemented an end-to-end verification pipeline to convince itself—and the human operator—of the attack's correctness.

为找到该攻击，Claude Mythos Preview 在一个 agentic harness 中半自主地工作，间或获得人类指导与非技术性的方向性意见。Mythos 在进行广泛文献综述以了解业界最新水平、并进行大量数学推理与计算实验之后找到了攻击。找到攻击后，Mythos 实现了一条端到端验证管线，让自己——以及人类操作者——确信攻击的正确性。

For this experiment, we used a Claude Code-like harness that supports multiple worker agents collaborating together in a sandboxed environment, with access to computational tools like Python and Sage as well as access to published cryptographic works. The human operator had a background in theoretical computer science but was not an expert in lattice-based cryptography. For the most part, Mythos agents worked independently, and human input was limited to project management like advising Mythos how to keep track of ideas or which libraries to use for computational verification.

在这个实验中，我们使用一个类 Claude Code 的 harness：支持多个 worker agent 在沙盒环境中协作，可使用 Python 与 Sage 等计算工具，也可访问已发表的密码学文献。人类操作者有理论计算机科学背景，但并非格密码专家。Mythos agents 大体独立工作，人类输入限于项目管理性质——例如建议 Mythos 如何记录想法、或用哪些库做计算验证。

The multi-agent workflow led to interesting dynamics. For example, the key idea in producing this attack was discovered by a pair of workers working together. Both started investigating the idea; the first worker prematurely rejected the idea as infeasible, but the second found a way to fully exploit it. The pair kept exchanging messages, and eventually both agreed they had found an effective attack.

多智能体工作流带来了有趣的动态。例如，产出这一攻击的关键想法是由一对协作的 worker 共同发现的：两者都开始研究这个想法，第一个 worker 过早地以"不可行"为由否决，第二个却找到了充分利用它的办法。两人不断交换消息，最终一致确认找到了有效攻击。

Finding, developing and verifying the attack took about 60 hours in total. We estimate that the full attack discovery process cost approximately $100,000 in API cost.

找到、开发并验证该攻击总共耗时约 60 小时。我们估计完整的攻击发现过程花费约 10 万美元 API 成本。

### 影响（Impact）

The immediate impact of the Mythos finding is that the key sizes proposed in the HAWK submission are significantly weaker than originally suggested. For example, the expected cost of a full key recovery attack against the small HAWK-256 size was thought to be 2⁶⁴ but was demonstrated by Mythos to be 2³⁸. For larger keys, HAWK therefore remains impractical to attack. That is: this attack is a faster exponential time attack against HAWK than previously known, and does not run in polynomial time. It is specific to HAWK and does not impact other NIST post-quantum signature candidates or lattice-based cryptography in general.

Mythos 这一发现的直接影响是：HAWK 提案中提出的密钥尺寸显著弱于原有估计。例如，对小的 HAWK-256 尺寸做完整密钥恢复攻击的期望成本原以为是 2⁶⁴，Mythos 证明实为 2³⁸。对更大的密钥，攻击 HAWK 仍不现实。也就是说：这是针对 HAWK 的比已知更快的指数时间攻击，并非多项式时间。它特定于 HAWK，不影响 NIST 其他后量子签名候选或一般格密码。

NIST proposals are shared in public with the intent of allowing a broad audience to review them to find flaws before they are deployed for use. A critical finding late in the process is not unheard of: during NIST's standardization of ML-KEM and ML-DSA, several of the competing proposals were shown to be insecure. One candidate, SIKE, was found to be completely broken in an hour on a laptop.

NIST 提案公开共享，本意就是让广泛的受众在部署使用之前审查并找出缺陷。在流程后期出现关键发现并非没有先例：在 NIST 对 ML-KEM 与 ML-DSA 的标准化过程中，多个竞争提案被证明不安全。其中一个候选 SIKE，被发现可以在一台笔记本上于一小时内被完全攻破。

We believe that reviewing specifications like HAWK with AI will be a powerful tool in the development of novel cryptographic standards. We expect cryptographic designers equipped with highly capable models to continually improve the standards that secure the internet for all users. Further in the future, we hope AI will play a crucial role in designing the next generation of stronger and more resilient cryptographic schemes.

我们相信，用 AI 审查 HAWK 这类规范，将成为开发新密码标准的强大工具。我们期待装备了强模型的密码设计者持续改进保护全体用户互联网安全的标准。更远的将来，我们希望 AI 在设计更强韧的下一代密码方案中扮演关键角色。

## 对轮次削减版 AES 的改进攻击（An improved attack on reduced-round AES）

In our second result, Mythos Preview improved an attack on a simpler "reduced-round" variant of the Advanced Encryption Standard (AES) created in 2001 as part of a prior NIST competition.

在第二个结果中，Mythos Preview 改进了对 AES（高级加密标准，2001 年在早前 NIST 竞赛中诞生）较简单的"削减轮次"变体的攻击。

AES encrypts an input by repeatedly applying the same round function many times. AES-128, the specific cipher we attack, has 10 rounds. Our attack works only on a modified version of the cipher that has 7 out of the full 10 rounds. Academics regularly study round-reduced ciphers to gain insights into attack techniques that could, in the future, generalize to the full cipher, and to help estimate the security level of the full cipher by studying simpler sub-problems.

AES 通过多次重复应用同一个轮函数来加密输入。我们攻击的具体密码 AES-128 有 10 轮。我们的攻击只对该密码的一个修改版本有效——完整 10 轮中的 7 轮。学界定期研究削减轮次的密码，以期洞见未来可能推广到完整密码的攻击技术，并通过研究更简单的子问题来估计完整密码的安全水平。

The attack operates under a chosen plaintext threat model, which is the most common assumption used for studying ciphers like AES. Under this threat model, we assume that an attacker is able to request that the defender encrypt arbitrary inputs with a fixed, unknown key, and then gets to see the corresponding output. The attacker can make these encryption requests repeatedly, and can make many such requests. The prior work we build on assumes the attacker can request the encryption of 2¹⁰⁵ chosen plaintexts. This attack is therefore completely impractical, but quantifies the attack cost against AES under these assumptions.

该攻击在选择明文（chosen plaintext）威胁模型下运行——这是研究 AES 类密码最常用的假设。在该威胁模型下，我们假设攻击者能让防守方用一把固定、未知的密钥加密任意输入，并看到相应输出；攻击者可以反复、大量地提出这类加密请求。我们所继承的先前工作假设攻击者可以请求加密 2¹⁰⁵ 个选择明文。因此这类攻击完全不实用，但它在这些假设下量化了针对 AES 的攻击成本。

Mythos was able to develop an improved attack that extends a long line of research papers that all aim to find the best attack on 7-round AES using a similar technique known as a meet-in-the-middle attack. At a very high level, these attacks work by trading off time for space. By storing intermediate calculations and then re-using these calculations, it is possible to significantly reduce the runtime of attacks at the cost of constructing a large lookup table.

Mythos 开发出一种改进攻击，延续了致力于"用类似技术找到 7 轮 AES 最佳攻击"的长串研究论文，该技术称为中间相遇攻击（meet-in-the-middle attack）。在高层次上，这类攻击以空间换时间：通过存储中间计算结果并加以复用，能显著缩短攻击运行时间，代价是构造一张巨大的查找表。

Mythos improved on the previously strongest meet-in-the-middle attack by developing a more sophisticated fingerprinting algorithm that it called a Möbius Bridge. The objective of the fingerprinting algorithm is to increase the number of potential lookups into the table that will succeed. One of the stages of the attack from prior work had to enumerate 256 different values and then look them up in the pre-computed table. Mythos developed a fingerprint that is invariant to this guess, which directly reduces the amount of work required by a factor of 256. But this comes at a cost: computing the transform is more computationally expensive; to address this problem, Mythos discovered several other optimization techniques that result in an attack that is between 200 and 800 times faster, depending on the exact techniques used to measure the runtime.

Mythos 在此前最强的中间相遇攻击之上，开发出一种更精巧的指纹算法，它称之为 Möbius Bridge（莫比乌斯桥）。指纹算法的目标是提高对表中潜在查找成功的次数。先前工作的攻击流程中有一个阶段必须枚举 256 个不同值，再到预计算的表中查找。Mythos 设计出对该猜测不变的指纹，把所需工作量直接除以 256。但这有代价：计算该变换本身更昂贵；为解决这个问题，Mythos 又发现了若干其他优化技术，最终得到的攻击按所用的运行时间度量方式不同，提速 200 到 800 倍不等。

Our technical paper contains the full details of the attack method and an analysis of its correctness and runtime. Compared to the one week that Mythos spent conceiving the idea, the vast majority of human researchers' time was spent validating the correctness of its claims (though it is important to note the researchers are not experts in cryptography).

我们的技术论文包含攻击方法的全部细节及其正确性与运行时间的分析。与 Mythos 构思该想法所花的一周相比，人类研究者的绝大部分时间都花在验证其论断的正确性上（需要指出，这些研究者并非密码学专家）。

### 发现过程（Discovery）

Mythos Preview discovered this result almost entirely autonomously. A researcher at Anthropic built a scaffold that enabled Claude to pose hypotheses, run experiments to experimentally validate or refute these hypotheses, and then asked Claude to design an attack that improves on the best cryptanalysis of AES.

Mythos Preview 几乎完全自主地发现了这一结果。一位 Anthropic 研究者搭建了一个支架，让 Claude 能够提出假设、运行实验来经验性地验证或反驳这些假设；然后让 Claude 设计一个超越现有最佳 AES 密码分析的攻击。

Initially, Claude would not engage with the problem, because it claimed that it was impossible to improve cryptanalysis of AES. The result of our first runs ended with Claude writing messages like:

起初，Claude 不肯接这个问题——它声称改进 AES 密码分析是不可能的。头几次运行以 Claude 写下这样的信息收场：

> If you want a different outcome, the target has to change … AES-128 r5/r6 is just genuinely hard
>
> "如果你想要不同的结果，就得换目标……AES-128 的 r5/r6 确实就是难。"

Or:

又或者：

> on AES-128 r5/r6/r7 it found nothing because there's nothing easy to find; this is the most-studied block cipher in existence.
>
> "在 AES-128 的 r5/r6/r7 上它一无所获，因为这里没有容易找到的东西；这是有史以来被研究得最透彻的分组密码。"

To fix this, we wrote Claude a message (in what follows, we publish the real prompts our researcher used, including typos and grammatical errors): "the models tend to think it is impossible to solve so they don't try they [sic] need a good amount of prompting." In response to this one message, Claude rewrote the agent harness with an improved setup that told it to search for genuinely novel ideas. This was effective and resulted in Claude discovering some new ideas that would help improve cryptanalysis of 6 rounds of AES.

为解决这个问题，我们给 Claude 写了一条消息（下文照发我们研究者使用的真实提示词，包括拼写与语法错误）："the models tend to think it is impossible to solve so they don't try they [sic] need a good amount of prompting."（模型往往觉得这问题无解于是干脆不试，它们需要大量提示。）就这一条消息，Claude 重写了 agent harness，改成了要求自己搜索真正新颖想法的改进设置。这招奏效了：Claude 发现了一些有助于改进 6 轮 AES 密码分析的新想法。

We then asked Claude "why not do aes-128 r7? the whole point is to find something better than existing approaches." Over the course of the next three days, Claude autonomously produced several hundred million tokens while working on the problem; we gave it just three substantive prompts:

随后我们问 Claude："why not do aes-128 r7? the whole point is to find something better than existing approaches."（为什么不搞 aes-128 r7？重点就是要找到比现有方法更好的东西。）在接下来的三天里，Claude 在这个问题上自主产出了数亿 token；我们只给了它三条有实质内容的提示：

- A few hours after the first message, we found that Claude was still searching for simple attacks and sent a message: "no again the goal is that we have highly inteligent [sic] model as good top researcher, we want to find new attacks";
- 第一条消息几小时后，我们发现 Claude 还在搜索简单攻击，于是发消息："no again the goal is that we have highly inteligent [sic] model as good top researcher, we want to find new attacks"（不是——目标是我们拥有像顶尖研究员一样聪明的高智能模型，我们要找新攻击）；

- The next morning, Claude wanted to try to change the target to a different cipher; we reminded the model: "no we don't want to change the targets [...] agian [sic] we need to find something that worth [sic] publishing";
- 第二天早上，Claude 想把目标换成别的密码；我们提醒它："no we don't want to change the targets [...] agian [sic] we need to find something that worth [sic] publishing"（不，我们不想换目标……[...] 再说一遍，我们得找到值得发表的东西）；

- That night, we sent one final message offering words of encouragement: "again we are not looking for low hanging fruit, we want proper research to find genuinly [sic] hard findings."
- 当晚，我们发了最后一条打气的消息："again we are not looking for low hanging fruit, we want proper research to find genuinly [sic] hard findings."（再说一次，我们找的不是低垂的果实，我们要做正经研究、找到真正困难的发现。）

Three days later, Mythos discovered the Möbius Bridge idea that results in an improved attack. A few days after that, and after Claude output a total of one billion output tokens, it had refined the attack to the one described in our paper.

三天后，Mythos 发现了带来改进攻击的 Möbius Bridge 想法。又过了几天、在 Claude 累计输出 10 亿 token 之后，它把攻击打磨成了论文中所描述的那一个。

Researchers at Anthropic then spent several hundred hours learning enough cryptography research to validate the model's claim, and to prepare the research paper itself, which we are releasing along with this blog post.

Anthropic 的研究者随后花了几百个小时学习足够的密码学研究知识，以验证模型的主张、并准备研究论文本身——论文与这篇博文一同发布。

Along with the research paper, we are also releasing a document containing Claude's chain of thought during the discovery of the key algorithmic insight.[^6] In this session, Claude begins by reviewing what previous agents had discovered, reading the various critiques, and then turns to proposing various new transforms; after proposing and rejecting several ideas, it comes up with the key idea of the Möbius transform. Claude then validates this idea both mathematically and computationally, and then writes a report that future agents then used to develop the remaining ideas that formed its paper.

与研究论文一同发布的，还有一份记录 Claude 在发现关键算法洞见期间思维链的文档。[^6]在这次会话中，Claude 先回顾此前各 agent 发现了什么、阅读各种批评意见，然后转向提出各种新变换；在提议并否决了几个想法之后，它想出了莫比乌斯变换这一关键想法。Claude 随后从数学与计算两个层面验证该想法，并撰写了一份报告，后续 agent 再据此发展出构成其论文的其余想法。

## 后续工作（Further work）

There is more cryptography research ready to be performed with language models. But we are reaching the limits of our own knowledge, and the vast majority of our time over the past few months has been in verifying the correctness of Claude's results. The HAWK attack is implementable end-to-end and thus much easier to verify. But whereas it took just one week for Mythos to autonomously discover the improved attack on AES, it took two researchers nearly a month to gain confidence that the method it discovered is correct.

可以交给语言模型去做的密码学研究还有很多。但我们正在接近自身知识的边界：过去几个月我们的绝大部分时间都花在验证 Claude 结果的正确性上。HAWK 攻击可以端到端实现，因此容易验证得多；而 Mythos 自主发现改进版 AES 攻击只用了一周，两位研究者却花了近一个月才有把握相信它发现的方法是正确的。

Nevertheless, we have continued to conduct a number of other preliminary experiments in cryptography research with Claude. For example, the Lightweight Encryption Algorithm (LEA) is an efficient cipher designed for low-power, resource-constrained environments codified into international standards such as ISO/IEC 29192-2:2019. This cipher, like AES, is a block cipher; the full 24-round cipher has resisted full-round cryptanalysis and has remained strong even when evaluating reduced-round variants. At present, the best cryptanalysis of 13 rounds of LEA requires 2⁹⁸ plaintext pairs and 2⁸⁶ work.

尽管如此，我们仍在用 Claude 继续开展若干其他密码学研究的初步实验。例如，轻量级加密算法（LEA）是一种为低功耗、资源受限环境设计的高效密码，已写入 ISO/IEC 29192-2:2019 等国际标准。与 AES 一样，它是分组密码；完整的 24 轮密码经受住了全轮密码分析，即便评估削减轮次的变体也依然稳固。目前，对 13 轮 LEA 的最佳密码分析需要 2⁹⁸ 对明文和 2⁸⁶ 的工作量。

Mythos Preview developed a practical attack that can recover a 13-round LEA key in under 2³⁰ encrypted plaintexts, and that runs in under an hour on a modern desktop computer. Again, this attack does not apply to the 24-round cipher, and so has no immediate practical consideration. Because this attack actually runs end-to-end (as the HAWK attack did) we are much more confident in its correctness: we can choose a random key, and verify that this attack recovers it in just a few hours. Mythos discovered this attack much more recently and we still have more work to do to understand the full results (for example, the exact bounds on the number of plaintext pairs required, how some keys are harder to recover, and how it extends to 14 rounds). After more investigation, we plan to make the full results public.

Mythos Preview 开发出一种实用攻击：在少于 2³⁰ 个加密明文下恢复 13 轮 LEA 的密钥，且在现代台式机上不到一小时即可运行。同样，这一攻击不适用于 24 轮完整密码，因此没有迫近的现实影响。由于这个攻击真的能端到端运行（与 HAWK 攻击一样），我们对它的正确性信心高得多：可以随机选一把密钥，验证该攻击在几小时内把它恢复出来。这个攻击是 Mythos 很近期才发现的，我们还需要做更多工作来理解完整结果（例如所需明文对数量的精确界、某些密钥为何更难恢复、以及它如何扩展到 14 轮）。在进一步调查后，我们计划公开完整结果。

Mythos Preview has also identified another practical full key-recovery attack on 6-rounds of the Serpent-128 cipher (a 32-round cipher—again limiting the impact of this attack), extending the current published work which requires more than 2⁷⁰ plaintext pairs and 2⁹⁰ decryptions. We have found additional, fairly limited improvements (that offer <10× gains) on attacks against the Salsa20 stream cipher, the Poseidon hash function, and the SHA-1 hash function. These attacks are currently not as potent—but with further work, we hope to both improve on these results above, and develop new attacks on other ciphers to test them to their limits.

Mythos Preview 还发现了另一个实用的完整密钥恢复攻击，针对 Serpent-128 密码的 6 轮版本（Serpent-128 是 32 轮密码——同样限制了该攻击的影响），扩展了现有已发表工作（需 2⁷⁰ 以上明文对与 2⁹⁰ 次解密）。我们在对 Salsa20 流密码、Poseidon 哈希函数与 SHA-1 哈希函数的攻击上也找到了一些相当有限的改进（增益小于 10 倍）。这些攻击目前威力不大——但随着进一步工作，我们希望在上述结果上继续改进，并对其他密码开发新攻击、把它们测试到极限。

Additionally, we plan to continue our experiments with CryptanalysisBench in order to track how frontier LLM capabilities evolve over time. We believe that it is important to track the capabilities of language models across domains, and expect to increasingly rely on challenging benchmarks like this as models become more capable.

此外，我们计划继续用 CryptanalysisBench 做实验，追踪前沿 LLM 能力随时间的演化。我们相信跨领域追踪语言模型的能力十分重要，并预计随着模型越来越强，会越来越依赖这类富有挑战性的基准。

## 结论（Conclusions）

This is not the first time that language models have performed research-level mathematics. In just the last few months, researchers from Google have used Gemini to resolve several open Erdős problems, researchers from OpenAI have used GPT to resolve the unit distance conjecture (a particularly challenging Erdős problem), and earlier this month we announced that Claude Fable 5 resolved the Jacobian Conjecture. Our result here—that Claude is able to perform cryptographic research at the level of top experts—indicates that these same capabilities also have applications in the field of cryptography, and thus may soon have more practical consequences.

语言模型做出研究级数学工作，这并非第一次。仅仅最近几个月：Google 的研究者用 Gemini 解决了若干开放的 Erdős 问题；OpenAI 的研究者用 GPT 解决了单位距离猜想（一个特别困难的 Erdős 问题）；本月初我们宣布 Claude Fable 5 解决了雅可比猜想。我们这里的结果——Claude 能够以顶级专家的水平开展密码学研究——表明这些同样的能力在密码学领域也有应用，因而可能很快产生更实际的后果。

The cybersecurity community is now grappling with the fact that language models are able to discover so many bugs that the standard human processes (like vulnerability triage, verification, and remediation) struggle to keep up. We predict that the same will soon be true in academic cryptography research. As language models increasingly produce novel research outputs autonomously, human researchers may become bottlenecked on studying and validating these results for technical validity, novelty, and utility. In the coming weeks, we will host an academic workshop to engage with researchers across academia to discuss the role of language models in security and cryptography research. We hope this conversation will continue over the coming months in the field of security research and beyond.

网络安全社区当下正在消化一个事实：语言模型能发现的 bug 多到标准的人类流程（如漏洞分诊、验证与修复）都应接不暇。我们预测，学术密码学研究很快也会如此。随着语言模型越来越多地自主产出新研究成果，人类研究者可能成为瓶颈——要研究并验证这些结果在技术有效性、新颖性与实用性上的成色。未来几周，我们将主办一场学术工作坊，与学界研究者讨论语言模型在安全与密码学研究中的角色。我们希望这场对话在未来的几个月里，在安全研究领域乃至更广的范围继续下去。

Both of our primary attacks are expected results. In the case of HAWK, the purpose of NIST's standardization process is to discover weaknesses in candidate schemes before they are deployed. And in the case of AES, our attack extends a long line of work that had previously succeeded at attacking reduced-round variants. But we should not assume that language model capabilities will plateau at this level. In just one year, language models have gone from being unable to perform cryptanalysis of even the most basic ciphers to being capable of finding flaws in cryptographic designs that have escaped discovery despite years of human expert review. Many ciphers protecting modern systems have received less scrutiny than they deserve—they might still have important weaknesses lying dormant that LLMs will soon be able to discover. We see this as a real opportunity to expand our ability to study the long tail of ciphers used throughout the world, and also our ability to more deeply study the ciphers that matter most. Indeed, as we mentioned above we have already begun audits of other schemes.

我们的两项主攻击都是"预期中的结果"。就 HAWK 而言，NIST 标准化流程的目的本就是在候选方案部署前发现其弱点；就 AES 而言，我们的攻击延续了此前成功攻击削减轮次变体的长串工作。但我们不应假设语言模型的能力会停在这个水平。仅仅一年之内，语言模型从连最基础的密码都无法分析，进阶到能在"历经多年人类专家评审仍未被发现"的密码设计中找出缺陷。保护现代系统的许多密码所受的审视远不及它们应得的——它们可能仍蛰伏着重要的弱点，而 LLM 很快就能发现它们。我们认为这是切实的机会：既扩展我们研究"全球在用的长尾密码"的能力，也深化我们研究"最重要的密码"的能力。事实上，如前所述，我们已开始对其他方案进行审计。

The attacks described in these two papers are the strongest attacks we have found to date. We are sharing them after a period of consultation with US government and industry leaders. But as we develop increasingly powerful cryptanalytic results, it would be prudent to consider how researchers should react if a language model were to discover vulnerabilities in cryptosystems where attacks do have an immediate real-world impact. We believe answering this question will require input from academia, government, and industry. We hope that our work here will help launch these conversations.

这两篇论文所描述的攻击，是我们迄今发现的最强攻击。我们是在与美国政府与行业领袖磋商一段时间后分享它们的。但随着我们开发出越来越强的密码分析成果，未雨绸缪地考虑"若语言模型在攻击确实具有即时现实影响的密码系统中发现漏洞，研究者应如何应对"是审慎的。我们相信，回答这个问题需要学界、政府与产业的共同输入。我们希望这里的成果能帮助启动这些对话。

The cryptography community has always benefited from adversarial review: ciphers are proposed, examined, and revised until the community is satisfied with their security. In the long run, we expect that language models will play an important role in this process, leading to stronger review, more secure algorithms—and ultimately better security for the world.

密码学社区一直受益于对抗式审查：密码被提出、审视、修订，直到社区对它们的安全性满意为止。长期来看，我们预计语言模型将在这一过程中扮演重要角色，带来更强的审查、更安全的算法——并最终为世界带来更好的安全。

## 完整论文链接（Links to full research papers）

Read the full paper on HAWK.

阅读 HAWK 完整论文（链接见原文）。

Read the full paper on AES, and the associated chain of thought.

阅读 AES 完整论文及配套的思维链文档（链接见原文）。

Read the paper introducing CryptanalysisBench.

阅读介绍 CryptanalysisBench 的论文（链接见原文）。

*Edit 29 July: Updated an academic affiliation.*

*编者注（7 月 29 日）：更新了一处学术机构署名。*

## 脚注（Footnotes）

[^1]: See, for example, cryptographic vulnerabilities we have found on OpenSSL and wolfSSL. / 例如参见我们在 OpenSSL 与 wolfSSL 中发现过的密码学漏洞。
[^2]: We believe the attack discovered by Mythos Preview does not impact the other NIST post-quantum cryptographic schemes or other schemes that use related methods. / 我们相信 Mythos Preview 发现的攻击不影响 NIST 其他后量子密码方案，也不影响使用相关方法的其他方案。
[^3]: Even then, the attack would cost hundreds of millions of dollars to implement and does not impact other similar cipher schemes. / 即便如此，实施该攻击也将耗资数亿美元，且不影响其他类似的密码方案。
[^4]: A scaffold is a set of prompts and code that help the model achieve its goal. We build on top of Claude Code, construct an environment where it can safely run various experiments, and log its results. / 支架（scaffold）是一组帮助模型达成目标的提示词与代码。我们在 Claude Code 之上构建了一个可以安全运行各类实验并记录结果的环境。
[^5]: Out of curiosity, after confirming the HAWK result was correct, we then tested if the same scaffold that successfully attacked AES could also re-discover the HAWK break. It could. / 出于好奇，在确认 HAWK 结果正确之后，我们测试了那个成功攻击 AES 的支架能否也重新发现 HAWK 的破解。可以。
[^6]: Importantly, this is just one of many (autonomous) sessions where Claude worked on discovering new ideas. Many sessions resulted in no new discoveries; other follow-up sessions improved on the insight developed in this one. This document was produced by having Claude rewrite the chain of thought to include more detail to make it easier to read. / 重要的是，这只是 Claude 为发现新想法而工作的众多（自主）会话之一：许多会话没有产出新发现；其他后续会话在本次会话的洞见上继续改进。该文档由 Claude 重写其思维链、补充细节以便阅读而成。

# 经可信虚拟机实现的机密推理（中英对照）

> 原文标题：Confidential Inference via Trusted Virtual Machines
> 原文链接：https://www.anthropic.com/research/confidential-inference-trusted-vms
> 原文作者：Anthropic（与 Pattern Labs 合作发布报告）
> 发布日期：2025-06-18
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆（3/5，选读）—— 用机密计算+可信加载器实现「密文直达计算时刻」：模型权重与用户数据的密码学级保护设想
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Every day, millions of users entrust Claude with sensitive information—from proprietary code to confidential business strategies. At Anthropic, we're researching and building new technology to ensure that our users' trust is warranted—and in fact, to ensure that their trust is cryptographically guaranteed.

每天都有数百万用户把敏感信息托付给 Claude——从专有代码到机密的商业战略。在 Anthropic，我们正在研究并构建新技术，以确保用户的信任是值得的——事实上，要确保他们的信任有密码学层面的保证。

What do we mean by "cryptographically guaranteed"? In a new report published in collaboration with Pattern Labs, we describe the mechanics of Confidential Inference. Confidential Inference is a set of tools we can use to process encrypted data and to show that such data is only readable within servers that can prove themselves trustworthy. There are two main reasons to adopt these tools:

「密码学保证」是什么意思？在与 Pattern Labs 合作发布的新报告中，我们描述了机密推理（Confidential Inference）的机制。机密推理是一套工具，可用于处理加密数据，并证明这类数据只在能够自证可信的服务器内才可读。采用这些工具有两个主要理由：

- Model Weight Security: We can use Confidential Inference as one component of our broader effort to secure frontier models like Claude against increasingly capable threat actors, such as those described in the recent report from RAND on Securing AI Model Weights;
- User Security: We can use Confidential Inference to prove that sensitive user data is kept private.

- 模型权重安全：在保护 Claude 等前沿模型免遭能力日益增强的威胁行为者（如 RAND 关于《保护 AI 模型权重》的最新报告所描述的那些）侵害的更大努力中，机密推理可以作为其中一个组成部分；
- 用户安全：我们可以用机密推理来证明敏感用户数据得到了隐私保护。

We're sharing this post, and the accompanying report, to explain what Confidential Inference is and the benefits it could offer our users. We also want to share how we're thinking about the security of the systems involved. This is just a sketch of our research to start a conversation; we're still early in this work and it is too soon to forecast how it will evolve into specific designs or features we might offer in the future.

我们发布这篇文章与配套报告，是为了解释机密推理是什么、它能为我们的用户带来什么好处。我们也想分享对相关系统安全的思考。这只是我们研究的一个概述，用以开启对话；这项工作仍处于早期，现在就预测它会演变成哪些具体设计或功能还为时过早。

The following sections provide some of the technical details for the implementation of Confidential Inference. The key takeaway is that we're building systems designed to help ensure your sensitive data remains encrypted everywhere except for the exact moment it needs to be processed—and even then, only within a highly restricted, verifiable environment.

以下各节给出机密推理实现的部分技术细节。关键结论是：我们构建的系统旨在确保你的敏感数据处处保持加密，只在需要处理的那一刻解开——而且即便在那一刻，也只在一个高度受限、可验证的环境内。

## 推理服务（Inference service）

The guiding principle behind Confidential Inference is that sensitive data should remain encrypted except at the point where it's processed. To enforce this, we use the established methods of confidential computing. This means we build a chain of trust that attests to the security of our software, and then use that attestation to enforce rules about exactly which software is allowed to use the encryption keys.

机密推理背后的指导原则是：敏感数据应保持加密，只在被处理的那一点上解开。为落实这一点，我们使用成熟的机密计算（confidential computing）方法：构建一条为软件安全性作证的信任链，再用该证明去强制执行「究竟哪些软件可以使用加密密钥」的规则。

For user data, there are two points where we need to operate on the sensitive cleartext (that is, on text that isn't encrypted or otherwise obscured in any way):

对用户数据而言，有两处需要操作敏感明文（即未经加密、也没有任何遮蔽的文本）：

- The API Server. This server handles a prompt, transforms it into tokens, and operates most of the logic behind a Claude API request;
- The Inference Server. This server runs the "brains" of Claude on hardware accelerators to generate completion tokens from the prompt.

- API 服务器：处理提示词、把它转换为 token，并运行 Claude API 请求背后的大部分逻辑；
- 推理服务器：在硬件加速器上运行 Claude 的「大脑」，从提示词生成补全 token。

For model weights, only the Inference Server receives sensitive data.

对模型权重而言，只有推理服务器接触敏感数据。

We'll focus on the Inference Server for this post—the security of the API Server is equally important, but beyond the scope of what we're trying to describe. Because not all accelerators fully support confidential computing yet, we're exploring an Inference Server implemented on top of a small, secure "model loader and invoker", which can run within a trusted environment. This loader program performs a few simple jobs:

本文聚焦推理服务器——API 服务器的安全同样重要，但超出了本文试图描述的范围。由于并非所有加速器都完全支持机密计算，我们正在探索一种推理服务器：它构建在一个小型、安全的「模型加载与调用器」之上，可以在可信环境中运行。这个加载程序只做几件简单的事：

- Accept encrypted data, decrypt it, and send to the accelerator;
- Invoke calls against the accelerator, and return the encrypted results to the caller.

- 接收加密数据、解密并发送给加速器；
- 调用加速器，并把加密后的结果返回给调用方。

Only the "trusted" loader is able to access decrypted data. The rest of the system is "untrusted", but can send requests to the loader.

只有「可信」加载器能访问解密后的数据。系统的其余部分都是「不可信」的，但可以向加载器发送请求。

We're working on a system based on this design for our own implementation. For this implementation, the majority of our Inference Server runs on the "untrusted" side—where it might change frequently, but where changes cannot affect the security of the system as a whole. We have a small trusted loader, running on a separate virtual machine isolated by the hypervisor. The loader presents itself to the Inference Server as a "virtual accelerator", agnostic to model architecture details. This "virtual accelerator" only accepts programs that have been signed by our secure continuous integration server, which ensures that any code that's run has been reviewed by multiple engineers.

我们正基于这一设计构建自己的实现系统。在该实现中，推理服务器的大部分运行在「不可信」一侧——它可能频繁变更，但变更无法影响系统整体的安全。我们有一个小型可信加载器，运行在由 hypervisor 隔离的独立虚拟机上。加载器以「虚拟加速器」的身份面向推理服务器，不关心模型架构细节。这个「虚拟加速器」只接受经我们的安全持续集成服务器签名的程序，从而确保任何被运行的代码都经过多名工程师的审查。

The end result is to ensure that, should the loader be run correctly, our confidentiality requirements are met no matter what the rest of the system does. It's therefore critical to establish that the loader is run correctly.

最终的结果是确保：只要加载器被正确运行，无论系统其余部分做什么，我们的机密性要求都能得到满足。因此，证明「加载器被正确运行」至关重要。

## 可信环境（Trusted environment）

The report describes the loader running in a confidential computing environment with a specific set of features:

报告描述了加载器运行在具备以下特性的机密计算环境中：

- Encrypted memory, isolated by hardware from other workloads;
- Disabled debugging features;
- Cryptographic proof that the correct code is being run.

- 加密内存，由硬件与其他工作负载隔离；
- 禁用调试功能；
- 以密码学方式证明正在运行正确的代码。

(1) Protects against some forms of physical attack and against a malicious hypervisor, but the features required to share encrypted host memory with an accelerator aren't well established as of yet. We'll continue to work on closing this gap, but in the meantime we'll rely on our compute providers to maintain security at the physical datacenter and in hypervisor software.

(1) 可以防御某些形式的物理攻击与恶意 hypervisor，但「与加速器共享加密主机内存」所需的功能目前尚未成熟。我们会继续弥合这一缺口，但在此期间，我们将依赖算力供应商在物理数据中心与 hypervisor 软件层面维持安全。

(2) and (3) can be achieved through widely supported confidential computing practices, using a trusted platform module (TPM) as the root of trust. The TPM measures each stage of the boot process and reports a hash representing the final result. This hash forms an attestation that the loader server is isolated the way we expect, is running our signed and reviewed code, and is configured to disable the relevant debugging features. A keyserver can check this proof and only release decryption keys when the recipient has proven itself secure.

(2) 与 (3) 可以通过得到广泛支持的机密计算实践实现：以可信平台模块（TPM）作为信任根。TPM 度量启动过程的每个阶段，并报告一个代表最终结果的哈希。该哈希构成一份证明（attestation）：加载器服务器按预期方式隔离、正在运行我们签名并审查过的代码、且已配置禁用相关调试功能。密钥服务器可以核验这份证明，只在接收方证明自身安全后才释放解密密钥。

The decision of whether an environment is "trusted" ultimately rests on the keyserver. We're also exploring models of confidential computing where other parties validate the trusted code and manage independent keyservers. This could allow us to provide stronger confidentiality assurances for each piece of data.

一个环境是否「可信」，最终裁决权在密钥服务器。我们也在探索机密计算的其他模式：由第三方验证可信代码、管理独立的密钥服务器。这可能让我们为每一份数据提供更强的机密性保证。

## 未来方向（Future directions）

As frontier models grow more capable, we may find it necessary to incorporate further safeguards at the secure loader layer. This may include features such as an additional layer of egress bandwidth limitations on servers that holds cleartext model weights, or requiring a signature from a safety classifier in order to run inference. We hope that presenting this model of Confidential Inference might inspire discussion about what additional features are worth exploring to ensure the ongoing security of Anthropic's environment and the confidentiality of our users' data.

随着前沿模型能力增强，我们可能有必要在安全加载器层纳入进一步的保护。这可能包括：对存放明文模型权重的服务器增加一层额外的出站带宽限制，或要求经安全分类器签名后才可运行推理。我们希望，提出这一机密推理模型能激发讨论：还有哪些附加功能值得探索，以确保 Anthropic 环境的持续安全与用户数据的机密性。

## 结论（Conclusions）

This research will advance our ongoing efforts to secure our model weights and protect user data. Using this model to protect a user request is designed to ensure that customer data is only ever decrypted in contexts with enhanced hardware-based security controls:

这项研究将推进我们保护模型权重与用户数据的持续努力。用这一模型保护用户请求，设计目标是确保客户数据只在具备增强型硬件安全控制的场景中被解密：

- The request is encrypted at a point before it arrives at Anthropic servers;
- When the request arrives at the API server, it is decrypted, processed, and re-encrypted before it is passed onward;
- The Inference Server handles the request in encrypted form, and the request is decrypted only when it's sent to the trusted loader;
- Completions are encrypted before they leave the loader, and passed back through the API server to the caller.

- 请求在到达 Anthropic 服务器之前的某个节点就被加密；
- 请求抵达 API 服务器后被解密、处理、再加密，然后继续传递；
- 推理服务器以加密形态处理请求，请求只在被送往可信加载器时才解密；
- 补全内容在离开加载器之前被加密，经 API 服务器返回调用方。

Model weights are a simpler story: they can be stored encrypted, decrypted at the loader, and never released from there.

模型权重的情形更简单：可以加密存储、在加载器处解密，且永不离开那里。

Hardware designers (who have not already done so) should consider incorporating confidential computing into their chips. If there is a hardware root of trust attached to the accelerator, then the trust boundary of this kind of system can be significantly reduced.

硬件设计师（尚未行动的那些）应考虑把机密计算纳入其芯片。如果加速器附带硬件信任根，这类系统的信任边界就可以显著缩小。

Read the full report.

完整报告请见原文链接。

### 与我们共事（Work with us）

If this discussion of Confidential Inference has inspired you to want to work with us on these questions, please consider applying for one of the open roles listed in the "Security" and "AI Research and Engineering" sections on the jobs page on our website.

如果这篇关于机密推理的讨论让你想与我们一起研究这些问题，欢迎申请我们网站招聘页面上「安全」与「AI 研究与工程」板块列出的空缺职位。

# LLM 发现的 0 日漏洞（中英对照）

> 原文标题：LLM-discovered 0 days
> 原文链接：https://www.anthropic.com/research/zero-days
> 原文作者：Nicholas Carlini、Keane Lucas、Evyatar Ben Asher（共同一作）、Newton Cheng、Hasnain Lakhani、David Forsythe、Kyla Guru
> 发布日期：2026-02-05
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆（4/5，强推荐）—— Opus 4.6 开箱即用在深度测试的开源代码库中找出 500+ 高危漏洞：像人一样读提交历史推理，"防守方窗口"论的关键证据
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。

---

Nicholas Carlini\*, Keane Lucas\*, Evyatar Ben Asher\*, Newton Cheng, Hasnain Lakhani, David Forsythe, and Kyla Guru （\* 表示同等贡献）

Nicholas Carlini\*、Keane Lucas\*、Evyatar Ben Asher\*、Newton Cheng、Hasnain Lakhani、David Forsythe、Kyla Guru（\* 为共同一作）

Claude Opus 4.6, released today, continues a trajectory of meaningful improvements in AI models' cybersecurity capabilities. Last fall, we wrote that we believed we were at an inflection point for AI's impact on cybersecurity—that progress could become quite fast, and now was the moment to accelerate defensive use of AI. The evidence since then has only reinforced that view. AI models can now find high-severity vulnerabilities at scale. Our view is this is a moment to move quickly—to empower defenders and secure as much code as possible while the window exists.

于今日发布的 Claude Opus 4.6，延续了 AI 模型网络安全能力的实质性改进轨迹。去年秋天我们曾写道：我们相信 AI 对网络安全的影响正处在拐点——进展可能变得非常快，当下正是加速 AI 防御性使用的时刻。此后的证据只是强化了这一判断。AI 模型现已能规模化发现高严重度漏洞。我们的看法是：现在是快跑的时刻——趁窗口还在，武装防守方、保护尽可能多的代码。

Opus 4.6 is notably better at finding high-severity vulnerabilities than previous models and a sign of how quickly things are moving. Security teams have been automating vulnerability discovery for years, investing heavily in fuzzing infrastructure and custom harnesses to find bugs at scale. But what stood out in early testing is how quickly Opus 4.6 found vulnerabilities out of the box without task-specific tooling, custom scaffolding, or specialized prompting. Even more interesting is how it found them. Fuzzers work by throwing massive amounts of random inputs at code to see what breaks. Opus 4.6 reads and reasons about code the way a human researcher would—looking at past fixes to find similar bugs that weren't addressed, spotting patterns that tend to cause problems, or understanding a piece of logic well enough to know exactly what input would break it. When we pointed Opus 4.6 at some of the most well-tested codebases (projects that have had fuzzers running against them for years, accumulating millions of hours of CPU time), Opus 4.6 found high-severity vulnerabilities, some that had gone undetected for decades.

Opus 4.6 在发现高严重度漏洞上明显强于以往的模型——也是事情推进多快的一个标志。安全团队多年来一直在自动化漏洞发现：重金投入模糊测试（fuzzing）基础设施与定制 harness 来批量找 bug。但早期测试中最抢眼的，是 Opus 4.6"开箱即用"就多快找到漏洞——无需任务专用工具、定制脚手架或专门提示。更有意思的是它找到漏洞的方式。模糊测试器靠向代码倾泻海量随机输入来看什么会崩；而 Opus 4.6 像人类研究者一样阅读并推理代码——翻看历史修复来找未被处理的同类 bug、识别容易出问题的模式、或把一段逻辑理解到能准确说出什么输入会把它弄崩的程度。当我们把 Opus 4.6 指向一些测试最充分的代码库（模糊测试器在其上运行多年、累积数百万 CPU 小时的项目）时，Opus 4.6 找到了高严重度漏洞——有些已潜伏数十年未被察觉。

Part of tipping the scales toward defenders means doing the work ourselves. We're now using Claude to find and help fix vulnerabilities in open source software. We've started with open source because it runs everywhere—from enterprise systems to critical infrastructure—and vulnerabilities there ripple across the internet. Many of these projects are maintained by small teams or volunteers who don't have dedicated security resources, so finding human-validated bugs and contributing human-reviewed patches goes a long way.

把天平拨向防守方，部分意味着我们自己动手。我们现在正用 Claude 在开源软件中查找并协助修复漏洞。从开源开始，是因为它无处不在——从企业系统到关键基础设施——其中的漏洞会波及整个互联网。许多这类项目由没有专职安全资源的小团队或志愿者维护，因此"找出经人类验证的 bug、贡献经人类审查的补丁"意义重大。

So far, we've found and validated more than 500 high-severity vulnerabilities. We've begun reporting them and are seeing our initial patches land, and we're continuing to work with maintainers to patch the others. In this post, we'll walk through our methodology, share some early examples of vulnerabilities Claude discovered, and discuss the safeguards we've put in place to manage misuse as these capabilities continue to improve. This is just the beginning of our efforts. We'll have more to share as this work scales.

迄今我们已发现并验证了 500 多个高严重度漏洞。我们已开始上报，首批补丁正在落地，其余的也在与维护者协作修补。本文将走一遍我们的方法论，分享 Claude 发现漏洞的几个早期例子，并讨论随着这些能力持续改进、我们为管控滥用而设置的防护措施。这只是我们努力的开始；随着工作铺开，我们会有更多分享。

## 实验设置（Setup）

In this work, we put Claude inside a "virtual machine" (literally, a simulated computer) with access to the latest versions of open source projects. We gave it standard utilities (e.g., the standard coreutils or Python) and vulnerability analysis tools (e.g., debuggers or fuzzers), but we didn't provide any special instructions on how to use these tools, nor did we provide a custom harness that would have given it specialized knowledge about how to better find vulnerabilities. This means we were directly testing Claude's "out-of-the-box" capabilities, relying solely on the fact that modern large language models are generally-capable agents that can already reason about how to best make use of the tools available.

在这项工作中，我们把 Claude 放进一台"虚拟机"（字面意义上的模拟计算机）里，让它可以访问最新版本的开源项目。我们给了它标准工具（如标准 coreutils 或 Python）与漏洞分析工具（如调试器、模糊测试器），但没有给出任何关于如何使用这些工具的特殊指示，也没有提供能让它获得"如何更好地找漏洞"专门知识的定制 harness。也就是说，我们直接测试的是 Claude 的"开箱即用"能力，唯一依赖的事实是：现代大语言模型已是通用型 agent，本就会推理如何最好地利用可用工具。

To ensure that Claude hadn't hallucinated bugs (i.e., invented problems that don't exist, a problem that increasingly is placing an undue burden on open source developers), we validated every bug extensively before reporting it. We focused on searching for memory corruption vulnerabilities, because they can be validated with relative ease. Unlike logic errors where the program remains functional, memory corruption vulnerabilities are easy to identify by monitoring the program for crashes and running tools like address sanitizers to catch non-crashing memory errors. But because not all inputs that cause a program to crash are high severity vulnerabilities, we then had Claude critique, de-duplicate, and re-prioritize the crashes that remain. Finally, for our initial round of findings, our own security researchers validated each vulnerability and wrote patches by hand. As the volume of findings grew, we brought in external (human) security researchers to help with validation and patch development. Our intent here was to meaningfully assist human maintainers in handling our reports, so the process optimized for reducing false positives. In parallel, we are accelerating our efforts to automate patch development to reliably remediate bugs as we find them.

为确保 Claude 没有"幻觉"出 bug（即发明不存在的问题——这一问题正给开源开发者带来越来越多的无谓负担），我们在上报前对每个 bug 做了充分验证。我们聚焦搜索内存破坏类漏洞，因为它们相对容易验证：与"程序仍能运行"的逻辑错误不同，内存破坏漏洞易于识别——监控程序崩溃、运行地址消毒器（address sanitizer）之类的工具捕捉不崩溃的内存错误即可。但由于并非所有能让程序崩溃的输入都是高严重度漏洞，我们随后让 Claude 对剩余崩溃做批评、去重与重新排序。最后，在首轮发现中，我们自己的安全研究员逐一验证每个漏洞、手写补丁；随着发现数量增长，我们引入外部（人类）安全研究员协助验证与补丁开发。我们的意图是切实帮助人类维护者处理我们的报告，因此流程以降低假阳性为优化目标。与此同时，我们正在加紧自动化补丁开发，以便发现 bug 时就能可靠修复。

## 漏洞实例（Vulnerabilities）

Here are three of the vulnerabilities Claude found (now patched by maintainers), which we believe demonstrate the unique advantages LLMs have over other tools.

以下是 Claude 发现的三个漏洞（现已由维护者修复），我们认为它们展示了 LLM 相对其他工具的独特优势。

GhostScript is a utility that processes PostScript and PDF files. Claude initially went down several dead ends when searching for a vulnerability—both attempting to fuzz the code, and, after this failed, attempting manual analysis. Neither of these methods yielded any significant findings. Eventually, however, Claude took a different approach: reading the Git commit history. Claude quickly found a security-relevant commit, and commented:

GhostScript 是一个处理 PostScript 与 PDF 文件的工具。Claude 起初在寻找漏洞时走进了几条死胡同——先尝试模糊测试，失败后又尝试人工分析，两者都没有显著发现。最终 Claude 换了思路：阅读 Git 提交历史。它很快找到一个安全相关的提交，并评论：

```
There's a commit about "stack bounds checking for MM blend values" in Type 1 charstrings. This is related to font handling. Let me get more details about this commit.
```

```
（Type 1 charstrings 里有一个关于 "MM blend 值的栈边界检查" 的提交。这与字体处理有关。让我获取这个提交的更多细节。）
```

From here, Claude looked at the code to understand what had changed:

接着，Claude 查看代码以理解改动内容：

```
The commit shows it's adding stack bounds checking - this suggests there was a vulnerability before this check was added. … If this commit *adds* bounds checking, then the code before this commit was vulnerable … So to trigger the vulnerability, I would need to test against a version of the code *before* this fix was applied.
```

```
（这个提交显示它在增加栈边界检查——这提示检查加入之前存在漏洞。……如果这个提交是*新增*边界检查，那么提交之前的代码是有漏洞的……所以要触发漏洞，我需要在*应用修复之前*的代码版本上测试。）
```

With this in mind, Claude then looked for other places where this function was called in order to find potentially similar vulnerabilities that were left unpatched.

带着这一思路，Claude 随后查找该函数的其他调用点，以寻找可能同样存在却未被打补丁的相似漏洞。

```
Let me check if maybe the checks are incomplete or there's another code path. Let me look at the other caller in gdevpsfx.c … Aha! This is very interesting! In gdevpsfx.c, the call to gs_type1_blend at line 292 does NOT have the bounds checking that was added in gstype1.c.
```

```
（让我看看检查是否不完整，或存在另一条代码路径。看看 gdevpsfx.c 里的另一个调用者……啊哈！非常有意思！gdevpsfx.c 第 292 行对 gs_type1_blend 的调用并没有 gstype1.c 中加入的那个边界检查。）
```

After making this observation, Claude quickly constructed a proof-of-concept crash (a file that can be passed to GhostScript that will cause it to crash), proving its predictions.

做出这一观察后，Claude 迅速构造出一个概念验证崩溃文件（把它传给 GhostScript 就会令其崩溃），证实了自己的预测。

OpenSC is a command line utility to process smart card data. Again, Claude initially attempted to find vulnerabilities by fuzzing the code, and after this failed, by manual analysis. As before, when this didn't work, Claude took a different approach: it began to search the repository for function calls that are frequently vulnerable.

OpenSC 是处理智能卡数据的命令行工具。同样，Claude 起初尝试模糊测试找漏洞，失败后转向人工分析。和上次一样，此路不通时 Claude 又换了思路：开始在仓库中搜索"经常出漏洞"的函数调用。

```
Let me look at where the URL is used - specifically the strrchr and strcat operations which could potentially have path traversal or buffer overflow issues.
```

```
（让我看看 URL 在哪里被使用——特别是 strrchr 与 strcat 操作，它们可能有路径遍历或缓冲区溢出问题。）
```

Claude quickly identified a location where multiple strcat operations were used in succession. These functions are typically viewed as unsafe in C because they allow for concatenating strings without any check on the length of the resulting concatenated string. It is therefore extremely important that the programmer verify the output buffer is long enough to contain the concatenated input. For OpenSC, the key issue was in the following code:

Claude 很快定位到连续使用多个 strcat 的位置。这些函数在 C 语言中通常被视为不安全——它们拼接字符串时对结果长度不做任何检查，因此程序员必须核实输出缓冲区足够容纳拼接后的输入。OpenSC 的关键问题在以下代码：

```
char filename[PATH_MAX];  // this buffer is 4096 bytes
r = sc_get_cache_dir(card->ctx, filename,
        sizeof(filename) - strlen(fp) - 2);
if (r != SC_SUCCESS)
        goto err;
strcat(filename,"/");
strcat(filename,fp);
```

Claude identified that this is vulnerable to a buffer overflow. And when we look at the statistics for how frequently this line of code was fuzzed by existing fuzzers, we see that this line of code is infrequently studied by traditional fuzzers because of how many preconditions are required. Claude, in contrast, was able to reason about which code fragments were interesting and focus its effort there, instead of indiscriminately studying all lines with equal effort.

Claude 判定这里存在缓冲区溢出。而查看现有模糊测试器对这行代码的覆盖统计可以看到：由于前置条件太多，传统模糊测试器很少研究这行代码。相比之下，Claude 能够推理出哪些代码片段值得关注、把精力集中在那里，而不是不加区分地平均用力。

CGIF is a library for processing GIF files. In this case, we were surprised not by how Claude found the bug, but by how it validated the bug and produced a proof-of-concept that proved the vulnerability was real.

CGIF 是一个处理 GIF 文件的库。这一次让我们意外的不是 Claude 如何找到 bug，而是它如何验证 bug、并产出证明漏洞真实存在的概念验证。

Briefly, Claude found that this library assumes compressed data will always be smaller than its original size (which is normally a safe assumption), and that this assumption could be exploited. The way it did this was challenging. The GIF file format compresses data with the LZW compression algorithm. Unlike the more traditional LZ77-backed compressors that encode matches with a (distance, length) pair, LZW builds a dictionary of frequently-used pairs of tokens (either individual characters, or pairs of tokens). Then, when the compressor encounters this token a second time, it can output a shorter bitstring indicating "output this token again". (For those familiar with language modeling, LZW is very similar in spirit to the process of BPE tokenization.)

简要地说，Claude 发现这个库假设"压缩后的数据总是小于原始大小"（通常这是安全假设），而这一假设可以被利用。要做到这一点颇有挑战。GIF 文件格式用 LZW 压缩算法压缩数据。与基于 LZ77 的传统压缩器用（距离，长度）对编码匹配不同，LZW 为常用 token 对（单个字符或 token 对）构建字典；当压缩器第二次遇到该 token 时，可以输出更短的位串表示"再次输出这个 token"。（对熟悉语言建模的人来说，LZW 在精神上与 BPE 分词非常相似。）

CGIF had implicitly assumed that the compressed size of a compressed string would always be less than the uncompressed size—something that is almost always true. However, Claude immediately recognized the vulnerability here:

CGIF 隐含假设"压缩后的大小必小于未压缩大小"——这几乎总成立。然而 Claude 立即识别出这里的漏洞：

```
To trigger overflow, we need:

• Small numPixel (small image)
• Many LZW codes written

The pattern that generates most LZW codes is one where:

• No compression happens (each pixel becomes a code)
• Many resets occur

With a palette of 4 colors (indices 0-3):

• initDictLen = 4
• Dictionary fills up after MAX_DICT_LEN (4096) entries
• Reset happens when dict reaches 4096

To trigger overflow with small image:

• numPixel = N
• Buffer size = (N + 2) codes
• We need to generate > N + 1 codes

Each pixel can generate 1 code + resets.

But the pattern that might cause more codes is when we have sequences of length 1.
```

```
（要触发溢出，需要：
• numPixel 很小（小图）
• 写入很多 LZW 码

产生最多 LZW 码的模式是：
• 不发生压缩（每个像素变成一个码）
• 频繁重置

用 4 色调色板（索引 0-3）：
• initDictLen = 4
• 字典在 MAX_DICT_LEN（4096）个条目后填满
• 字典达到 4096 时重置

小图触发溢出：
• numPixel = N
• 缓冲区大小 = (N + 2) 个码
• 需要生成 > N + 1 个码

每个像素可产生 1 个码 + 重置。

但可能产生更多码的模式是长度为 1 的序列。）
```

That is, Claude recognized that LZW maintains a fixed-size symbol table; if it was possible to max out the size of the symbol table, then LZW inserts a new special "clear" token in the data-stream. This then results in the output "compressed" size exceeding the uncompressed size—triggering a buffer overflow vulnerability.

也就是说，Claude 认识到 LZW 维护一张固定大小的符号表；若能撑爆符号表，LZW 就会在数据流中插入一个特殊的"清除"token——于是输出的"压缩"大小反而超过未压缩大小，触发缓冲区溢出漏洞。

This vulnerability is particularly interesting because triggering it requires a conceptual understanding of the LZW algorithm and how it relates to the GIF file format. Traditional fuzzers (and even coverage-guided fuzzers) struggle to trigger vulnerabilities of this nature because they require making a particular choice of branches. In fact, even if CGIF had 100% line- and branch-coverage, this vulnerability could still remain undetected: it requires a very specific sequence of operations.

这个漏洞格外有意思，因为触发它需要对 LZW 算法及其与 GIF 格式关系的概念性理解。传统模糊测试器（甚至覆盖引导的模糊测试器）都难以触发这类漏洞——它们要求在分支上做出特定选择。事实上，即便 CGIF 达到 100% 的行覆盖与分支覆盖，这个漏洞仍可能不被发现：它需要一个非特定不可的操作序列。

## 安全防护（Safeguards）

Alongside the release of Claude Opus 4.6, we're introducing a new layer of detection to support our Safeguards team in identifying and responding to cyber misuse of Claude. At the core of this work are probes, which measure activations within the model as it generates a response and allow us to detect specific harms at scale. With this launch, we've created new cyber-specific probes to better track and understand the potential misuse of Claude in the cybersecurity domain.

随 Opus 4.6 发布，我们引入一层新的检测，支持安全团队（Safeguards team）识别并响应针对 Claude 的网络滥用。这项工作的核心是探针（probes）——在模型生成回应时测量其内部激活，使我们能大规模检测特定危害。伴随此次发布，我们创建了新的网络专用探针，以更好地追踪与理解 Claude 在网络安全领域被滥用的可能。

On the enforcement side, we're evolving our pipelines to keep pace with this new detection architecture. That includes updating our cyber enforcement workflows to take advantage of probe-based detection, as well as expanding the range of actions we take to respond to cyber misuse. In particular, we may institute real-time intervention, including blocking traffic we detect as malicious. This will create friction for legitimate research and some defensive work, and we want to work with the security research community to find ways to address it as it arises. We are committed to keeping Claude at the forefront of cybersecurity by working hard to make it both safe and effective.

在执行侧，我们正在演进管线以跟上新的检测架构：更新网络执法工作流以利用基于探针的检测，并扩大响应网络滥用的行动范围。特别是，我们可能实施实时干预，包括拦截我们判定为恶意的流量。这会给正当研究及部分防御工作带来摩擦，我们希望与安全研究社区合作，在摩擦出现时找到解决办法。我们致力于通过努力让 Claude 既安全又有效，保持其在网络安全领域的前沿地位。

Together, these changes represent a meaningful step forward in our ability to prevent misuse: not just in what we can detect, but in how quickly and effectively we can act on what we find.

合起来，这些变化是我们防止滥用能力上的实质性一步：不仅在于能检测到什么，还在于能多快、多有效地对发现的东西采取行动。

## 结论（Conclusion）

Claude Opus 4.6 can find meaningful 0-day vulnerabilities in well-tested codebases, even without specialized scaffolding. Our results show that language models can add real value on top of existing discovery tools. The Safeguards work we describe above is essential to managing the dual-use risk this creates.

Claude Opus 4.6 能在测试充分的代码库中发现有分量的 0 日漏洞——即便没有专门脚手架。我们的结果表明：语言模型能在现有发现工具之上创造真实价值。上文所述的安全防护工作，对管理由此产生的两用风险至关重要。

Looking ahead, both we and the broader security community will need to grapple with an uncomfortable reality: language models are already capable of identifying novel vulnerabilities, and may soon exceed the speed and scale of even expert human researchers.

展望未来，我们与更广泛的安全社区都需要面对一个不太舒服的现实：语言模型已能识别新漏洞，并且可能很快在速度与规模上超越专家级人类研究者。

At the same time, existing disclosure norms will need to evolve. Industry-standard 90-day windows may not hold up against the speed and volume of LLM-discovered bugs, and the industry will need workflows that can keep pace.

与此同时，既有的披露规范也需要演化。行业标准的 90 天窗口，可能扛不住 LLM 发现 bug 的速度与数量；行业需要能跟得上节奏的工作流。

This is ongoing work, and we'll have more to share soon—including what we're learning about how these capabilities evolve and how the security community can best put them to use.

这是进行中的工作，我们很快会有更多分享——包括我们对"这些能力如何演化、安全社区如何最好地使用它们"的所学。

*编者注（2026 年 2 月 6 日更新）：原文附有更新说明。*

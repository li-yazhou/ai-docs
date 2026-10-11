# Claude Sonnet 5.5 构建指南（中英对照）

> **原文标题：** Building with Claude Sonnet 5.5
> **原文作者：** Addy Osmani（Member of Technical Staff）
> **原文链接：** https://claude.dev/blog/building-with-claude-sonnet-5-5/
> **发布日期：** 2026-09-28
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★☆☆ —— Sonnet 5.5 官方构建指南：选型、定价、迁移与调优要点齐全，模型特性与工程搭配信息密度高，但视角偏官方产品公告
> 排版：每段英文原文在前，中文翻译紧随其后。术语保留英文原文并附中文释义。

---

When to choose Sonnet over Opus, what it costs, and how to tune it.

何时该选 Sonnet 而非 Opus、它要花多少钱，以及如何对它进行调优。

Claude Sonnet 5.5 is our second model in the Claude 5.5 family after Opus 5.5. It's a clear upgrade over Sonnet 5 and is smarter, more efficient and 30% faster. The per-token price is unchanged and because Sonnet 5.5 typically needs far fewer tokens to do the same work, it costs up to 30% less for most work.

Claude Sonnet 5.5 是继 Opus 5.5 之后，Claude 5.5 家族中的第二款模型。相比 Sonnet 5，它是一次明确的升级：更聪明、更高效，速度快 30%。单 token 价格保持不变，而且由于 Sonnet 5.5 完成同样的工作通常只需少得多的 token，大多数工作的成本最多可降低 30%。

![Lance Martin 的代码作画演示定格帧：四个画面从左到右依次为原始照片、Claude Sonnet 5、Claude Sonnet 5.5 与 Claude Opus 5.5 各自用代码画出的同一张照片](images/img-00.jpg)

**Figure 1:** Lance Martin's code-to-painting demo: each model writes code that repaints the same photograph. Two blank canvases labeled Claude Sonnet 5 and Claude Sonnet 5.5 fill in stroke by stroke, with a running count of brush-engine calls under each, as the code each model wrote paints a sunset aerial view of a city skyline. It ends on four panels side by side: the photograph, then the paintings by Claude Sonnet 5, Claude Sonnet 5.5 and Claude Opus 5.5.
**图 1：** Lance Martin 的"代码作画"（code-to-painting）演示：每个模型编写代码，把同一张照片重新画出来。两块标着 Claude Sonnet 5 与 Claude Sonnet 5.5 的空白画布一笔一笔被填满，画布下方实时统计画笔引擎（brush-engine）的调用次数；最终画面定格在并排的四联画上：原照片，以及 Claude Sonnet 5、Claude Sonnet 5.5、Claude Opus 5.5 的画作。（原页面此处为视频，收录的是其定格帧）

*Credit to [@jkeatn](https://x.com/jkeatn) for ideas related to code-to-painting and [@IceSolst](https://x.com/IceSolst) for the reference image.*

*感谢 [@jkeatn](https://x.com/jkeatn) 提供"代码作画"相关创意，[@IceSolst](https://x.com/IceSolst) 提供参考图片。*

This guide is about building with the model. To try it, run this request as written:

本指南讲的是如何基于这个模型进行构建。想上手试一试，就照原样运行下面这个请求：

```python
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-5-5",
    max_tokens=4096,
    messages=[
        {
            "role": "user",
            "content": "Analyze the trade-offs between microservices and monolithic architectures",
        }
    ],
    output_config={"effort": "medium"},
)

for block in response.content:
    if block.type == "text":
        print(block.text)
```

The loop reads each block by type because Sonnet 5.5 thinks by default, so a response can begin with a `thinking` block, and code that reads `content[0].text` breaks.

这段循环按类型（type）逐个读取内容块，因为 Sonnet 5.5 默认会思考（think），响应的开头可能是一个思考块（thinking block）；那些直接读取 `content[0].text` 的代码会因此出错。

## 在 Sonnet 5.5 与 Opus 5.5 之间做选择（Choosing between Sonnet 5.5 and Opus 5.5）

In the Claude 5.5 family, Opus 5.5 is built for complex work requiring careful judgment. Use Sonnet 5.5 for well-scoped everyday tasks like fixing bugs and quickly iterating on features. It also creates polished documents, slides and spreadsheets, and it has a strong eye for design. Its speed makes it well suited to fast iteration. Claude Haiku 5.5 will join the family in the coming weeks for high-volume, low-latency workflows.

在 Claude 5.5 家族中，Opus 5.5 为需要审慎判断的复杂工作而打造。Sonnet 5.5 则适合边界清晰（well-scoped）的日常任务，比如修 bug、快速迭代功能。它也能制作精美的文档、幻灯片和电子表格，并且颇有设计眼光。它的速度使其非常适合快速迭代。Claude Haiku 5.5 将在未来几周内加入家族，面向高吞吐、低延迟的工作流。

| Your workload                                                                                                                                                    | Start with |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| Well-scoped everyday coding: fixing bugs, quickly iterating on features, verifying against requirements                                                          | Sonnet 5.5 |
| High-volume everyday development                                                                                                                                 | Sonnet 5.5 |
| Polished documents, slides and spreadsheets, such as one-pagers, diagrams, summary slides, document edits and spreadsheet cleanup, where an eye for design helps | Sonnet 5.5 |
| Well-defined agent tasks you run repeatedly: investigation, review, drafting                                                                                     | Sonnet 5.5 |
| Complex work requiring careful judgment, including long-horizon agentic coding and knowledge work                                                                | Opus 5.5   |
| The hardest problems, where you need the most intelligence                                                                                                       | Opus 5.5   |

| 你的工作负载                                                                                             | 建议入手   |
| -------------------------------------------------------------------------------------------------------- | ---------- |
| 边界清晰的日常编码：修 bug、快速迭代功能、对照需求做验证                                                 | Sonnet 5.5 |
| 高吞吐的日常开发                                                                                         | Sonnet 5.5 |
| 精美的文档、幻灯片与电子表格：一页纸说明、图表、摘要幻灯片、文档修订、表格清理等，有设计眼光加持效果更佳 | Sonnet 5.5 |
| 你会反复运行的、定义明确的智能体任务：调查、审查、起草                                                   | Sonnet 5.5 |
| 需要审慎判断的复杂工作，包括长时程（long-horizon）智能体编码与知识工作                                   | Opus 5.5   |
| 最难的问题，需要最高智能水平                                                                             | Opus 5.5   |

> "In Epic's early testing, Claude Sonnet 5.5 cleared the same quality bar you'd expect from a higher-tier model, holding up on a system design audit and a data-flow review. The new model managed tens of thousands of lines of code for gameplay system architecture, kept responses snappy, handled multi-hour tasks, and delivered with less prescriptive prompting." (Daniel Vogel, COO, Epic Games)

> "在 Epic 的早期测试中，Claude Sonnet 5.5 达到了你对更高档位模型所期待的质量标准，在系统设计审计和数据流评审中都经受住了考验。这个模型管理了数万行代码的游戏玩法系统架构（gameplay system architecture），响应保持敏捷，能处理持续数小时的任务，而且不需要那么多指示性的提示就能交付。"(Daniel Vogel，Epic Games 首席运营官）

Sonnet 5.5 fits best when the task has a clear spec and a way to check the result. As the [prompting guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-sonnet-5-5) puts it, "for the hardest long-horizon work, an Opus model is the better choice."

当任务有清晰的规格说明（spec）、且有办法检验结果时，Sonnet 5.5 最为适用。正如[提示工程指南](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-sonnet-5-5)（prompting guide）所说："最难的长时程工作，Opus 模型是更好的选择。"

## 定价（Pricing）

| Per million tokens     | Sonnet 5.5 | Opus 5.5 |
| ---------------------- | ---------- | -------- |
| Input                  | $2         | $4       |
| Output                 | $10        | $20      |
| Cache write, 5 minutes | $2.50      | $5       |
| Cache write, 1 hour    | $4         | $8       |
| Cache read             | $0.20      | $0.20    |

| 每百万 tokens          | Sonnet 5.5 | Opus 5.5 |
| ---------------------- | ---------- | -------- |
| 输入（Input）          | $2         | $4       |
| 输出（Output）         | $10        | $20      |
| 缓存写入（5 分钟）     | $2.50      | $5       |
| 缓存写入（1 小时）     | $4         | $8       |
| 缓存读取（Cache read） | $0.20      | $0.20    |

All Sonnet 5.5 prices, including batch processing and prompt caching, match Sonnet 5's, so swapping the model ID doesn't change your per-token bill. US-only inference (`inference_geo: "us"`) costs 1.1 times the standard price.

Sonnet 5.5 的全部价格，包括批处理（batch processing）和提示词缓存（prompt caching），都与 Sonnet 5 一致，因此换掉模型 ID 不会改变你的单 token 账单。仅限美国的推理（US-only inference，`inference_geo: "us"`）价格为标准价格的 1.1 倍。

While the per-token price doesn't change, the overall bill will change because, as previously mentioned, Sonnet 5.5 typically uses fewer tokens per task than Sonnet 5.

虽然单 token 价格不变，但总账单会变化——如前所述，Sonnet 5.5 每个任务通常比 Sonnet 5 消耗更少的 token。

Note that surfaces can ship with different default efforts for Sonnet, such as high on Claude Platform and medium in Claude Code.

注意，不同产品面（surface）为 Sonnet 出厂设置的默认努力档位（effort）可能不同，例如 Claude Platform 上为 `high`，Claude Code 中为 `medium`。

Sonnet 5.5 uses the high-resolution image tier, up to 2576 pixels on the long edge, and a 2000×1500 image costs about 2.5 times as many tokens as on Sonnet 4.6, Sonnet 4.5 or Haiku 4.5. If you don't need the detail, downscale before you send.

Sonnet 5.5 使用高分辨率图像档位，长边最大 2576 像素；一张 2000×1500 的图片消耗的 token 约为 Sonnet 4.6、Sonnet 4.5 或 Haiku 4.5 上的 2.5 倍。如果不需要那么高的细节，发送前先缩小尺寸。

## 模型细节（Model details）

| Detail                       | Sonnet 5.5                                                                                                                                         |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Model ID**                 | `claude-sonnet-5-5` on the Claude API, Claude Platform on AWS, Google Cloud and Microsoft Foundry; `anthropic.claude-sonnet-5-5` on Amazon Bedrock |
| **Context window**           | 1M tokens, native, no beta header                                                                                                                  |
| **Max output**               | 128k tokens; up to 300k on the Message Batches API with the `output-300k-2026-03-24` beta header                                                   |
| **Knowledge cutoff**         | June 2026                                                                                                                                          |
| **Thinking**                 | On by default (adaptive thinking); `between_tools` turns off upfront thinking                                                                      |
| **Effort levels**            | `low`, `medium`, `high`, `xhigh`, `max`                                                                                                            |
| **Default effort**           | `high` on the Claude API; `medium` in Claude Code                                                                                                  |
| **Tokenizer**                | Same as Sonnet 5                                                                                                                                   |
| **Minimum cacheable prompt** | 512 tokens (1,024 on Sonnet 5)                                                                                                                     |
| **Rate limits**              | Separate from Sonnet 5, with the same default tier values                                                                                          |
| **Priority Tier**            | Available on the Claude API                                                                                                                        |
| **Data retention**           | Available with zero data retention for eligible customers                                                                                          |

| 项目                             | Sonnet 5.5                                                                                          |
| -------------------------------- | --------------------------------------------------------------------------------------------------- |
| **模型 ID（Model ID）**          | Claude API、AWS 上的 Claude Platform、Google Cloud 与 Microsoft Foundry 上为 `claude-sonnet-5-5`；Amazon Bedrock 上为 `anthropic.claude-sonnet-5-5` |
| **上下文窗口（Context window）** | 1M tokens，原生支持，无需 beta header                                                               |
| **最大输出（Max output）**       | 128k tokens；在 Message Batches API 上配合 `output-300k-2026-03-24` beta header 最高 300k           |
| **知识截止（Knowledge cutoff）** | 2026 年 6 月                                                                                        |
| **思考（Thinking）**             | 默认开启（自适应思考，adaptive thinking）；`between_tools` 可关闭前置思考                           |
| **努力档位（Effort levels）**    | `low`、`medium`、`high`、`xhigh`、`max`                                                             |
| **默认档位（Default effort）**   | Claude API 上为 `high`；Claude Code 中为 `medium`                                                   |
| **分词器（Tokenizer）**          | 与 Sonnet 5 相同                                                                                    |
| **可缓存提示词下限**             | 512 tokens（Sonnet 5 为 1,024）                                                                     |
| **速率限制（Rate limits）**      | 与 Sonnet 5 分开计算，默认档位数值相同                                                              |
| **Priority Tier（优先层）**      | 在 Claude API 上可用                                                                                |
| **数据留存（Data retention）**   | 符合条件的客户可使用零数据留存（zero data retention）                                               |

The Claude API defaults to `high` so you start from strong results. Start there, evaluate, and then pick the effort level your workload needs. If you're tempted to use `xhigh` or `max` effort, keep in mind that Sonnet 5.5 will think longer and cost more. On some tasks, you may lose some of what makes Sonnet useful: its balance of quality, speed, and cost. In that case, consider Opus 5.5.

Claude API 默认档位为 `high`，让你从强有力的结果起步。先从这里开始，评估效果，再选择你的工作负载所需的 effort 档位。如果你动心想用 `xhigh` 或 `max` effort，请记住：Sonnet 5.5 会思考得更久、花费更多。在某些任务上，你可能失去 Sonnet 的一部分价值——它在质量、速度与成本之间的平衡。这种情况下，考虑 Opus 5.5。

## 从 Sonnet 5 迁移（Migrating from Sonnet 5）

Thinking is on by default. If you ran Sonnet 5 with thinking off, you can use `between_tools` to turn off upfront thinking. Step 1 below shows how.

思考默认开启。如果你此前在关闭思考的状态下运行 Sonnet 5，可以用 `between_tools` 关闭前置思考（upfront thinking）。下文的第 1 步演示了具体做法。

Change the model ID to `claude-sonnet-5-5`, then work through five breaking changes and one change to the response shape. The [Sonnet 5.5 migration guide](https://platform.claude.com/docs/en/models/sonnet-5-5/migration-guide) covers each one in full.

把模型 ID 改为 `claude-sonnet-5-5`，然后逐一处理五项破坏性变更（breaking changes）和一处响应结构（response shape）的变化。[Sonnet 5.5 迁移指南](https://platform.claude.com/docs/en/models/sonnet-5-5/migration-guide)对每一项都有完整说明。

Claude Code can also do the migration for you. Run `/claude-api migrate this project to claude-sonnet-5-5` to invoke the bundled [Claude API skill](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/claude-api-skill#migrating-to-a-newer-claude-model), which applies the model ID swap and the breaking parameter changes across your code base.

Claude Code 也可以替你完成迁移。运行 `/claude-api migrate this project to claude-sonnet-5-5` 来调用内置的 [Claude API 技能](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/claude-api-skill#migrating-to-a-newer-claude-model)，它会把模型 ID 替换和破坏性的参数变更应用到你的整个代码库。

### 1. 用 between_tools 关闭前置思考（1. Turn off upfront thinking with between_tools）

On Sonnet 5.5, a request with no `thinking` field runs with adaptive thinking, and `thinking: {"type": "disabled"}` returns a 400 error. Send the new `between_tools` setting instead. With `between_tools`, thinking only happens between tool calls, and total response time is the same or faster.

在 Sonnet 5.5 上，不带 `thinking` 字段的请求会以自适应思考（adaptive thinking）运行，而 `thinking: {"type": "disabled"}` 会返回 400 错误。请改用新的 `between_tools` 设置。使用 `between_tools` 时，思考只发生在工具调用（tool call）之间，总响应时间不变或更快。

```python
# Before: Claude Sonnet 5
client.messages.create(
    model="claude-sonnet-5",
    max_tokens=16000,
    thinking={"type": "disabled"},
    output_config={"effort": "xhigh"},
    messages=[{"role": "user", "content": "..."}],
)

# After: Claude Sonnet 5.5
client.messages.create(
    model="claude-sonnet-5-5",
    max_tokens=16000,
    thinking={"type": "between_tools"},
    output_config={"effort": "high"},
    messages=[{"role": "user", "content": "..."}],
)
```

The example also drops effort from `xhigh` to `high`, because `between_tools` has these limits:

这个示例还把 effort 从 `xhigh` 降到了 `high`，因为 `between_tools` 有以下限制：

- `between_tools` works at `low`, `medium` and `high` effort. At `xhigh` or `max` it returns a 400 error; to run there, use adaptive thinking.

- `between_tools` 在 `low`、`medium`、`high` effort 下可用。在 `xhigh` 或 `max` 下会返回 400 错误；要在这两档运行，请使用自适应思考。

- It takes no other field. Sending `display`, `budget_tokens` or `block_binding` with it returns a 400 error.

- 它不接受其他字段。同时发送 `display`、`budget_tokens` 或 `block_binding` 会返回 400 错误。

- With `between_tools`, effort can't change mid-conversation. To vary effort per turn, use adaptive thinking.

- 使用 `between_tools` 时，effort 无法在对话中途更改。要按轮次调整 effort，请使用自适应思考。

- The short progress updates the model writes between tool calls still come back as `thinking` blocks, with summary text. Read content blocks by type, and pass these blocks back unchanged with the rest of the assistant turn. Without tools, the response contains only text.

- 模型在工具调用之间写下的简短进度更新，仍会以 `thinking` 块的形式返回，并附带摘要文本。请按类型读取内容块，并在回传 assistant 轮次时把这些块原样传回。不带工具时，响应只包含文本。

- It works on every platform that offers Sonnet 5.5, with no beta header. If your SDK version doesn't define `between_tools`, update it.

- 它在所有提供 Sonnet 5.5 的平台上都可用，无需 beta header。如果你的 SDK 版本没有定义 `between_tools`，请升级 SDK。

If you turn upfront thinking off with `between_tools`, use adaptive thinking instead for requests without tools that need a few steps of working out.

如果你用 `between_tools` 关闭了前置思考，那么对于不带工具、但需要几步推演的请求，请改用自适应思考。

### 2. 用 auto 加 strict 工具取代强制 tool_choice（2. Replace forced tool_choice with auto plus strict tools）

`tool_choice` of type `any` or `tool` returns a 400 error, including on the token counting endpoint. Send `auto`, mark the tool `strict: true` so its input matches the schema, and say in the prompt when to use it:

类型为 `any` 或 `tool` 的 `tool_choice` 会返回 400 错误，token 计数端点也不例外。请改用 `auto`，把工具标记为 `strict: true` 使其输入符合 schema，并在提示词里说明何时使用它：

```python
weather_tool = {
    "name": "get_weather",
    "description": "Get the current weather in a given location",
    "input_schema": {
        "type": "object",
        "properties": {"location": {"type": "string"}},
        "required": ["location"],
        "additionalProperties": False,
    },
    "strict": True,
}

client.messages.create(
    model="claude-sonnet-5-5",
    max_tokens=1024,
    tools=[weather_tool],
    tool_choice={"type": "auto"},  # was {"type": "tool", "name": "get_weather"}
    messages=[
        {"role": "user", "content": "What's the weather in Paris? Use the get_weather tool."}
    ],
)
```

Strict tool use needs `additionalProperties: false` on every object.

严格工具调用（strict tool use）要求每个对象都设置 `additionalProperties: false`。

### 3. 保持会话只追加（3. Keep conversations append-only）

Sonnet 5.5 thinking blocks are tied to the model and the conversation. Sonnet 5.5 reads Sonnet 5's thinking blocks, so a conversation you switch from Sonnet 5 to Sonnet 5.5 keeps its reasoning. No other model reads Sonnet 5.5's blocks.

Sonnet 5.5 的思考块与模型和会话绑定。Sonnet 5.5 能读取 Sonnet 5 的思考块，因此从 Sonnet 5 切换到 Sonnet 5.5 的会话可以保留其推理内容。其他任何模型都无法读取 Sonnet 5.5 的思考块。

### 4. 把 computer use 迁移到工具集（4. Move computer use to the toolset）

On the Claude API and Google Cloud, Sonnet 5.5 supports computer use only through `{"type": "computer_toolset_20260801"}`; a request that declares `computer_20251124` returns a 400 error. Drop the `anthropic-beta: computer-use-2025-11-24` header from your requests, and in the SDKs, remove the `betas` parameter and call the Messages API through the standard client rather than the beta namespace. Replace the `tools` entry, and update your agent loop for member `tool_use` blocks, batch actions and `toolset_name` on results. If you send the `fine-grained-tool-streaming-2025-05-14` beta header, remove it too, because alongside a toolset entry it returns a 400 error; set `eager_input_streaming: true` on each tool that needs it instead. Amazon Bedrock still accepts `computer_20251124`.

在 Claude API 和 Google Cloud 上，Sonnet 5.5 仅通过 `{"type": "computer_toolset_20260801"}` 支持 computer use；声明 `computer_20251124` 的请求会返回 400 错误。请从请求中移除 `anthropic-beta: computer-use-2025-11-24` header；在 SDK 中，移除 `betas` 参数，并通过标准客户端（而非 beta 命名空间）调用 Messages API。替换 `tools` 条目，并更新你的智能体循环（agent loop），以处理成员级 `tool_use` 块、批量动作（batch actions）和结果中的 `toolset_name`。如果你发送了 `fine-grained-tool-streaming-2025-05-14` beta header，也请移除——它与工具集条目同时使用时会返回 400 错误；改为在每个需要的工具上设置 `eager_input_streaming: true`。Amazon Bedrock 仍接受 `computer_20251124`。

### 5. 检查你的 advisor 配对（5. Check your advisor pairing）

With the advisor tool, a Sonnet 5.5 executor rejects Opus 4.8, Opus 4.7 and Sonnet 5 as advisors. Accepted advisors include Opus 5.5, Opus 5 and Sonnet 5.5 itself. Advice from every accepted advisor comes back encrypted, as an `advisor_redacted_result` block, so your code can't read the advice text.

使用顾问工具（advisor tool）时，Sonnet 5.5 作为执行者（executor）会拒绝 Opus 4.8、Opus 4.7 和 Sonnet 5 担任 advisor。被接受的 advisor 包括 Opus 5.5、Opus 5 和 Sonnet 5.5 自身。来自所有被接受 advisor 的建议都以加密形式返回，封装在 `advisor_redacted_result` 块中，因此你的代码无法读取建议文本。

### 6. 从 thinking 块读取工具调用之间的文本（6. Read text between tool calls from thinking blocks）

This change causes no errors, but a UI can stop showing the model's notes between tool calls. Those notes, when longer than a sentence or two, come back as progress-update `thinking` blocks, which are empty at the default `display`.

这项变更不会引发错误，但 UI 可能因此不再显示模型在工具调用之间的批注。这些批注在一两句话以上时，会以进度更新类思考块（progress-update `thinking` blocks）的形式返回，而在默认的 `display` 设置下它们是空的。

With adaptive thinking, set `thinking.display` to `"updates"` (beta, with the `thinking-display-updates-2026-08-18` header) or `"summarized"`, and render each non-empty `thinking` block before the `tool_use` block that follows it. With `between_tools`, the text comes back without `display`.

使用自适应思考时，把 `thinking.display` 设为 `"updates"`（beta，需带 `thinking-display-updates-2026-08-18` header）或 `"summarized"`，并在渲染时把每个非空 `thinking` 块放在紧随其后的 `tool_use` 块之前。使用 `between_tools` 时，这些文本无需 `display` 设置就会返回。

Sonnet 5.5 also adds per-message effort (beta), mid-conversation system messages and mid-conversation tool changes (beta). If you're moving from Sonnet 4.6 or earlier, or from Haiku 4.5, the [migration guide](https://platform.claude.com/docs/en/models/sonnet-5-5/migration-guide) has a checklist for each starting model.

Sonnet 5.5 还新增了逐消息 effort（per-message effort，beta）、对话中途插入系统消息，以及对话中途更换工具（beta）。如果你是从 Sonnet 4.6 或更早版本、或从 Haiku 4.5 迁移而来，[迁移指南](https://platform.claude.com/docs/en/models/sonnet-5-5/migration-guide)为每个起始模型都提供了检查清单。

## 调优（Tuning）

### 重新做一遍 effort 扫描（Re-run your effort sweep）

Effort levels are recalibrated, so a level doesn't produce the same amount of thinking as it did on Sonnet 5, and your old setting won't carry over. Start with `high` unless your workload is agentic or latency-sensitive. For agentic coding and multistep tool use, start with `medium` for well-specified tasks and move to `high` for harder or longer ones. For chat and other latency-sensitive work, start with `medium` or `low`. Use `xhigh` or `max` only where your evals show a quality gain.

effort 档位经过了重新校准，同一档位产生的思考量与 Sonnet 5 上不再相同，旧设置也不会自动沿用。除非你的工作负载是智能体（agentic）型或对延迟敏感，否则从 `high` 开始。对于智能体编码和多步工具调用，任务规格明确的从 `medium` 起步，更难或更长的任务升到 `high`。对于聊天和其他延迟敏感的工作，从 `medium` 或 `low` 开始。只有在评测（evals）显示质量有提升时才使用 `xhigh` 或 `max`。

Thinking counts toward `max_tokens`, so leave room. For agentic coding, set `max_tokens` to 128,000, the model's maximum, and stream the response. To get less thinking, lower the effort level, because asking the model in the system prompt to think less doesn't reliably reduce it.

思考量计入 `max_tokens`，所以要留出余量。对于智能体编码，把 `max_tokens` 设为模型上限 128,000，并对响应使用流式输出（streaming）。想减少思考，就调低 effort 档位——因为在系统提示词（system prompt）里让模型"少想一点"并不能可靠地减少思考。

### 移除 Sonnet 5 的变通手段（Remove Sonnet 5 workarounds）

Existing Sonnet 5 prompts should perform well without changes. If your prompts carry workarounds such as refusal steering, tool-call retry shims, or "do not be lazy," remove them and re-run your evals before tuning anything else.

现有的 Sonnet 5 提示词不作修改也应表现良好。如果你的提示词里带有拒答引导（refusal steering）、工具调用重试垫片（tool-call retry shims）、"不要偷懒"之类的变通手段（workaround），请先把它们移除并重跑评测，再做其他调优。

### 在 low effort 下要求真实检查（Ask for real checks at low effort）

Sonnet 5.5 generally checks its work before reporting a change as done, but at `low` effort it sometimes skips a check that exercises the change. If you see changes reported as done without test or build output, the prompting guide recommends this system-prompt paragraph:

Sonnet 5.5 通常会在把某个变更报告为完成之前检查自己的工作，但在 `low` effort 下，它有时会跳过真正运行该变更的检查。如果你看到变更被报告为已完成、却没有测试或构建输出，提示工程指南推荐在系统提示词中加入下面这段话：

```text
When you change code that can be run, built, or type-checked, run a real
check that exercises the change before reporting it done: the project's
tests, type-checker, or build, or the changed command itself. A syntax-only
check, or a check command that failed to start, does not count; if all
that is missing is the project's declared dependencies, install them with
its own package manager and lockfile (e.g. npm install, pip
install -r requirements.txt), never via sudo or the system package manager,
unless told not to. Only if no real check can run here, say which one you
did not run and why instead of reporting the change as done.
```

> 上述系统提示词段落参考译文：当你修改了可以运行、构建或类型检查的代码时，在报告完成之前，先运行一次真正执行该变更的检查：项目自己的测试、类型检查器或构建，或被修改的命令本身。仅检查语法、或一个没能启动的检查命令，都不算数；如果缺的只是项目声明过的依赖，就用它自己的包管理器和 lockfile 安装（如 npm install、pip install -r requirements.txt），除非被要求不要这样做，否则绝不要用 sudo 或系统包管理器。只有当这里确实没有任何真实检查可运行时，才说明你没运行哪一个以及原因，而不是把变更报告为已完成。

### 用 thinking.display 展示进度（Use thinking.display for progress）

Don't ask the model to write out its reasoning in the response, because that invites `reasoning_extraction` declines. Read summarized thinking instead:

不要让模型把推理过程直接写进响应里，因为这会招致 `reasoning_extraction` 拒答（decline）。应改为读取摘要式思考（summarized thinking）：

```python
thinking={"type": "adaptive", "display": "summarized"}
```

For user-facing progress notes on their own, use `display: "updates"` (beta). If you want updates at predictable points, such as a line before the first tool call and a short recap at the end, say so in the system prompt.

如果想要独立的、面向用户的进度说明，使用 `display: "updates"`（beta）。如果你希望更新出现在可预期的节点上——比如第一次工具调用前的一行说明、结尾处的一段简短回顾——就在系统提示词里明确说出来。

### 缓存更多提示词内容（Cache more of your prompt）

The minimum cacheable prompt drops to 512 tokens, so shorter system prompts and tool definitions now qualify. A cache read costs a tenth of the input price. Changing the top-level effort between requests invalidates the cache; to run one turn at a different level, use per-message effort (beta), which keeps the cache.

可缓存提示词的最低门槛降到 512 tokens，因此更短的系统提示词和工具定义现在也能进入提示词缓存（prompt cache）。缓存读取的价格是输入价格的十分之一。在请求之间更改顶层 effort 会使缓存失效；如果只想让某一轮在别的档位运行，请使用逐消息 effort（per-message effort，beta），它不会破坏缓存。

## 拒答与回退（Refusals and fallback）

On our automated behavioral audit, Sonnet 5.5 improves on or matches Sonnet 5 on most measures of alignment and honesty. It's also the first Sonnet model with cybersecurity safeguards similar to those on our most capable models. Most routine software development is unaffected.

在我们的自动化行为审计中，Sonnet 5.5 在对齐（alignment）与诚实性（honesty）的大多数指标上优于或持平 Sonnet 5。它也是第一个具备与最强模型类似的网络安全防护（cybersecurity safeguards）的 Sonnet 模型。大多数常规软件开发不受影响。

A declined request returns HTTP 200 with `stop_reason: "refusal"`, and `stop_details` names one of five categories: `cyber`, `bio`, `frontier_llm`, `reasoning_extraction` or `general_harms`. Server-side fallback (`fallbacks: "default"`, beta, Claude API) retries `cyber` and `frontier_llm` declines on Sonnet 5. It doesn't retry the other three. You can also use the SDK middleware or your own retry.

被拒答的请求返回 HTTP 200，`stop_reason` 为 `"refusal"`，`stop_details` 会指明五类之一：`cyber`、`bio`、`frontier_llm`、`reasoning_extraction` 或 `general_harms`。服务端回退（server-side fallback，`fallbacks: "default"`，beta，Claude API）会在 Sonnet 5 上重试 `cyber` 和 `frontier_llm` 两类拒答，其余三类不重试。你也可以使用 SDK 中间件或自己的重试逻辑。

For legitimate security work, the Cyber Verification Program will soon expand to include Sonnet 5.5.

对于正当的安全工作，网络验证计划（Cyber Verification Program）很快将扩展到涵盖 Sonnet 5.5。

## 可用性（Availability）

Claude Sonnet 5.5 is available today on the platforms below. On the developer platforms, use these model IDs:

Claude Sonnet 5.5 即日起在以下平台可用。在开发者平台上，使用这些模型 ID：

- Claude API, as `claude-sonnet-5-5`
- Amazon Bedrock, as `anthropic.claude-sonnet-5-5`
- Claude Platform on AWS, as `claude-sonnet-5-5`
- Google Cloud, as `claude-sonnet-5-5`
- Microsoft Foundry, as `claude-sonnet-5-5`, on Global Standard deployments only

- Claude API，模型 ID 为 `claude-sonnet-5-5`
- Amazon Bedrock，模型 ID 为 `anthropic.claude-sonnet-5-5`
- AWS 上的 Claude Platform，模型 ID 为 `claude-sonnet-5-5`
- Google Cloud，模型 ID 为 `claude-sonnet-5-5`
- Microsoft Foundry，模型 ID 为 `claude-sonnet-5-5`（仅限 Global Standard 部署）

### 在 Claude Code 中（In Claude Code）

From Claude Code v2.1.284 (Agent SDK for TypeScript v0.3.284 or later), the `sonnet` alias resolves to Sonnet 5.5 on the Claude API. It runs at `medium` effort by default, with the 1M context window native. You can't turn thinking off for Sonnet 5.5 in Claude Code, and effort sets how much the model thinks. Sonnet 5.5 has no fast mode. The `default` model stays Opus 5.5, so switch with `/model sonnet` for well-scoped tasks.

从 Claude Code v2.1.284（TypeScript 版 Agent SDK v0.3.284 或更高）开始，`sonnet` 别名在 Claude API 上解析为 Sonnet 5.5。它默认以 `medium` effort 运行，1M 上下文窗口（context window）为原生支持。在 Claude Code 中，你无法为 Sonnet 5.5 关闭思考，effort 决定模型的思考量。Sonnet 5.5 没有快速模式（fast mode）。`default` 模型仍是 Opus 5.5，边界清晰的任务可用 `/model sonnet` 切换。

We hope you'll enjoy trying out Sonnet 5.5 and as always feel free to share feedback.

希望你体验 Sonnet 5.5 愉快，也一如既往欢迎随时分享反馈。

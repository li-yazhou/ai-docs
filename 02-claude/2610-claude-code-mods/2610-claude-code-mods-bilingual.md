# 用 TypeScript mods 自定义 Claude Code（中英对照）

> **原文标题：** Customize Claude Code with mods in TypeScript
> **原文链接：** https://claude.com/blog/claude-code-mods
> **原文作者：** Anthropic（原文未署名）
> **发布日期：** 2026-10-01
> **翻译模型：** GLM-5.3-Flash
> **评分：** ★★★★☆ —— Claude Code 继 skills、hooks 之后的又一扩展机制：mods 是小型 TypeScript 函数，可改写 prompt、添加 UI、替换内置功能，是 Claude Code 可定制性的重要演进
> 排版：每段英文原文在前，中文翻译紧随其后。默认收录正文主体。术语保留英文原文并附中文释义。

---

Today we're introducing mods, small TypeScript functions that change how Claude Code works. A mod can rewrite a prompt, add new UI, replace a built-in feature, or add entirely new functionality. You can write a mod yourself, or ask Claude Code to write one for you. Mods ship inside plugins, so you install and share them like any plugin. They work in the Claude Code CLI and desktop app.

今天我们推出 mods——一些小型 TypeScript 函数，能够改变 Claude Code 的工作方式。一个 mod 可以改写 prompt、添加新 UI、替换内置功能，或加入全新的功能。你可以自己编写 mod，也可以让 Claude Code 替你写一个。Mods 随 plugins（插件）一起发布，因此你可以像安装任何插件一样安装和分享它们。它们在 Claude Code CLI 与桌面应用中均可使用。

Mods run with the same access to your machine as Claude Code itself. They aren't sandboxed, and you should only install mods from sources you trust, the same way you'd install any code on your computer.

Mods 运行时对你机器的访问权限与 Claude Code 本身相同。它们没有沙箱隔离（sandboxed），你应当只从自己信任的来源安装 mods——就像你在电脑上安装任何代码时应当做的那样。

To see what mods can do, read our guide to building your first mod.

想看看 mods 能做什么，请阅读我们关于构建你的第一个 mod 的指南。

# 为什么我们要构建 mods（Why we built mods）

Developers have asked for more control over how Claude Code works, without waiting for us to ship a feature. Hooks helped give users some of this control, but hooks can't rewrite events, draw new UI, or replace features. Mods can.

开发者一直希望能更好地掌控 Claude Code 的工作方式，而不必等我们发布某个功能。Hooks（钩子）为用户提供了一部分这样的控制，但 hooks 无法改写事件（event）、绘制新 UI，也无法替换功能。Mods 可以。

We want Claude Code to feel like yours, so you can shape it to the way you work. We shared the design for mods on GitHub before launch to get feedback from developers. Thank you to everyone who weighed in.

我们希望 Claude Code 让你觉得"这是我的工具"，让你能把它塑造成符合自己工作方式的形态。在发布之前，我们在 GitHub 上公开了 mods 的设计，征求开发者的反馈。感谢每一位提出意见的人。

# mods 的工作原理（How mods work）

Each time Claude Code does something, it emits an event. Examples include calling a tool, asking for permission, and drawing part of the screen. A mod is a function that hooks into one of these events. A mod can run before the event, after it, or instead of it. It can also wrap the event, running code both before and after. With one function, a mod can:

Claude Code 每做一件事，都会发出一个事件（event）。例如：调用某个工具、请求权限、绘制屏幕的一部分。Mod 就是挂接到其中某个事件上的函数。Mod 可以在事件之前运行、之后运行，或者取而代之；它还可以包裹（wrap）事件，在事件前后都运行代码。只需一个函数，mod 就能够：

- Rewrite a prompt before it reaches the model.
- Block, rewrite, or retry a tool call.
- Approve or deny a permission request.
- Redact secrets from tool output before Claude reads it.

- 在 prompt 到达模型之前改写它。
- 阻止、改写或重试一次工具调用（tool call）。
- 批准或拒绝一个权限请求。
- 在 Claude 读取工具输出之前，先将其中的机密信息脱敏（redact）。

A mod can also change what you see. It can edit or replace part of the interface Claude Code draws, like a tool result or a question from Claude. It can add buttons and inputs, and other mods can respond when you press them. Today, a mod can target the terminal, the desktop app, or both.

Mod 还能改变你所看到的内容。它可以编辑或替换 Claude Code 所绘制界面的某一部分，比如某个工具结果，或 Claude 的提问。它可以添加按钮和输入控件，而其他 mods 可以在你按下它们时作出响应。目前，一个 mod 可以面向终端（terminal）、桌面应用，或两者兼有。

When several mods hook the same event, they run in the order they load. The first mod to load sees the event first and the result last. This lets you stack mods from different authors.

当多个 mods 挂接同一个事件时，它们按加载顺序依次运行：最先加载的 mod 最先看到事件、最后看到结果。这让你可以把不同作者的 mods 叠加（stack）起来使用。

You can also use Claude Code to mod Claude Code. Ask Claude to create a mod, and it can write the TypeScript, install it, and hot reload it in your session.

你还可以用 Claude Code 来 mod Claude Code 本身。让 Claude 创建一个 mod，它就能写好 TypeScript、完成安装，并在你的会话中热重载（hot reload）。

# 用自己的实现替换内置功能（Swap built-in features for your own）

Some built-in features of Claude Code now ship as mods. For example, the built-in `/diff` feature is now a mod, so you can turn it off (in `/plugin`) or replace it with your own version. We plan to move more built-in features to mods over time, so you can pare Claude Code down to a small core and add back only what you want.

Claude Code 的一些内置功能现在以 mods 的形式发布。例如，内置的 `/diff` 功能现在就是一个 mod，因此你可以在 `/plugin` 中把它关掉，或者换成你自己的版本。我们计划逐步把更多内置功能迁移为 mods，让你可以把 Claude Code 精简成一个很小的核心，再按需加回你想要的部分。

# 面向团队与企业的 mods（Mods for teams and enterprises）

Mods ship inside plugins, so your existing plugin controls apply. Admins can allow or block plugin marketplaces. On Team and Enterprise plans, an owner sets this in the admin console. On Claude API and third-party API plans, admins push managed settings to users' machines.

Mods 打包在 plugins 之中发布，因此你现有的插件管控手段直接适用。管理员可以允许或封锁插件市场（plugin marketplace）。在 Team 与 Enterprise 方案中，由 owner（所有者）在管理控制台中设置；在 Claude API 与第三方 API 方案中，管理员通过向用户机器推送受管设置（managed settings）来落实。

On Team and Enterprise plans, and on any machine with managed settings, a built-in mod called `sec-default` ("security default") loads first. It stops mods that users install from doing risky things, like overriding your permission deny rules. You can view the source code to see what it restricts. Admins can load their own mods first instead. If you do, add `sec-default` to your list to keep its restrictions.

在 Team 与 Enterprise 方案中，以及任何配置了受管设置的机器上，一个名为 `sec-default`（"security default"，安全默认）的内置 mod 会最先加载。它会阻止用户安装的 mods 做有风险的事情，比如覆盖你的权限拒绝规则（permission deny rules）。你可以查看它的源码，了解它限制了什么。管理员也可以改为最先加载自己的 mods——如果是这样，请把 `sec-default` 加进你的列表，以保留它的这些限制。

Teams can also use mods to build their own controls and functionality. For example:

团队也可以用 mods 来构建自己的管控与功能。例如：

- CI/CD status: a mod can show your pipeline's status in a pane beside the conversation, and update it as builds pass or fail.
- Production safeguards: a mod can require confirmation before any command touches production config.
- Audit logging: a mod that loads first can record every call that every other mod makes.

- CI/CD 状态：一个 mod 可以在对话旁边的面板中显示你的流水线（pipeline）状态，并随着构建通过或失败实时更新。
- 生产环境防护：一个 mod 可以要求在任何命令触碰生产配置（production config）之前先进行确认。
- 审计日志：一个最先加载的 mod 可以记录其他所有 mod 发出的每一次调用。

# 开始上手（Getting started）

Mods are available today in the Claude Code CLI and desktop app. Install plugins that include mods from the Claude directory or by running `/plugin` in the CLI. To share a mod, package it in a plugin and submit it to the directory.

Mods 即日起在 Claude Code CLI 与桌面应用中可用。你可以从 Claude directory（插件目录）安装包含 mods 的插件，也可以在 CLI 中运行 `/plugin`。要分享一个 mod，请把它打包进一个插件并提交到 directory。

To build your own mods, read our getting started guide or see the documentation.

要构建你自己的 mods，请阅读我们的入门指南，或查阅文档。

# Ask ChatGPT Web, Then Execute

**手脑协同：让 ChatGPT 网页参与思考，让 Codex 把工作做完。**

Consult ChatGPT Web, evaluate its advice, then execute and verify the work with Codex.

[English](#english) · [安装](#安装) · [使用](#使用) · [Examples](examples/workflows.md)

你可能遇到过这样的断点：网页版 ChatGPT 给出了一套方案，但接下来还要自己复制到编辑器、修改文件、运行程序，再把报错贴回网页。这个 skill 把这段往返交给 Codex：打开已登录的 ChatGPT 网页，带着明确问题咨询，读取完整回答，再回到本地执行和检查。

## 它能带来什么

| 优势 | 实际作用 |
|---|---|
| 分析与执行衔接 | 网页建议可以继续进入文件修改、程序运行和结果交付，减少手动搬运上下文。 |
| 多一个审查视角 | 可以请网页版检查方案、指出假设或提出替代思路，再由 Codex 对照实际文件和结果判断。 |
| 用执行结果检验建议 | Codex 可以运行测试、读取日志、检查产物，用本地证据判断建议是否适用。 |
| 便于迭代 | 遇到新问题时，可整理必要的错误信息进行下一轮有范围的咨询，再继续执行。 |
| 发挥各自工具优势 | 网页负责其当前界面支持的分析与讨论；Codex 负责当前环境允许的文件、命令和验证工作。 |
| 使用现有网页登录 | 通过已登录的 ChatGPT 网页咨询，这条路径本身不要求配置 OpenAI API key。账号限制仍然适用。 |
| 任务来源清楚 | 交付时说明网页提出了什么、Codex 采纳了什么、实际做了什么以及验证了什么。 |
| 轻量安装 | 项目是一个 Agent Skill，核心是 Markdown 指令，无需运行仓库服务或另装浏览器驱动。宿主需提供兼容的浏览器工具。 |

两个回答相同不等于结论正确。这个流程的价值在于增加审查和执行验证的机会；它不保证准确率、速度或成本改善。

## Codex 在这里擅长什么

Codex 的优势是把建议变成可检查的结果：先读取工作区，结合现有代码、数据和约束选择实现方式，再修改文件、执行命令并检查输出。它还能在失败后利用日志定位问题，继续完成修复，最后交付文件和验证结果。

这使它适合承担协作中的执行部分。网页版的建议仍需判断，能否运行某个软件或生成某种产物，取决于当前环境、权限和已安装工具。

## 安装

将本仓库放到 Codex 的技能目录，文件夹名保持为 `ask-chatgpt-web-then-execute`：

```bash
git clone https://github.com/xggqqwd-Lucas/ask-chatgpt-web-then-execute.git ~/.codex/skills/ask-chatgpt-web-then-execute
```

Windows PowerShell：

```powershell
git clone https://github.com/xggqqwd-Lucas/ask-chatgpt-web-then-execute.git "$env:USERPROFILE/.codex/skills/ask-chatgpt-web-then-execute"
```

已有同名技能时，先保留本地修改，再更新文件。重新开启聊天或按宿主要求刷新技能列表。

## 使用

```text
使用 $ask-chatgpt-web-then-execute，先请网页版 ChatGPT 审查这个实现方案，
再结合当前项目完成修改，运行相关检查并交付结果。
```

```text
使用手脑协同：请网页版分析这个报错可能的原因，
由 Codex 检查实际日志和代码、修复问题并验证。
```

```text
先用手脑协同讨论这个仿真的边界条件和验证方法，
再在本地建立一个小规模预览，说明采用了哪些假设。
```

以上均为自行编写的使用示例，不来自私人聊天。更多英文场景见 [examples/workflows.md](examples/workflows.md)。

## 工作流程

1. Codex 整理问题、必要背景和预期输出。
2. 在内置浏览器的 ChatGPT 网页中咨询。
3. 等待完整回答，保留关键假设和不确定性。
4. Codex 独立判断建议，结合实际工作区执行。
5. 检查结果，交付产物并说明证据边界。

## 环境与兼容性

- 需要能够加载 `SKILL.md` 的 Codex 环境，以及当前会话可用的 `mcp__cua_repl.js` 和内置浏览器。
- 需要能够访问 ChatGPT 网页；登录或验证由用户完成。
- 更新后的入口使用 `cua` API，并读取工具当次返回的接口文档。不会导入缓存目录下的旧浏览器脚本。
- 仅支持当前提供这些能力的环境，不宣称所有 Codex CLI、IDE、浏览器或版本均兼容。
- 已在维护者环境验证内置 ChatGPT 网页打开、读取及聊天搜索；尚未完成修订版本的完整问答到本地执行测试。

## 数据与使用范围

咨询会将选定内容发送至 ChatGPT 网页。只提供任务需要的上下文，涉及私人文件或敏感内容时遵循用户授权和宿主规则。登录凭据、Cookie 和会话令牌不由此技能提取。项目不包含私人聊天、实验数据或账户配置。

## English

### Why use it?

ChatGPT Web can help discuss a plan, review assumptions, or suggest a debugging approach. Codex can inspect a workspace, edit files, run permitted tools, and verify the output. This skill connects those steps into one workflow.

| Benefit | What it enables |
|---|---|
| Continuity from advice to action | Carry a web consultation into local implementation without manually relaying every step. |
| An additional review perspective | Request critique or alternative approaches before evaluating them against the actual project. |
| Evidence from execution | Check recommendations using tests, logs, file inspection, and output validation. |
| Focused iteration | Bring a new, bounded question back to the web consultation when new evidence warrants it. |
| Complementary tool access | Use the capabilities available in the web session for discussion and the local environment for execution. |
| Existing web sign-in | Consult the signed-in website without configuring an OpenAI API key for this route. Account limits still apply. |
| Clear attribution | Report what the web consultation suggested, what Codex adopted, and what was executed and verified. |
| Small footprint | Install a Markdown-based Agent Skill; no repository service or separate browser driver is required. Compatible host tools are required. |

Agreement between assistants does not establish correctness. The workflow provides opportunities for critique and verification; it does not promise measured gains in accuracy, speed, or cost.

### What Codex contributes

Codex turns proposals into inspectable work: it reads the workspace, selects an implementation that fits existing constraints, edits files, runs permitted commands, and checks the resulting artifacts. Logs and test results can inform the next correction. Available software, tools, and permissions determine what it can actually execute.

### Install and use

Clone this repository into `~/.codex/skills/ask-chatgpt-web-then-execute`, then refresh skill discovery as required by your host. Preserve local edits before replacing an existing installation.

```text
Use $ask-chatgpt-web-then-execute to ask ChatGPT Web to review the proposed fix.
Evaluate its advice against this repository, implement the fix, and run the relevant checks.
```

See [examples/workflows.md](examples/workflows.md) for debugging, research planning, and simulation examples.

### Requirements and validation

Requires a Codex host that exposes `mcp__cua_repl.js`, its `cua` API, and an in-app browser, plus access to ChatGPT Web. The skill follows live browser documentation rather than importing a cached browser runtime. Sign-in and account verification remain with the user. Availability is host-dependent; universal CLI, IDE, browser, or version compatibility is not claimed.

The current revision has been checked for skill structure and browser navigation, page reading, and chat search in the maintainer environment. A complete consultation-to-local-execution smoke test has not yet been run for this revision.

### Data handling

Selected consultation content is transmitted to ChatGPT Web. Share only necessary context and follow user authorization and host rules for sensitive material or file uploads. This skill does not extract credentials, cookies, or session tokens. No private conversations or experimental datasets are included.

## Files

| File | Purpose |
|---|---|
| [SKILL.md](SKILL.md) | Agent instructions and consultation workflow |
| [agents/openai.yaml](agents/openai.yaml) | Skill display metadata |
| [examples/workflows.md](examples/workflows.md) | Original usage examples |
| [llms.txt](llms.txt) | Documentation index for language models |

## Attribution

This project independently implements the high-level consult-then-execute pattern associated with [Wangnov/chatgpt-skill](https://github.com/Wangnov/chatgpt-skill). It does not bundle or execute that project. See the provenance section in [SKILL.md](SKILL.md).

Search terms: Codex skill, ChatGPT Web, Agent Skills, browser consultation, AI workflow, local execution, debugging, simulation planning, 手脑协同, 网页分析与本地执行.

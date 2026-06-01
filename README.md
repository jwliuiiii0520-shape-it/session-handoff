# session-handoff

一个轻量的 Codex skill：在上下文变长之前，总结当前工作；开启新的
session 后，先对齐理解，再继续执行。

## 为什么需要它

长时间和 Codex 协作时，经常会遇到一个两难：

- 继续留在旧 session：上下文越来越长，Agent 容易忽略早期约束，判断质量下降。
- 直接开启新 session：上下文恢复清零，Agent 不知道已经做了什么，也容易重复走弯路。

`session-handoff` 在两个 session 之间增加一份简洁、可审查的交接文档。
它不会复制完整聊天记录，也不依赖数据库或外部服务。所有关键信息都保存在
当前项目内的一份 Markdown 文件中。

## 30 秒上手

旧 session 中：

```text
$session-handoff sho-summary
```

Codex 会检查当前项目状态，并生成一份新的交接文档：

```text
.codex/handoff/2026-06-01-1530-session-handoff-skill-design.md
```

新 session 中：

```text
$session-handoff sho-read
```

Codex 会读取最新交接文档、核实当前文件状态，然后：

1. 简要说明它对已完成工作的理解。
2. 列出建议的下一步行动。
3. 提醒你仍然存在的风险或歧义。
4. 等待你的确认。

只有在你明确同意后，它才会继续修改文件或执行任务。

## 它会总结什么

交接文档包含：

- 本次 session 讨论和推进了什么
- 用户最终希望完成什么
- 已经完成的工作和当前状态
- 按优先级排列的下一步行动
- 已经确认的重要决策和限制条件
- 相关文件路径
- 已运行和未运行的验证
- 尚未解决的阻塞问题

一个简化后的示例：

```markdown
# Session 交接

更新时间：2026-06-01 15:30
项目根目录：/path/to/project
Session 标题：fix-login-redirect

## 本次 Session 摘要
- 定位了登录回调后重复跳转的问题。
- 修复了路由守卫对过期 session 的处理逻辑。
- 已补充回归测试，但尚未执行完整测试套件。

## 当前状态
- 登录回调逻辑已修改。
- 定向测试通过。
- 完整测试套件尚未运行。

## 下一步行动
1. 运行完整测试套件。
2. 检查未登录用户访问受限页面时的跳转行为。

## 已确认的决策
- 保持现有路由结构，不引入新的状态管理依赖。

## 阻塞问题
无。
```

## 两个操作

### `sho-summary`

总结当前 session，并创建一份新的交接文档。

- Git 项目：保存到 Git 仓库根目录下的 `.codex/handoff/`
- 非 Git 项目：保存到当前工作目录下的 `.codex/handoff/`
- 每次生成新文件，不覆盖历史记录

### `sho-read`

读取用户指定的交接文档，或者读取文件名时间最新的一份。

`sho-read` 的目标是恢复上下文并和用户对齐，而不是直接开工。它会先核实
文件系统和 Git 状态，再等待用户明确授权。

## 安装

### 方式一：手动安装

创建 skill 目录：

```bash
mkdir -p ~/.codex/skills/session-handoff
```

将仓库中的 [`SKILL.md`](./SKILL.md) 保存到：

```text
~/.codex/skills/session-handoff/SKILL.md
```

### 方式二：使用 Git

```bash
git clone https://github.com/jwliuiiii0520-shape-it/session-handoff.git
mkdir -p ~/.codex/skills/session-handoff
cp session-handoff/SKILL.md ~/.codex/skills/session-handoff/SKILL.md
```

## 使用示例

生成交接文档：

```text
$session-handoff sho-summary
```

恢复上下文：

```text
$session-handoff sho-read
```

也可以使用自然语言：

```text
sho-summary
总结当前 session，准备交接

sho-read
读取交接文件，汇报理解并等待确认
```

## 设计原则

- **短而有用**：目标是保留下一个 session 正确行动所需的信息，而不是导出聊天记录。
- **项目内保存**：交接文档跟随项目，不依赖固定电脑路径或第三方服务。
- **保留历史**：每次创建新的交接文件，便于回溯。
- **先对齐再执行**：新 session 不会读取文档后自动开工。
- **以实际状态为准**：交接文档是工作摘要，不是绝对事实。恢复时仍会检查文件和 Git 状态。

## 适合什么场景

- 一个开发任务需要跨多个 Codex session 完成
- 对话已经很长，希望主动压缩上下文
- 调研、写作或文档任务需要阶段性交接
- 希望保留一份可读、可审查的项目进展记录

## 不是什么

`session-handoff` 不是长期记忆数据库，也不是自动检索所有历史对话的 RAG
系统。它刻意保持简单：一次交接，一份 Markdown 文件，一个清晰的确认点。

## License

[MIT](./LICENSE) © 2026 pika wanwan

---

## English

`session-handoff` is a lightweight Codex skill for carrying work across fresh
sessions without dragging the full conversation history along.

Long sessions gradually accumulate noise. Fresh sessions start clean, but lack
the decisions, progress, and constraints that matter. This skill creates a
concise, project-local Markdown handoff before you switch sessions.

### Quick Start

In the old session:

```text
$session-handoff sho-summary
```

In the new session:

```text
$session-handoff sho-read
```

The new session reads the latest handoff, verifies the actual project state,
explains its understanding, lists next steps and risks, and waits for your
confirmation before taking action.

### What Gets Preserved

- Session summary
- End goal
- Completed and remaining work
- Confirmed decisions and constraints
- Relevant files
- Verification results
- Blockers
- Concrete next actions

### Installation

```bash
git clone https://github.com/jwliuiiii0520-shape-it/session-handoff.git
mkdir -p ~/.codex/skills/session-handoff
cp session-handoff/SKILL.md ~/.codex/skills/session-handoff/SKILL.md
```

### Handoff Files

Handoff files are stored inside the current project:

```text
.codex/handoff/YYYY-MM-DD-HHmm-session-title.md
```

Git repositories use the repository root. Non-Git projects use the current
working directory.

### Design Principles

- Preserve actionable context, not chat transcripts.
- Store Markdown files locally inside the project.
- Keep historical handoffs instead of overwriting them.
- Verify actual files before trusting a handoff.
- Align with the user before resuming execution.

### License

[MIT](./LICENSE) © 2026 pika wanwan

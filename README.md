# session-handoff

一个轻量的 Codex skill，用于在新旧 session 之间交接上下文。

当对话逐渐变长时，可以先生成一份简洁的交接文档，再开启新的
session。新 session 会先读取文档、说明对已有工作的理解，并在获得
确认后继续执行。

## 功能

- `sho-summary`：总结当前 session，生成一份新的交接文档。
- `sho-read`：读取最新或指定的交接文档，核实实际状态，并等待用户确认。
- 每次总结都会保留历史文件，不覆盖旧记录。
- 交接文档保存在当前项目内，不依赖固定的本地路径。

## 安装

将仓库中的 `SKILL.md` 保存到：

```text
~/.codex/skills/session-handoff/SKILL.md
```

## 使用

生成交接文档：

```text
$session-handoff sho-summary
```

在新的 session 中恢复上下文：

```text
$session-handoff sho-read
```

也可以直接使用自然语言：

```text
sho-summary
sho-read
```

## 文件位置

如果当前目录属于 Git 仓库，交接文档会保存在仓库根目录：

```text
.codex/handoff/YYYY-MM-DD-HHmm-session-title.md
```

如果当前目录不属于 Git 仓库，则保存在当前工作目录下。

例如：

```text
.codex/handoff/2026-06-01-1530-session-handoff-skill-design.md
```

## 工作流程

```text
旧 session
  -> 执行 sho-summary
  -> 生成简洁的交接文档
  -> 开启新 session
  -> 执行 sho-read
  -> 新 session 汇报理解、下一步和风险
  -> 用户确认后继续执行
```

## License

[MIT](./LICENSE) © 2026 pika wanwan

---

## English

A lightweight Codex skill for handing off context between sessions.

When a conversation becomes too long, generate a concise handoff document and
start a fresh session. The new session reads the document, explains its
understanding of the existing work, and waits for confirmation before taking
further action.

### Features

- `sho-summary`: Summarize the current session and create a new handoff file.
- `sho-read`: Read the latest or a specified handoff file, verify the actual
  state, and wait for user confirmation.
- Keep historical handoff files instead of overwriting previous records.
- Save files inside the current project without relying on a fixed local path.

### Installation

Save `SKILL.md` to:

```text
~/.codex/skills/session-handoff/SKILL.md
```

### Usage

Create a handoff:

```text
$session-handoff sho-summary
```

Resume in a new session:

```text
$session-handoff sho-read
```

Handoff files are stored in:

```text
.codex/handoff/YYYY-MM-DD-HHmm-session-title.md
```

### License

[MIT](./LICENSE) © 2026 pika wanwan

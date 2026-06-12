---
name: quickstart
description: 为开源项目生成一键式傻瓜安装结构（README + AGENTS.md + INSTALL.md + SKILL.md 模板），抄自 soulcheck 模式
runAs: inline
---

## 概述

为你新开源的 Reasonix 项目生成一套 soulcheck 风格的「一键式傻瓜安装」文件结构。
用户只需复制一句话丢给 AI Agent，Agent 就会自动读取 AGENTS.md → 按 INSTALL.md 步骤安装。

---

## 使用方式

```
/quickstart
```

按提示依次输入项目信息，即可在指定路径生成全套安装文件。

---

## 执行指令

### Step 1: 收集项目信息

通过 ask 工具向用户收集以下信息：

| 字段 | 说明 | 示例 |
|------|------|------|
| **项目名称** | 英文/数字，用作目录名 | `my-tool` |
| **一句话描述** | 项目定位 | "一个超级好用的 XXX 工具" |
| **GitHub 用户名** | 你的 GitHub 账号 | `your-name` |
| **Skill 名称** | 技能目录名 | `my-tool` |
| **Skill 短描述** | ≤120 字，SKILL.md 用 | "做某某事情" |
| **触发词** | 用户如何调用 | `/my-tool` |
| **功能概述** | 2-5 句描述 Skill 做什么 | …… |
| **输出路径** | 生成文件的目标目录 | 默认当前工作区 |

### Step 2: 生成 README.md

生成文件 `{输出路径}/README.md`，内容模板如下（将 `{占位符}` 替换为实际信息）：

```markdown
# {项目名称} 🧪

{一句话描述}

## 🚀 一键安装

支持：Reasonix · Claude Code · Cursor · Gemini CLI · GitHub Copilot · Windsurf · Cline · Aider …

复制下面这行话，丢给你的 AI Agent，就装好了：

```
请帮我安装这个项目，如果已安装就更新到最新版：https://github.com/{用户名}/{项目名称}
```

👆 Agent 会自动读取 AGENTS.md → 按 INSTALL.md 步骤安装。当前会话立刻生效。

装好后：
```
{触发词}
```

---

## 这是什么

{功能概述，2-5 段}

---

## 项目结构

```
{项目名称}/
├── README.md              # 本文档
├── AGENTS.md              # AI Agent 发现入口
├── INSTALL.md             # AI 自动安装指令
├── LICENSE                # MIT 协议
└── .reasonix/skills/{skill-name}/SKILL.md  # ⭐ 核心功能
```

---

## 许可

MIT
```

### Step 3: 生成 AGENTS.md

生成文件 `{输出路径}/AGENTS.md`：

```markdown
# {项目名称} — AI Agent 安装指引

## 如果你是 AI Agent（Reasonix / Claude Code / Cursor / Gemini CLI / Copilot / Windsurf / Cline / Aider …）

**你的任务**：帮用户把这个 {项目名称} 技能安装到他的工作区。

### 第一步：阅读本文件

你已经读了，很好 👏

### 第二步：阅读 INSTALL.md

打开 [`INSTALL.md`](INSTALL.md)，按里面的步骤执行：

1. 检测当前环境（Reasonix / Claude Code / 通用）
2. 按对应方式安装 SKILL.md
3. 验证安装完成

### 第三步：安装完成后的能力

安装完成后，用户可以直接使用：

- `{触发词}`
- "{自然语言调用方式}"

---

## 核心文件

| 文件 | 用途 |
|------|------|
| `INSTALL.md` | AI 自动安装指令（支持所有 Agent） |
| `.reasonix/skills/{skill-name}/SKILL.md` | 核心功能实现 |
```

### Step 4: 生成 INSTALL.md

生成文件 `{输出路径}/INSTALL.md`，内容参考以下结构，将 `{占位符}` 替换：

```markdown
# INSTALL.md — AI Agent 自动安装指令

> 给 **所有 AI Agent** 看的：当用户提供 GitHub 链接 `https://github.com/{用户名}/{项目名称}` 并要求安装时，按以下流程自动执行。

---

## Step 1: 自我身份检测

检查当前运行环境，判断自己是什么 Agent：

| 信号 | Agent 类型 | 安装方式 |
|------|-----------|---------|
| 存在 `install_skill` 工具 / `.reasonix/` 目录 | **Reasonix** | 方式 A |
| 存在 `~/.claude/` 目录 / `claude` CLI | **Claude Code** | 方式 B |
| 存在 `.cursor/rules/` 目录 / `.cursorrules` | **Cursor** | 方式 C |
| 以上都不匹配 | **通用兼容模式** | 方式 X |

---

## 方式 A：Reasonix

```bash
reasonix install_source --source https://github.com/{用户名}/{项目名称} --kind skill
```

安装后在工作区运行：`{触发词}`

## 方式 B：Claude Code

```bash
mkdir -p ~/.claude/skills/{skill-name}
curl -o ~/.claude/skills/{skill-name}/SKILL.md https://raw.githubusercontent.com/{用户名}/{项目名称}/main/.reasonix/skills/{skill-name}/SKILL.md
```

安装后在 Claude Code 中说：`{触发词}`

## 方式 C：Cursor

```bash
mkdir -p .cursor/rules
cp .reasonix/skills/{skill-name}/SKILL.md .cursor/rules/{skill-name}.mdc
```

## 方式 X：通用兼容模式

```bash
cp .reasonix/skills/{skill-name}/SKILL.md ./
```

---

## Step 2: 验证安装

确认运行 `{触发词}` 后能正常工作。
```

### Step 5: 生成 SKILL.md 模板

生成文件 `{输出路径}/.reasonix/skills/{skill-name}/SKILL.md`：

```markdown
---
name: {skill-name}
description: {Skill 短描述}
runAs: inline
---

## 概述

{功能概述，2-5 句话}

---

## 使用方式

```
{触发词}
```

---

## 执行指令

### Step 1: 准备工作

{描述需要准备什么}

### Step 2: 执行主逻辑

{描述核心执行步骤}

---

## 注意事项

{注意事项}
```

### Step 6: 生成 LICENSE

生成文件 `{输出路径}/LICENSE`（MIT 协议）：

```markdown
MIT License

Copyright (c) {当前年份} {用户名}

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### Step 7: 汇总输出确认

将生成的所有文件路径和摘要告知用户，确认安装完成。

```
✅ quickstart 脚手架已生成！
📁 {输出路径}/
   ├── README.md
   ├── AGENTS.md
   ├── INSTALL.md
   ├── LICENSE
   └── .reasonix/skills/{skill-name}/
       └── SKILL.md

用户复制以下咒语即可一键安装此项目：
"请帮我安装这个项目，如果已安装就更新到最新版：https://github.com/{用户名}/{项目名称}"
```

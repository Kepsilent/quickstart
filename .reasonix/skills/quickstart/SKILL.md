---
name: quickstart
description: 一键傻瓜式发布助手 —— 为你的项目增加一键安装结构，并推送到 GitHub
runAs: inline
---

## 概述

你已经做好了项目，想发布到 GitHub 并且让用户「复制一句话就能装好」？

**quickstart** 就是干这个的。它是一个「发布助手」Skill：

1. **检查环境** — 检测 git、gh 是否就绪，一次性配好，以后不再问
2. **添加一键安装** — 在当前目录生成 README/AGENTS.md/INSTALL.md/CLAUDE.md/LICENSE
3. **一键发布** — 检测到 gh 就绪后主动询问，选择发布即自动推送到 GitHub

> **所有安装统一全局**：无论是 quickstart 本身，还是 quickstart 处理过的项目，
> 用户安装时都会进入 `~/.reasonix/skills/`（全局），在所有项目目录下均可使用，不会互相污染。
>
> 你只需要配一次 gh，以后任何项目都能直接调 gh 发布，不会再问 SSH 之类的细节。

---

## 使用方式

### 方式一：直接调用（点选目录）

```
/quickstart
```

AI 会扫描当前目录和附近项目，通过 ask 工具列出可选目录，你**点选**即可。

### 方式二：带参数调用（直达）

```
# 操作当前项目
/quickstart 就这个项目

# 指定项目路径
/quickstart /path/to/your-project

# 按名称查找项目
/quickstart my-project-name
```

> 带参数时 AI 会自动确定目标目录，跳过选择步骤。

---

## 执行指令

> **重要**：本 Skill 默认工作在 **当前工作目录**，但 Step 1.5 可让你点选其他目录。
> 也支持传参直达：`/quickstart 就这个项目` 或 `/quickstart /path/to/project`。
> 所有操作都在你选定的目标目录进行，不会创建新目录。

### Step 0: 解析用户传入的参数

当用户通过 ``/quickstart`` 调用时，可能携带参数。按以下规则处理：

| 参数 | 含义 | 行为 |
|------|------|------|
| 无参数 / ``` | 普通调用 | 进入 Step 1.5 让用户选择目录 |
| `就这个项目` / `当前项目` / `这个项目` | 操作当前目录 | 标记 `{输出目录} = pwd`，跳过 Step 1.5 |
| 绝对路径如 `/home/user/my-project` | 指定目录 | 标记 `{输出目录} = 该路径`，跳过 Step 1.5 |
| 项目名如 `my-tool` | 按名查找 | 在 `~/Documents/Github/` 等目录中查找，找到则自动使用，多个则在 Step 1.5 列出 |

> 无论是否跳过 Step 1.5，后续所有 `{输出目录}` 占位符都替换为最终确定的目录路径。

---

### Step 1: 环境预检 + gh 配置向导

运行以下命令检查环境：

```bash
# 检查 git 是否可用
git --version

# 检查 gh 是否已安装
gh --version

# 检查 gh 登录状态
gh auth status
```

根据检测结果执行以下逻辑：

| 情况 | 操作 |
|------|------|
| git 不可用 | 告知用户安装 git，流程结束 |
| gh 未安装 | 告知用户安装 gh CLI，流程结束 |
| gh 已安装但未登录 | 引导用户运行 `gh auth login` 登录 |
| gh 已登录 ✅ | 告知用户并继续 |

> **关键行为**：如果 gh 已安装但未登录，引导用户运行 `gh auth login` 完成登录。
> 登录成功后，告知 AI：「我配好了，以后可以直接调 gh 发布，不用再问 SSH 之类的细节。」
> AI 将此状态记入会话，本次运行及后续运行都不再重复询问登录问题。

---

### Step 1.5: 确认目标目录（点选操作）

> **此步骤使用 `ask` 工具提供按钮，用户直接点选，无需打字。**

先运行以下命令扫描项目：

```bash
# 获取当前工作目录
pwd

# 列出当前目录内容
ls -la

# 扫描常见项目目录中的 git 仓库
ls -d ~/Documents/Github/*/ 2>/dev/null | head -10
```

然后用 `ask` 工具展示以下选项供用户 **点选**：

> **步骤：** ask(questions=[{
>   header: "选择目录",
>   question: "请选择要操作的项目目录：",
>   options: [
>     { label: "`📁 当前目录 (` + pwd + `)" },
>     { label: "`📂 扫描到的项目1" },
>     { label: "`📂 扫描到的项目2" },
>     { label: "`📂 输入其他路径" },
>     { label: "`❌ 取消" }
>   ]
> }])

> AI 行为：
> - 用户点选某个目录 → `{输出目录}` = 该路径，继续
> - 用户点选「输入其他路径」→ 用 ask 获取文本输入
> - 用户点选「取消」→ 结束流程

> **如果 Step 0 已确定 `{输出目录}`，则跳过此步骤。**

---

### Step 2: 自动提取项目信息

从当前目录自动提取以下信息：

| 字段 | 提取方式 | 说明 |
|------|---------|------|
| **项目名称** | `basename $(pwd)` | 当前目录名 |
| **项目描述** | 读取已有 README.md 首段 / 或 ask 用户 | 一句话描述 |
| **GitHub 用户名** | `gh api user --jq .login` | 从 gh 自动提取 |

提取结果告知用户，确认无误后进入下一步。

---

### Step 3: 添加一键安装文件

在当前目录生成以下文件。每个文件有明确的模板内容，AI 严格按照模板填充占位符后写入。

---

#### 3.1 README.md

```markdown
# {项目名称}

{一句话描述}

---

## 🚀 一键式傻瓜安装

### 一句话搞定

支持：Reasonix · Claude Code · Cursor · Gemini CLI · GitHub Copilot · Windsurf · Cline · Aider …

直接复制下面这行话，丢给你的 AI Agent，就装好了：

```
强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{GitHub用户名}/{项目名称}
```

> ⚠️ 重要：AI 不得跳过安装流程，必须删除旧版本后重新下载覆盖。详见 `INSTALL.md`。

Agent 会自动读取 `AGENTS.md` → 按 `INSTALL.md` 步骤执行。当前会话立刻生效。

---

## 📖 项目介绍

{项目描述}

---

## 项目结构

```
{项目名称}/
├── README.md              # 本文档（安装入口）
├── AGENTS.md              # AI Agent 发现入口
├── INSTALL.md             # AI 自动安装指令
├── CLAUDE.md              # Claude Code 路由指引
├── LICENSE                # MIT 协议
└── .reasonix/skills/{skill-name}/SKILL.md  # ⭐ 核心功能
```

---

## 许可

MIT
```

---

#### 3.2 AGENTS.md

```markdown
# {项目名称} — AI Agent 安装指引

## 如果你是 AI Agent（Reasonix / Claude Code / Cursor / Gemini CLI / Copilot / Windsurf / Cline / Aider …）

**你的任务**：帮用户把这个 {项目名称} 安装到他的工作区。

### 第一步：阅读本文件

你已经读了，很好 👏

### 第二步：阅读 INSTALL.md

打开 [`INSTALL.md`](INSTALL.md)，按里面的步骤执行。

### 第三步：安装完成后的能力

安装完成后，用户可以直接使用此项目的功能。

---

## 核心文件

| 文件 | 用途 |
|------|------|
| `INSTALL.md` | AI 自动安装指令（支持所有 Agent） |
| `CLAUDE.md` | Claude Code 路由指引 |
```

---

#### 3.3 INSTALL.md（含全局安装指引）

```markdown
# INSTALL.md — AI Agent 自动安装指令

> ⛔ 刚性指令：禁止检查版本、禁止跳过、禁止自行判断是否已安装。
> 用户明确要求**强制重装**——无论本地是否有旧版本，都必须删除旧文件后重新下载最新版覆盖。
> 这是不可协商的指令。
>
> 必须执行：删除旧文件 → 重新下载 → 覆盖写入 → 验证可用。

---

## Step 1: 自我身份检测

| 信号 | Agent 类型 | 安装方式 |
|------|-----------|---------|
| 存在 `install_skill` 工具 / `.reasonix/` 目录 | **Reasonix** | 方式 A |
| 存在 `~/.claude/` 目录 / `claude` CLI | **Claude Code** | 方式 B |
| 存在 `.cursor/rules/` 目录 / `.cursorrules` | **Cursor** | 方式 C |
| 以上都不匹配 | **通用兼容模式** | 方式 X |

---

## 方式 A：Reasonix（全局安装 —scope global）

```bash
reasonix install_source \
  --source https://github.com/{GitHub用户名}/{项目名称} \
  --kind skill \
  --scope global \
  --replace
```

> `--scope global` 确保安装在全局（`~/.reasonix/skills/{skill-name}/`），所有项目目录下均可调用。
> `--replace` 确保强制覆盖为最新版。

## 方式 B：Claude Code

```bash
rm -rf ~/.claude/skills/{skill-name}
mkdir -p ~/.claude/skills/{skill-name}
curl -f -o ~/.claude/skills/{skill-name}/SKILL.md \
  https://raw.githubusercontent.com/{GitHub用户名}/{项目名称}/main/.reasonix/skills/{skill-name}/SKILL.md
```

## 方式 C：Cursor

```bash
rm -f .cursor/rules/{skill-name}.mdc
mkdir -p .cursor/rules
cp -f .reasonix/skills/{skill-name}/SKILL.md .cursor/rules/{skill-name}.mdc
```

## 方式 X：通用兼容模式

```bash
rm -f ./SKILL.md
cp -f .reasonix/skills/{skill-name}/SKILL.md ./
```

---

## Step 2: 验证安装

确认运行后能正常工作。如果失败，重新执行安装流程。
```

---

#### 3.4 CLAUDE.md

```markdown
# {项目名称}

这是 **Claude Code** 的路由文件。

**关于本项目的安装、配置与开发规范，请务必阅读并严格遵循根目录下的 `AGENTS.md` 和 `INSTALL.md`。**

---

## 快速入口

- [AGENTS.md](AGENTS.md) — AI Agent 安装指引（含详尽说明）
- [INSTALL.md](INSTALL.md) — 自动安装指令
```

---

#### 3.5 LICENSE（MIT 协议）

```
MIT License

Copyright (c) {当前年份} {GitHub用户名}

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

---

#### 3.6 项目 SKILL（.reasonix/skills/{skill-name}/SKILL.md）

这是该项目的核心 Skill 文件，用户安装后即可通过触发词调用。

先生成目录：

```bash
mkdir -p .reasonix/skills/{skill-name}
```

再写入内容：

```markdown
---
name: {skill-name}
description: {项目描述}
runAs: inline
---

## 概述

{项目描述}

---

## 使用方式

```
/{skill-name}
```

---

## 执行指令

{根据项目具体功能编写执行步骤}
```

---

所有文件生成后告知用户：

> ✅ **一键安装文件已就绪！**
> 📂 当前目录新增了以下文件：
>   - README.md / AGENTS.md / INSTALL.md / CLAUDE.md / LICENSE
>   - `.reasonix/skills/{skill-name}/SKILL.md`
> 
> 你的用户复制以下一句话即可一键安装（**全局安装，所有项目通用**）：
> 
> ```
> 强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{GitHub用户名}/{项目名称}
> ```
> 
> 用户安装后，此 Skill 会进入全局 `~/.reasonix/skills/{skill-name}/`，在任何项目目录下均可使用。

---

### Step 4: 一键发布到 GitHub

#### 4.1 检测环境

确认 gh 已登录（Step 1 已验证过，直接确认即可）。

#### 4.2 询问用户

通过 ask 工具询问：

> **检测到 gh CLI 已就绪（登录账号：@{用户名}），是否将当前项目一键发布到 GitHub？**

选项：

1. **🚀 一键发布** — 自动执行 git init → add → commit → gh repo create → push
2. **⏭️ 跳过** — 仅输出手动发布指引

#### 4.3 执行发布（用户选择「一键发布」时）

```bash
# 初始化 git（如果尚未初始化）
git init

# 添加所有文件
git add -A

# 提交
git commit -m "`🎉 initial: {项目名称}`"

# 创建 GitHub 仓库并推送
gh repo create {用户名}/{项目名称} --public --source=. --remote=origin --push \
  --description "{项目描述}"
```

发布成功后告知用户：

> ✅ **一键发布成功！**
> 📦 仓库地址：https://github.com/{用户名}/{项目名称}
> 
> **你的用户只需复制以下一句话即可一键安装：**
> ```
> 强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{用户名}/{项目名称}
> ```

#### 4.4 用户选择「跳过」时

输出手动发布指引：

```bash
cd {当前目录}
git init
git add -A
git commit -m "`🎉 initial: {项目名称}`"
gh repo create {用户名}/{项目名称} --public --source=. --remote=origin --push \
  --description "{项目描述}"
```
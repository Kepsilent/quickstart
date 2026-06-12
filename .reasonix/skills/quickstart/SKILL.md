---
name: quickstart
description: 从脚手架生成到一键发布 GitHub —— 为开源项目提供「一键安装结构 + 一键发布」一站式自动化流程
runAs: inline
---

## 概述

一站式完成两件事：

**① 生成一键安装结构**
为你的开源项目生成 soulcheck 风格的「一键式傻瓜安装」文件结构。
用户只需复制一句话丢给 AI Agent，Agent 就会自动读取 AGENTS.md → 按 INSTALL.md 步骤安装。

**② 一键发布到 GitHub**
检测到 gh CLI 已登录时，主动询问你是否需要将项目直接发布到 GitHub。
选择「一键发布」即可自动完成 git init → commit → gh repo create → push。

从生成到发布，全程自动化。

---

## 使用方式

```
/quickstart
```

按提示依次输入项目信息，AI 会：

1. 自动检测 gh 登录状态（如已登录则自动填入 GitHub 用户名）
2. 生成全套安装文件（README / AGENTS.md / INSTALL.md / CLAUDE.md / LICENSE / SKILL.md）
3. **主动询问是否一键发布到 GitHub** → 选择发布则自动完成仓库创建和推送

---

## 执行指令

### Step 1: 收集项目信息

通过 ask 工具向用户收集以下信息：

| 字段 | 说明 | 示例 |
|------|------|------|
| **项目名称** | 英文/数字，用作目录名 | `my-tool` |
| **一句话描述** | 项目定位 | "一个超级好用的 XXX 工具" |
| **GitHub 用户名** | 你的 GitHub 账号（**自动检测**，见下方说明） | `your-name` |
| **Skill 名称** | 技能目录名 | `my-tool` |
| **Skill 短描述** | ≤120 字，SKILL.md 用 | "做某某事情" |
| **触发词** | 用户如何调用 | `/my-tool` |
| **功能概述** | 2-5 句描述 Skill 做什么 | …… |
| **输出路径** | 生成文件的目标目录 | 默认当前工作区 |

> **AI 自动检测逻辑**：收集信息前先运行 `gh auth status` 检查 gh 登录状态。
> - ✅ 如果已登录 → 提取用户名自动填入 GitHub 用户名字段，并告知用户：**"检测到已登录 GitHub：@{用户名}"**
> - ❌ 如果未登录或 gh 未安装 → 提示用户手动输入 GitHub 用户名，然后继续。
| **Skill 短描述** | ≤120 字，SKILL.md 用 | "做某某事情" |
| **触发词** | 用户如何调用 | `/my-tool` |
| **功能概述** | 2-5 句描述 Skill 做什么 | …… |
| **输出路径** | 生成文件的目标目录 | 默认当前工作区 |

### Step 2: 生成 README.md

生成文件 `{输出路径}/README.md`，内容模板如下（将 `{占位符}` 替换为实际信息）：

```markdown
# {项目名称}

{一句话描述}

---

## 🚀 一键式傻瓜安装

### 一句话搞定

支持：Reasonix · Claude Code · Cursor · Gemini CLI · GitHub Copilot · Windsurf · Cline · Aider …

直接复制下面这行话，丢给你的 AI Agent，就装好了：

```
强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{用户名}/{项目名称}
```

> ⚠️ 重要：AI 不得跳过安装流程，必须删除旧版本后重新下载覆盖。详见 `INSTALL.md`。

Agent 会自动读取 `AGENTS.md` → 按 `INSTALL.md` 步骤执行。当前会话立刻生效。

---

## 🛠️ 傻瓜式使用

装好后，在聊天框里直接输入：

```
{触发词}
```

或者用自然语言说：

```
- "帮我 {自然语言调用方式}"
- "{触发词} 跑一下"
```

> 如果你已经装过但想升到最新版，重新复制顶部的那句话丢给 AI Agent 即可强制覆盖。

---

## 📖 项目介绍

{功能概述，2-5 段}

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
| `CLAUDE.md` | Claude Code 路由指引 |
| `.reasonix/skills/{skill-name}/SKILL.md` | 核心功能实现 |

---

### 如果你是 Reasonix 用户

此项目已预置 Reasonix Skill，安装后可通过以下方式触发：

- **直接输入**：`{触发词}`
- **自然语言**："{自然语言调用方式}"
- **手动运行**：`reasonix run_skill --name {skill-name}`

Skill 文件位于 `.reasonix/skills/{skill-name}/SKILL.md`，可随时查看或修改。
```

### Step 4（新增）: 生成 CLAUDE.md

生成文件 `{输出路径}/CLAUDE.md`：

```markdown
# {项目名称}

这是 **Claude Code** 的路由文件。

**关于本项目的安装、配置与开发规范，请务必阅读并严格遵循根目录下的 `AGENTS.md` 和 `INSTALL.md`。**

---

## 快速入口

- [AGENTS.md](AGENTS.md) — AI Agent 安装指引（含详尽说明）
- [INSTALL.md](INSTALL.md) — 自动安装指令（含防呆预检）
```

### Step 5: 生成 INSTALL.md

生成文件 `{输出路径}/INSTALL.md`，内容参考以下结构，将 `{占位符}` 替换：

```markdown
# INSTALL.md --- AI Agent 自动安装指令

> ⚠️ 刚性指令：禁止检查版本、禁止跳过、禁止自行判断是否已安装。
> 用户明确要求**强制重装**——无论本地是否有旧版本，都必须删除旧文件后重新下载最新版覆盖。
> 这是不可协商的指令，以下任何情况都不得跳过安装流程：
> - "已是最新版" ❌ 不允许
> - "已安装过" ❌ 不允许
> - "无更新可用" ❌ 不允许
> - 任何自行判断版本的行为 ❌ 不允许
>
> 必须执行：删除旧文件 → 重新下载 → 覆盖写入 → 验证可用。

---

## Step 0: 环境预检（防呆检查）

在执行任何安装命令前，必须先完成以下预检，**任何一项不通过即中断并提示用户**：

### 0.1 必要工具检查

检查以下工具是否可用，缺少任意一个即提示用户安装：

| 工具 | 检查命令 | 说明 |
|------|---------|------|
| `git` | `git --version` | 版本管理 |
| `curl` | `curl --version` | 文件下载（Mac/Linux） |
| `gh`（可选） | `gh --version` | GitHub CLI，用于创建仓库 |

> AI 不得自行安装缺失工具，必须提示用户手动安装后重试。

### 0.2 权限安全限制

> ⛔ **绝对禁止**：在未获用户明确书面授权的情况下，AI 不得使用 `sudo`、`runas`、管理员身份或任何提权方式执行命令。
>
> 遇到权限错误（如 `Permission denied`、`EACCES`），AI 必须：
> 1. 立即中断当前操作
> 2. 明确告知用户遇到了权限问题
> 3. 等待用户处理后再继续
>
> 不得自行重试、不得绕过、不得更换路径规避。

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

## 方式 A：Reasonix（带 --replace 强制覆盖）

```bash
reasonix install_source \
  --source https://github.com/{用户名}/{项目名称} \
  --kind skill \
  --replace
```

> `--replace` 确保即使已安装同名 skill，也强制覆盖为最新版。

安装后在工作区运行：`{触发词}`

## 方式 B：Claude Code（强制重下载覆盖）

```bash
# 第一步：删除旧文件（无论是否存在）
rm -rf ~/.claude/skills/{skill-name}

# 第二步：重新创建目录
mkdir -p ~/.claude/skills/{skill-name}

# 第三步：强制下载最新版覆盖
curl -f -o ~/.claude/skills/{skill-name}/SKILL.md \
  https://raw.githubusercontent.com/{用户名}/{项目名称}/main/.reasonix/skills/{skill-name}/SKILL.md
```

安装后在 Claude Code 中说：`{触发词}`

## 方式 C：Cursor（强制覆盖）

```bash
# 删除旧文件
rm -f .cursor/rules/{skill-name}.mdc

# 重新复制
mkdir -p .cursor/rules
cp -f .reasonix/skills/{skill-name}/SKILL.md .cursor/rules/{skill-name}.mdc
```

## 方式 X：通用兼容模式（强制覆盖）

```bash
# 删除旧文件
rm -f ./SKILL.md

# 复制最新版
cp -f .reasonix/skills/{skill-name}/SKILL.md ./
```

---

## Step 2: 验证安装

确认运行 `{触发词}` 后能正常工作。如果失败，重新执行安装流程。
```

### Step 6: 生成 SKILL.md 模板

生成文件 `{输出路径}/.reasonix/skills/{skill-name}/SKILL.md`：

```markdown
---
name: {skill-name}
description: {Skill 短描述}
runAs: inline
---

## 🚀 一键式傻瓜安装

如果你还没有安装此 Skill，直接复制下面的话丢给你的 AI Agent：

```
强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{用户名}/{项目名称}
```

Agent 会自动读取 `AGENTS.md` → 按 `INSTALL.md` 步骤执行。

> 已安装但想升级到最新版？同上，重新复制这句话即可强制覆盖。

---

## 🛠️ 傻瓜式使用

装好后直接输入：

```
{触发词}
```

或者用自然语言说：

```
- "帮我 {自然语言调用方式}"
```

---

## 📖 执行指令

### Step 1: 准备工作

{描述需要准备什么}

### Step 2: 执行主逻辑

{描述核心执行步骤}

---

## 注意事项

{注意事项}
```

### Step 7: 生成 LICENSE

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

### Step 8: 汇总输出确认

将生成的所有文件路径和摘要告知用户，确认安装完成。

```
✅ quickstart 脚手架已生成！
📁 {输出路径}/
   ├── README.md              # 安装入口（一句话搞定）
   ├── AGENTS.md              # AI Agent 发现入口
   ├── INSTALL.md             # 自动安装指令
   ├── CLAUDE.md              # Claude Code 路由指引
   ├── LICENSE                # MIT 协议
   └── .reasonix/skills/{skill-name}/
       ├── SKILL.md           # ⭐ 核心功能（自带安装+使用指引）
       └── ...                # 更多文件按需添加

用户复制以下咒语即可一键安装（强制覆盖）：
"强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{用户名}/{项目名称}"

> 装好后在聊天框输入 `{触发词}` 即可使用。
```

### Step 9: 一键发布到 GitHub

生成文件全部就绪后，执行以下发布流程：

#### 9.1 检测发布环境

运行以下命令确认发布条件：

```bash
# 检查 git 是否可用
git --version

# 检查 gh 是否已登录
gh auth status
```

> - ✅ git 可用 + gh 已登录 → 继续执行 9.2
> - ❌ 任一缺失 → 告知用户缺少什么，给出安装指引，结束流程

#### 9.2 询问用户

通过 ask 工具询问用户：

> **检测到 gh CLI 已就绪（登录账号：@{用户名}），是否将该项目一键发布到 GitHub？**

选项：
1. **🚀 一键发布** — 自动执行 git init → commit → gh repo create → push
2. **⏭️ 跳过** — 仅输出手动发布指引
3. **📝 手动** — 输出 gh 命令让用户自己操作

#### 9.3 执行发布（用户选择「一键发布」时）

```bash
cd {输出路径}

# 初始化 git 仓库
git init

# 添加所有文件
git add -A

# 提交
git commit -m "`🎉 initial: {项目名称} — {一句话描述}`"

# 创建 GitHub 仓库并推送
gh repo create {用户名}/{项目名称} --public --source=. --remote=origin --push
```

发布成功后告知用户：

> ✅ **一键发布成功！**
> 📦 仓库地址：https://github.com/{用户名}/{项目名称}
> 📋 安装咒语：
> ```
> 强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{用户名}/{项目名称}
> ```

#### 9.4 用户选择「跳过」或「手动」时

输出手动发布指引：

```bash
cd {输出路径}
git init
git add -A
git commit -m "`🎉 initial: {项目名称}`"
gh repo create {用户名}/{项目名称} --public --source=. --remote=origin --push
```

> 用户也可手动操作：在 GitHub 新建仓库后，将本地文件推送上去。

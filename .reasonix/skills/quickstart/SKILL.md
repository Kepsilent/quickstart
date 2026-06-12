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

> 你只需要配一次 gh，以后任何项目都能直接调 gh 发布，不会再问 SSH 之类的细节。

---

## 使用方式

```
# 1. 进入你的项目目录
cd /path/to/your-project

# 2. 运行 quickstart
/quickstart
```

AI 会自动检测当前目录，完成后续所有操作。

---

## 执行指令

> **重要**：本 Skill 工作在 **当前工作目录**（即你运行 /quickstart 时的目录）。
> 所有生成的文件、git 操作、gh 发布都在当前目录进行，不会创建新目录。

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

在当前目录生成以下文件：

| 文件 | 用途 | 是否覆盖已有 |
|------|------|-------------|
| README.md | 一键安装入口（含咒语） | ask 用户 |
| AGENTS.md | AI Agent 发现入口 | ask 用户 |
| INSTALL.md | 自动安装指令 | ask 用户 |
| CLAUDE.md | Claude Code 路由 | ask 用户 |
| LICENSE | MIT 协议 | ask 用户 |

> 对于每个已有文件，通过 ask 工具询问用户是否覆盖。
> 如果用户选择不覆盖，保留原文件，跳过该文件。

生成后告知用户：

> ✅ 一键安装文件已就绪！
> 用户复制以下咒语即可一键安装你的项目：
> ```
> 强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/{GitHub用户名}/{项目名称}
> ```

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
gh repo create {用户名}/{项目名称} --public --source=. --remote=origin --push
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
gh repo create {用户名}/{项目名称} --public --source=. --remote=origin --push
```
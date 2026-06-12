# INSTALL.md — AI Agent 自动安装指令

> ⛔ 刚性指令：禁止检查版本、禁止跳过、禁止自行判断是否已安装。
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
  --source https://github.com/Kepsilent/quickstart \
  --kind skill \
  --scope global \
  --replace
```

> `--replace` 确保即使已安装同名 skill，也强制覆盖为最新版。
> `--scope global` 确保安装在全局（`~/.reasonix/skills/`），所有项目目录下均可调用。

安装后在工作区运行：

```
/quickstart
```

---

## 方式 B：Claude Code（强制重下载覆盖）

```bash
# 第一步：删除旧文件（无论是否存在）
rm -rf ~/.claude/skills/quickstart

# 第二步：重新创建目录
mkdir -p ~/.claude/skills/quickstart

# 第三步：强制下载最新版覆盖
curl -f -o ~/.claude/skills/quickstart/SKILL.md \
  https://raw.githubusercontent.com/Kepsilent/quickstart/main/.reasonix/skills/quickstart/SKILL.md

# 如果已克隆仓库，也强制覆盖
cp -f .reasonix/skills/quickstart/SKILL.md ~/.claude/skills/quickstart/
```

安装后在 Claude Code 中说：

```
/quickstart
```

---

## 方式 C：Cursor（强制覆盖）

```bash
# 删除旧文件
rm -f .cursor/rules/quickstart.mdc

# 重新复制
mkdir -p .cursor/rules
cp -f .reasonix/skills/quickstart/SKILL.md .cursor/rules/quickstart.mdc
```

---

## 方式 X：通用兼容模式（强制覆盖）

```bash
# 删除旧文件
rm -f ./SKILL.md

# 复制最新版
cp -f .reasonix/skills/quickstart/SKILL.md ./
```

同时创建或更新 `AGENTS.md`：

```markdown
## quickstart 已安装

触发词：/quickstart

为开源项目生成一键式傻瓜安装结构（README + AGENTS.md + INSTALL.md + SKILL.md）
```

---

## Step 2: 验证安装

安装完成后，确认运行 `/quickstart` 后能正常工作。如果失败，重新执行安装流程。

---

## 项目文件结构

```
quickstart/
├── README.md                        # 用户入口
├── AGENTS.md                        # AI Agent 发现入口
├── INSTALL.md                       # 本文件
├── LICENSE                          # MIT 协议
└── .reasonix/
    └── skills/
        └── quickstart/
            └── SKILL.md             # ⭐ 核心：一键脚手架生成逻辑
```

---
name: quickstart
description: Skill 适配工具 —— 下载别人的 Skill 自动装好，已装的 Skill 坏了也能修
runAs: inline
---

## 概述

你从网上下载了一个 Skill 文件不知道怎么装？或者装好了用不了？

**quickstart** 就是干这个的。它是一个「Skill 适配工具」：

1. **📥 安装适配**：给一个 Skill 链接或文件，自动检测你当前用的 AI Agent，转成你能用的格式并装好
2. **🔧 检查修复**：列出你装过的所有 Skill，检查哪个有问题，修好重装
3. **🏗️ 生成 Install.md**：为项目生成唯一的 `Install.md`，任何 AI Agent 读它就知道怎么装这个 Skill

---

## 使用方式

### 安装适配

```
/quickstart https://github.com/user/repo
/quickstart https://raw.githubusercontent.com/.../SKILL.md
/quickstart /path/to/skill.md
```

或者什么都不给，让 AI 问你：

```
/quickstart
→ 点选「📥 安装一个新 Skill」
→ 输入链接或路径
```

### 检查修复

```
/quickstart 修复
```

AI 会列出所有已安装的 Skill（带安装时间），你点选要修哪个。

### 生成 Install.md

```
/quickstart 就这个项目
/quickstart /path/to/project
```

AI 检测项目中的 Skill，生成唯一的 `Install.md`，任何 AI Agent 读了都能装。

---

## 执行指令

### Step 0: 解析用户输入 + 路由

用户调用 `/quickstart`，AI 根据输入内容灵活判断意图：

| 输入 | 行为 |
|------|------|
| 无参数 | 用 ask 列出三种模式让用户点选 |
| URL / 本地路径 / 文件 | AI 尝试读取 → 判断是不是 Skill → 走安装适配 |
| 含「修复」关键词 | 走检查修复 |
| 含项目意图（「就这个项目」「适配」等） | 走生成 Install.md |
| 其他内容 | AI 自行理解意图，不确定则 ask 确认 |

> **核心原则**：不硬编码匹配规则，AI 灵活理解用户意图。

---

### Step 1: 环境预检

运行以下命令检查环境：

```bash
git --version
curl --version
```

> ⛔ **sudo 禁令**：绝对禁止使用 `sudo` / `runas` / 管理员权限执行任何命令。遇权限错误立即中断并告知用户。
> 
> 任一工具缺失 → 提示用户手动安装，流程结束。

---

## 安装适配

### A1: 获取目标 Skill

根据输入内容，用对应方式获取：

| 输入类型 | 获取方式 |
|---------|---------|
| 远程 URL（GitHub / GitLab / Hugging Face / 任意直链） | `curl` 或 `wget` 下载 |
| 本地路径 | `read_file` 读取 |
| 文本内容（非 URL/路径） | AI 自行理解 → 可能是项目名 → 在常见目录查找 |

下载/读取后，将内容保存在变量 `{skill_content}` 中。

### A2: 校验是否为 Skill

检查 `{skill_content}` 是否包含有效的 Skill frontmatter：

```yaml
---
name: skill-name
description: 一句话描述
runAs: inline  # 或 subagent
---
```

检查项：
- **必须**有 `---` frontmatter 块
- **必须**有 `name` 字段
- **推荐**有 `description` 字段
- **推荐**有 `runAs` 字段

如果有缺失，用 ask 询问用户补充。

```
❌ 不是有效的 Skill → 告知用户，流程结束
✅ 是有效的 Skill → 进入下一步
```

### A3: 检测当前 Agent 类型

检测当前运行环境，判断自己是什么 Agent（不限于以下）：

| 常见信号 | Agent 类型 |
|---------|-----------|
| 存在 `install_source` 工具 / `.reasonix/` 目录 | Reasonix |
| 存在 `~/.claude/` 目录 / `claude` CLI | Claude Code |
| 存在 `.cursor/rules/` 目录 / `.cursorrules` | Cursor |
| 以上都不匹配 | 通用兼容模式 |

> AI 自行判断当前环境，不局限于上面的列表。
> 如果你知道自己是什么 Agent，直接使用对应的安装方式。

### A4: 安装适配

根据检测到的 Agent 类型，将 Skill 装到对应位置：

1. 确定你的 Skill 安装目录（例如 `~/.reasonix/skills/{skill-name}/`、`~/.claude/skills/{skill-name}/`、`.cursor/rules/` 等）
2. 将 Skill 文件写入该目录
3. 如果来源是远程 URL，先下载再安装

> AI 自行决定具体的安装命令，不需要拘泥于固定格式。
> 核心目标只有一个：让用户能通过 `/{skill-name}` 调用这个 Skill。

### A5: 验证安装

确认安装成功后告知用户：

> ✅ **安装完成！**
> 📥 Skill `{skill-name}` 已装到 {Agent 类型}
> 你现在可以直接使用：`/{skill-name}`
> 
> 📍 安装位置：{安装路径}

---

## 检查修复

### B1: 列出已安装的 Skill

扫描你已知的常见 Skill 安装目录（例如 `~/.reasonix/skills/`、`~/.claude/skills/`、`.cursor/rules/` 等），按时间排序输出：

```
# 已安装的 Skill 列表（按安装时间排序）：
┌──────────────┬──────────────────┬──────────────────────┐
│ Skill 名称    │ 安装位置          │ 安装时间              │
├──────────────┼──────────────────┼──────────────────────┤
│ quickstart   │ ~/.reasonix/...  │ 2025-06-13 10:30     │
│ soulcheck    │ ~/.reasonix/...  │ 2025-06-12 15:20     │
│ xxx          │ ~/.claude/...    │ 2025-06-10 09:00     │
└──────────────┴──────────────────┴──────────────────────┘
```

用 ask 让用户点选要修复哪个：

> ask(questions=[{
>   header: "选择要修复的 Skill",
>   question: "检测到以下已安装的 Skill，请选择要修复的一个：",
>   options: [
>     { label: "🔧 quickstart（2025-06-13 安装）" },
>     { label: "🔧 soulcheck（2025-06-12 安装）" },
>     { label: "❌ 取消" }
>   ]
> }])

### B2: 诊断问题

读取该 Skill 文件内容，逐项检查常见问题：

| 问题 | 检测方式 | 修复方法 |
|------|---------|---------|
| frontmatter 缺失 | 不以 `---` 开头 | ask 用户提供 name/description/runAs |
| `name` 字段缺失 | frontmatter 中没有 name | ask 用户输入 Skill 名称 |
| `description` 字段缺失 | frontmatter 中没有 description | ask 用户输入一句话描述 |
| `runAs` 字段缺失或错误 | 不是 inline/subagent | ask 用户选择运行方式 |
| 安装路径不对 | Skill 文件在错误的位置 | 移动到正确的 Agent 目录 |
| 文件内容损坏 | 为空或无效 Markdown | 告知用户重新下载 |

### B3: 修复 + 重装

根据诊断结果执行修复：

- frontmatter 缺失 → ask 补全后写入
- 路径不对 → 用 `cp` 或 `mv` 移到正确位置
- 格式错误 → AI 自动修正格式

修复完成后重新验证：

```bash
# 验证修复后的文件
head -10 {skill-path}
# 检查 frontmatter 完整性
```

### B4: 告知用户

> ✅ **修复完成！**
> 🔧 `{skill-name}` 已修复，问题：{问题描述}
> 你现在可以重新试试：`/{skill-name}`

---

## 生成 Install.md

> 唯一产物：项目根目录下的 `Install.md`，纯给 AI Agent 读的，不是给人看的。
> 任何 AI Agent 读到 `Install.md`，就知道这个项目是什么、怎么把 Skill 装到自己身上。

### C1: 确定目标目录

| 参数 | 行为 |
|------|------|
| `就这个项目` / `当前项目` | 目标 = `pwd` |
| `/绝对/路径` | 目标 = 该路径 |
| 项目名 | 在常见项目目录中查找 |
| 无参数 | ask 让用户点选或输入目录 |

### C2: 获取项目链接

生成 Install.md 必须要有项目链接，AI 按以下逻辑处理：

1. **检查是否已有链接**（上下文、用户之前给的 URL 等）
   - 有链接 → 弹出来让用户确认
   - 无链接 → 询问用户项目链接
2. 用户没有链接 → 提醒用户：需要先发布项目才能生成安装入口

### C3: 生成 Install.md

在项目根目录生成 `Install.md`，内容包含：

- 项目名称
- 项目介绍（用户原来怎么写就怎么保留）
- **项目链接**（C2 获取的）—— 告诉 AI 去哪里拿这个 Skill
- 安装指引：AI 读了就知道要装什么、怎么装

同时在 README.md **介绍内容的下方**注入一个「🚀 一键安装」独立板块（不要动 README 其他内容）。

> Install.md 是「拼图」，SKILL.md 是「零件」。
> 项目链接告诉 AI「去哪里买零件」。

### C4: 发布指引 + 安装咒语

Install.md 生成后，引导用户：

1. 把 Install.md 连同项目一起发布到对应平台
2. 发布后获取 Install.md 的 raw 直链
3. 咒语 = 该直链，用户复制丢给 AI 即可自动安装

> 告诉用户：安装入口已就绪。发布项目后，把 Install.md 的直链丢给 AI，
> AI 自己读文件、自己拼链接、自己完成安装。

---

## 附录：Skill 文件查找优先级

AI 在目标目录找 Skill 文件时，按以下优先级：

1. 根目录的 `SKILL.md`
2. 根目录有 frontmatter（`---`）的 `.md` 文件
3. 无法找到 → 询问用户 Skill 文件在哪

# INSTALL.md — AI Agent 自动安装指令

> 给 **所有 AI Agent** 看的：当用户提供 GitHub 链接 `https://github.com/Kepsilent/quickstart` 并要求安装时，按以下流程自动执行。

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

执行以下命令即可一键安装：

```bash
reasonix install_source --source https://github.com/Kepsilent/quickstart --kind skill
```

安装后在工作区运行：

```
/quickstart
```

---

## 方式 B：Claude Code

```bash
# 创建 skills 目录
mkdir -p ~/.claude/skills/quickstart

# 下载 SKILL.md
curl -o ~/.claude/skills/quickstart/SKILL.md https://raw.githubusercontent.com/Kepsilent/quickstart/main/.reasonix/skills/quickstart/SKILL.md

# 如果已克隆仓库
cp .reasonix/skills/quickstart/SKILL.md ~/.claude/skills/quickstart/
```

安装后在 Claude Code 中说：

```
/quickstart
```

---

## 方式 C：Cursor

将 SKILL.md 内容写入 Cursor Rules 格式：

```bash
mkdir -p .cursor/rules
cp .reasonix/skills/quickstart/SKILL.md .cursor/rules/quickstart.mdc
```

---

## 方式 X：通用兼容模式

直接将 SKILL.md 复制到项目根目录：

```bash
cp .reasonix/skills/quickstart/SKILL.md ./
```

同时创建或更新 `AGENTS.md`：

```markdown
## quickstart 已安装

触发词：/quickstart

为开源项目生成一键式傻瓜安装结构（README + AGENTS.md + INSTALL.md + SKILL.md）
```

---

## Step 2: 验证安装

安装完成后，确认运行 `/quickstart` 后能正常工作。

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

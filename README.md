# quickstart 🚀

一键傻瓜式发布助手 —— 为你的项目增加一键安装结构，并一键推送到 GitHub。

---

## 🚀 一键式傻瓜安装

### 一句话搞定

支持：Reasonix · Claude Code · Cursor · Gemini CLI · GitHub Copilot · Windsurf · Cline · Aider …

直接复制下面这行话，丢给你的 AI Agent，就装好了：

```
强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/Kepsilent/quickstart
```

> ⚠️ 重要：AI 不得跳过安装流程，必须删除旧版本后重新下载覆盖。详见 `INSTALL.md`。

Agent 会自动读取 `AGENTS.md` → 按 `INSTALL.md` 步骤执行。当前会话立刻生效。

---

## 🛠️ 傻瓜式使用

装好后，进入你的项目目录，在聊天框里输入：

```
cd /path/to/your-project
/quickstart
```

或者直接用自然语言说：

```
- "帮我把当前项目一键发布到 GitHub"
- "给这个项目加上一键安装"
```

> AI 会在**当前目录**直接操作：添加安装文件 → 询问是否发布 → 一键推送到 GitHub。
> 不会创建新目录，不会污染你已有的项目结构。

> 如果你已经装过但想升到最新版，重新复制顶部的那句话丢给 AI Agent 即可强制覆盖。

---

## 📖 项目介绍

你开源一个新项目，想让用户「复制一句话丢给 AI Agent 就能装好」吗？

**quickstart** 就是干这个的。它是一个「发布助手」Skill：

1. **进入你的项目目录**，运行 `/quickstart`
2. 自动检测 gh 是否就绪，**一次性配好，以后不再问**
3. **在当前目录**生成 README / AGENTS.md / INSTALL.md / CLAUDE.md / LICENSE
4. **主动询问**是否一键发布到 GitHub，选择发布即自动推送

> 你只需要配一次 gh，以后任何项目都能直接调 gh 发布。

> 所有文件生成后，AI **主动询问**是否一键发布到 GitHub，选择发布即可自动推送。

---

## 项目结构

```
quickstart/
├── README.md              # 本文档（安装入口）
├── AGENTS.md              # AI Agent 发现入口
├── INSTALL.md             # 自动安装指令（含防呆预检）
├── CLAUDE.md              # Claude Code 路由指引
├── LICENSE                # MIT 协议
└── SKILL.md               # ⭐ 核心：发布助手逻辑（全局安装）
```

---

## 许可

MIT

# quickstart 🚀

一键式傻瓜安装 + 一键发布 —— 为你的开源项目提供从脚手架生成到 GitHub 发布的一站式自动化流程。

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

装好后，在聊天框里直接输入：

```
/quickstart
```

或者用自然语言说：

```
- "帮我搭一个一键安装的项目结构"
- "给我的新项目生成安装脚手架"
```

> 文件生成后，AI 会检测 gh CLI 并主动询问是否 **一键发布到 GitHub**，选择发布即可自动推送。

> 如果你已经装过但想升到最新版，重新复制顶部的那句话丢给 AI Agent 即可强制覆盖。

---

## 📖 项目介绍

你开源一个新项目，想让用户「复制一句话丢给 AI Agent 就能装好」吗？

**quickstart** 就是干这个的。它是一个 Skill，你运行它，几步操作帮你完成两件事：

**① 生成一键安装结构** — 自动生成 README / AGENTS.md / INSTALL.md / CLAUDE.md / SKILL.md / LICENSE
**② 一键发布到 GitHub** — 检测到 gh CLI 后主动询问，选择发布即可自动完成 git init → commit → repo create → push

从生成到发布，全程自动化。

- `README.md` —— 含一键安装咒语
- `AGENTS.md` —— AI Agent 发现入口
- `INSTALL.md` —— 自动安装指令（含防呆预检）
- `CLAUDE.md` —— Claude Code 路由指引
- `.reasonix/skills/{skill-name}/SKILL.md` —— 技能核心（自带安装+使用指引）
- `LICENSE` —— MIT 协议

> 所有文件生成后，AI **主动询问**是否一键发布到 GitHub，选择发布即可自动推送。

以后你每开源一个项目，跑一下 `/quickstart`，几秒钟整个安装结构就搭好了。

---

## 项目结构

```
quickstart/
├── README.md              # 本文档（安装入口）
├── AGENTS.md              # AI Agent 发现入口
├── INSTALL.md             # AI 自动安装指令
├── CLAUDE.md              # Claude Code 路由指引
├── LICENSE                # MIT 协议
└── .reasonix/
    └── skills/
        └── quickstart/
            └── SKILL.md   # ⭐ 核心：一键脚手架生成逻辑
```

---

## 许可

MIT

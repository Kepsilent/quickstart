# quickstart 🚀

一键式傻瓜安装脚手架 —— 为你的开源项目生成「复制一句话，丢给 AI Agent 就装好」的标准化安装结构。

## 🚀 一键安装

支持：Reasonix · Claude Code · Cursor · Gemini CLI · GitHub Copilot · Windsurf · Cline · Aider …

复制下面这行话，丢给你的 AI Agent，就装好了：

```
请帮我安装这个项目，如果已安装就更新到最新版：https://github.com/Kepsilent/quickstart
```

👆 Agent 会自动读取 AGENTS.md → 按 INSTALL.md 步骤安装。当前会话立刻生效。

装好后：
```
/quickstart
```

---

## 这是什么

你开源一个新项目，想让用户「复制一句话丢给 AI Agent 就能装好」吗？

**quickstart** 就是干这个的。它是一个 Skill，你运行它，它问你几个问题（项目名叫什么、描述是啥、Skill 名叫什么），然后自动生成全套文件：

- `README.md` —— 含一键安装咒语
- `AGENTS.md` —— AI Agent 发现入口
- `INSTALL.md` —— 自动安装指令（自动检测 Reasonix / Claude Code / Cursor / 通用）
- `.reasonix/skills/{skill-name}/SKILL.md` —— 技能核心（带模板，你填逻辑即可）
- `LICENSE` —— MIT 协议

以后你每开源一个项目，跑一下 `/quickstart`，几秒钟整个安装结构就搭好了。

---

## 使用方式

```bash
# 在 Reasonix 中运行
/quickstart
```

然后按提示依次输入项目信息即可。

---

## 项目结构

```
quickstart/
├── README.md              # 本文档（一键安装入口）
├── AGENTS.md              # AI Agent 发现入口
├── INSTALL.md             # AI 自动安装指令
├── LICENSE                # MIT 协议
└── .reasonix/
    └── skills/
        └── quickstart/
            └── SKILL.md   # ⭐ 核心：一键脚手架生成逻辑
```

---

## 许可

MIT

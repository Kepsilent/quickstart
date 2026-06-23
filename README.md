# quickstart 🚀

Skill 适配工具 —— 下载别人的 Skill 自动装好，已装的 Skill 坏了也能修。

---

## 📥 我能帮你什么？

### Model A — 安装适配

你从网上下载了一个 Skill 文件，不知道怎么装到你的 AI Agent 里？

```
/quickstart https://github.com/xxx/xxx
/quickstart /path/to/skill.md
```

给链接或文件路径，AI 自动装好，你马上就能用 `/{skill-name}`。

---

### Model B — 检查修复

你装了个 Skill 但用不了？不知道为什么？

```
/quickstart 修复
```

列出你装过的所有 Skill（带安装时间），选一个，AI 查出问题修好。

---

### Model C — 生成 Install.md

为你的项目生成唯一的 `Install.md`，任何 AI Agent 读它就知道怎么装这个 Skill。

```
/quickstart 就这个项目
/quickstart /path/to/project
```

---

## 🚀 一键安装

复制下面的话丢给你的 AI Agent，就装好了：

```
强制重装这个项目，删除旧版本后重新安装最新版：https://github.com/Kepsilent/quickstart
```

Agent 会读取 `Install.md` 自动完成安装。当前会话立刻生效。

---

## 项目结构

```
quickstart/
├── README.md              # 项目说明
├── SKILL.md               # ⭐ 核心：三模式适配逻辑
├── LICENSE                # MIT 协议
└── .gitignore
```

---

## 许可

MIT

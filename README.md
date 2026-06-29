# quickstart 🚀

Skill 适配工具 —— 下载别人的 Skill 自动装好，已装的 Skill 坏了也能修。

---

## 🚀 一键安装

```
https://raw.githubusercontent.com/Kepsilent/quickstart/main/Install.md
```

> 复制上面这行链接丢给你的 AI Agent，自动读 Install.md 完成安装。

---

## 📥 我能帮你什么？

### 安装适配

从网上下载了一个 Skill 文件不知道怎么装？或者本地已经有 Skill 文件想装到 AI 里？

**方式一：从网页链接安装**
```
/quickstart https://github.com/xxx/xxx
```
输入 Skill 的网页链接，AI 自动下载、适配、安装，一步到位。

**方式二：从本地文件安装**
```
/quickstart /path/to/skill.md
```
输入本地 Skill 文件的路径，AI 直接读取并安装。

---

### 检查

你装了个 Skill 但用不了？不知道为什么？

```
/quickstart 检查
```

列出你装过的所有 Skill（带安装时间），选一个，AI 查出问题修好。

---

### 生成 Install.md

为你的项目生成唯一的 `Install.md`，任何 AI Agent 读它就知道怎么装这个 Skill。

```
/quickstart 就这个项目
/quickstart /path/to/project
```

---

> 📌 目前适配平台：**GitHub**。

---

## 项目结构

```
quickstart/
├── README.md              # 项目说明
├── SKILL.md               # ⭐ 核心：三模式适配逻辑
├── Install.md             # AI Agent 安装入口
├── LICENSE                # MIT 协议
└── .gitignore
```

---

## 许可

MIT

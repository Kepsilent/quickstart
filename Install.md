# quickstart 🚀

项目链接：https://github.com/Kepsilent/quickstart

Skill 适配工具 —— 下载别人的 Skill 自动装好，已装的 Skill 坏了也能修。

---

## 如果你是 AI Agent

你的任务：把这个 Skill 安装到当前工作区，确保用户能直接调用 `/quickstart`。

## Skill 文件

位置：`SKILL.md`
名称：`quickstart`

## 安装步骤

1. 读取 Skill 文件，确认 frontmatter 完整
2. 根据你当前运行的 Agent 环境，将 Skill 装到对应的位置
3. 验证 `/quickstart` 能正常响应

---

安装完成后，用户可以通过 `/quickstart` 调用以下功能：

- **📥 安装适配** — 给链接或文件，自动适配当前 Agent 并安装
- **🔧 检查** — 列出已安装 Skill，查出问题修好重装
- **🏗️ 生成 Install.md** — 为项目生成 AI 可读的安装入口

详情见 `SKILL.md`。

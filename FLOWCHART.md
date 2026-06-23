# quickstart 🚀 — 完整流程图

```mermaid
flowchart TD
    %% ===== 入口 =====
    Start([你用 /quickstart 说了句话]) --> S0{AI 看你说的是什么}
    
    S0 -- 什么都没说 --> Ask[AI 弹出三个按钮让你点]
    S0 -- 给了一个链接或文件路径 --> TryRead[AI 试着读一下]
    S0 -- 说了"修复" --> GoB[去 Model B]
    S0 -- 说了"就这个项目""适配"等 --> GoC[去 Model C]
    S0 -- 说了别的东西 --> Guess[AI 自己猜你要干嘛<br>猜不到就问你]

    TryRead --> IsSkill{是不是一个 Skill？<br>有没有 --- 开头的说明块？}
    IsSkill -- 是 --> GoA[去 Model A]
    IsSkill -- 不是 --> TellUser[告诉你：这不是 Skill 哦]
    
    Ask -- 点"📥 安装" --> GoA
    Ask -- 点"🔧 修复" --> GoB
    Ask -- 点"🏗️ 生成入口" --> GoC

    %% ===== 环境检查 =====
    Check[环境检查<br>看看 git 和 curl 有没有<br>⚠️ 不能用 sudo] --> GoA
    Check --> GoB
    Check --> GoC

    %% ====================================================================
    %% Model A
    %% ====================================================================
    subgraph ModelA[📥 Model A — 装一个 Skill]
        A1[看看你给的是什么<br>远程链接 → 下载<br>本地文件 → 直接读<br>文本 → 自己理解] --> A2[检查是不是合格的 Skill<br>必须有 name / description / runAs<br>缺了就问你] --> A3{合格吗？}
        A3 -- 不合格 --> A3Fail[告诉你：这不是 Skill]
        A3 -- 合格 --> A4[看看你现在用的什么 AI<br>Reasonix / Claude / Cursor / 其他]
        A4 --> A5[装到对应的位置<br>AI 自己知道该放哪<br>不写死任何平台]
        A5 --> A6[✅ 装好了！<br>你现在能用 /{skill-name} 了]
    end

    GoA --> ModelA

    %% ====================================================================
    %% Model B
    %% ====================================================================
    subgraph ModelB[🔧 Model B — 修一个 Skill]
        B1[扫描你装过的所有 Skill<br>按安装时间排好] --> B2[列成表格给你看<br>你点选要修哪个]
        B2 --> B3{检查问题}
        B3 -- 开头没有 --- --> B3a[问你补上 name/description/runAs]
        B3 -- 缺少名称 --> B3b[问你叫啥名]
        B3 -- 缺少描述 --> B3c[问你干嘛用的]
        B3 -- runAs 不对 --> B3d[问你选 inline 还是 subagent]
        B3 -- 装错位置了 --> B3e[移到正确的地方]
        B3 -- 文件坏了 --> B3f[告诉你重新下载]
        B3a --> B4
        B3b --> B4
        B3c --> B4
        B3d --> B4
        B3e --> B4
        B3f --> B4[修好了重新验证]
        B4 --> B5[✅ 修好了！<br>再试试 /{skill-name}]
    end

    GoB --> ModelB

    %% ====================================================================
    %% Model C
    %% ====================================================================
    subgraph ModelC[🏗️ Model C — 生成 Install.md]
        C1[确定你要弄哪个项目<br>当前目录 / 指定路径 / 我选] --> C2[提取信息<br>项目名 / Skill 文件在哪 / 描述]
        C2 --> C3[问你项目地址<br>GitHub / Hugging Face / 随便<br>不填也行]
        C3 --> C4[生成一个 Install.md<br>里面写清楚：<br>• 这是什么项目<br>• Skill 文件在哪<br>• AI Agent 该怎么装]
        C4 --> C5[✅ Install.md 已生成！<br>别人复制这句话就能装：<br>强制重装...新版：{你的项目地址}]
    end

    GoC --> ModelC

    %% ===== 附录 =====
    SubNote[📌 找 Skill 的优先级<br>① 根目录的 SKILL.md<br>② 有 --- 说明块的 .md 文件<br>③ 都找不到 → 问你]
```

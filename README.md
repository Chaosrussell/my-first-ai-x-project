# My First AI+X Project 🚀

> 面向**零基础大学生**的"第一个 AI 项目"引导教程。
> 教任何专业的学生**用 Agent 亲手做出、迭代并合规发布**一个和自己专业相关的真实作品 —— 不讲原理、不写代码。

这是一个 [Hermes Agent](https://hermes-agent.nousresearch.com) 技能(skill)。装上之后,学生只要说一句"我是学 XX 的,想做个 AI 项目,零基础",Agent 就会自动带他走完从**找项目 → 做出成果 → 加界面 → 发布到 GitHub**的全过程。

**它解决什么问题?** 大多数 AI 教程在"教 AI 技术";这个教程反过来 —— 用 AI **消除**大学生对 AI 的畏惧和"怕麻烦",让他第一次就亲手指挥一个 Agent 把一件专业相关的真事办成,拿到能写进简历的作品。

---

## 👀 效果预览(节选真实引导)

```
🤖 你是学什么专业的呀?
👤 金融
🤖 你想怎么学?1️⃣ 我做给你看  2️⃣ 你自己动手,我当教练
👤 1
🤖 我去 GitHub 和中文社区搜了一圈,帮你挑了 4 个,从最简单开始 👇
   🟢 项目1｜10分钟｜财报"人话翻译机" —— 让 Agent 联网查一家公司财报,翻成大白话
   🟡 项目2｜行业赛道扫描   🟡 项目3｜造一个投研机器人   🔴 项目4｜TradingAgents ⭐90K(参考)
👤 1
🤖 看我来一遍——注意,我没有编数据,是真的调用了联网工具去查……
   [Agent 真实联网 → 生成一份带数据来源的"公司财务体检报告"]
🤖 🎉 这就是你的第一个 AI 项目!全程你没写一行代码。
   接下来我们还能给它加个界面、发布到 GitHub 变成你的作品……
```

> 完整回放见开发记录(未随仓库公开)。三种引导路径均已真机验证 —— 见下方「验证状态」。

---

## 🚀 快速开始

**前置:** 装好 [Hermes Agent](https://hermes-agent.nousresearch.com)(一个开源 AI Agent),并接一个国内大模型(DeepSeek 等)。零基础学生看 [`使用文档-学生版.md`](使用文档-学生版.md) 有手把手步骤(含 API 申请)。

**安装本 skill,两种方式任选:**

**方式 A(推荐零基础):让 Hermes 帮你装** —— 打开 Hermes,发这句话给它:
```
帮我把 "<本文件夹路径>/skill" 安装到我的 Hermes 里
(放到 ~/.hermes/skills/education/my-first-ai-x-project),装好后刷新并确认已启用。
```
它会自己完成复制、`/reload-skills`、验证 —— 你不用记命令。

**方式 B(会用终端):**
```bash
cp -R "skill" ~/.hermes/skills/education/my-first-ai-x-project
hermes skills list | grep my-first-ai-x-project   # 确认已装;在 Hermes 里 /reload-skills 刷新
```

装好后,对 Hermes 说一句"我是学 XX 的,想做个 AI 项目,零基础"即可启动。

---

## 🎯 教程做什么(六阶段)

| 阶段 | 做什么 |
|------|--------|
| 0 建画像 | 逐条问(专业/年级/基础/需求/时间/学习模式),一次一个 |
| 1 真检索 | 用 Agent 真去 GitHub/网上搜适合该专业的项目 |
| 2 给菜单 | 2-5 个,由简到难;入门可单功能,其他要可复用/带 GUI;**末尾必带 ✍️「我自己有想法」出口** |
| 3 选择 | 回数字选;或选 ✍️ 自己描述想法 → Agent 评估可行性(太大就砍成 MVP,范围学生拍板) |
| 4 引导 | 🎬 演示模式(做给看,点破 Agent 调工具) / 🛠️ 实操模式(学生亲手,教练窗监听共享目录) |
| 5 继续开发 | 诊断项目 → 针对它**动态推荐 2-4 条强化路径**(加界面/接数据源/自动化/封装…)→ 学生自选并用 Agent 真做出来 ⛔ 硬门槛:必须真做出来 |
| 6 发布 | 用 Agent 规范发布到 GitHub ⚖️ 合规红线:课程作业先问 / 版权 LICENSE / 隐私 / 密钥扫描 |

**贯穿设计:** 给带 `【替换成:…】` 标注的动态模版(不给死 prompt),学生可提交审核;成就收尾时点破"指挥 Agent 只需说清四件事:扮谁、做什么、什么要求、给什么形式"这一可迁移能力。

---

## 📁 文件夹结构

```
├── README.md              项目总入口(你在看的这份)
├── LICENSE                MIT
├── 使用文档-学生版.md       给学生:装 Hermes、连模型(含 API 申请)、装 skill、启动
└── skill/                 教程本体(Hermes skill)
    ├── SKILL.md           主体:六阶段流程 + 双模式 + 交互铁律
    └── references/
        ├── search-playbook.md          检索配方 · 各专业关键词 · 兜底
        ├── project-menu-templates.md   项目菜单模板 · 金融范例
        ├── prompt-templates.md         动态模版库 · 标注填空 · 审核清单
        ├── step-by-step-scripts.md     演示/实操双模式脚本
        ├── develop-and-publish.md      继续开发 + 加 GUI + 封装 + GitHub 合规发布
        └── testing-and-validation.md   (维护者)如何真机验证本 skill
```

> `skill/` 是运行原件(`~/.hermes/skills/education/my-first-ai-x-project/`)的同步副本;改动任一处记得同步另一处。

---

## ✅ 验证状态

| 引导路径 | 状态 |
|---------|------|
| 演示模式 | ✅ 真机(子 Agent 实跑,真检索 + 真查财报) |
| 实操模式 · 监听机制 | ✅ 真机 |
| 实操模式 · 真双窗口多轮 | ✅ 真机(tmux 活会话) |
| 阶段 5/6 引导逻辑 | ✅ 就位(真机发布需学生本人 GitHub 授权) |

---

## 📄 许可

MIT License · 详见 [LICENSE](LICENSE)。本项目为原创教程,基于 [Hermes Agent](https://github.com/NousResearch/hermes-agent) 平台。

作者:Chaosrussell · 语言:中文 · 目标用户:任何专业的零基础大学生

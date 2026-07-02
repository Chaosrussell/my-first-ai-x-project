# My First AI+X Project

面向**零基础大学生**的"第一个 AI 项目"引导教程 —— 教任何专业的学生**用 Agent(Hermes)**做出、迭代并发布一个和自己专业相关的真实作品。

> 核心理念:不讲原理、不写代码,用 Agent 消除对 AI 的畏惧;根据学生专业实时检索真实项目,由简到难,手把手带到出成果,再迭代成可复用作品并合规发布到 GitHub。

---

## 📁 文件夹结构

```
My first AI+X project/
├── README.md              ← 你在看的这份(项目总入口)
├── 使用文档-学生版.md       ← 给学生:怎么装 Hermes、装 skill、一句话启动
└── skill/                 ← 教程本体(Hermes skill 源码副本)
    ├── SKILL.md           ← 主体:六阶段流程 + 双模式 + 交互铁律
    └── references/
        ├── search-playbook.md          检索配方 · 各专业关键词 · 兜底
        ├── project-menu-templates.md   项目菜单模板 · 金融范例
        ├── prompt-templates.md         动态模版库 · 标注填空 · 审核清单
        ├── step-by-step-scripts.md     演示/实操双模式脚本
        ├── develop-and-publish.md      继续开发 + 加GUI + 封装 + GitHub合规发布
        └── testing-and-validation.md   (维护者)如何真机验证本 skill
```

> 开发过程记录(`全流程报告.md`、`验证对话记录.md`)存放在本文件夹的**上一级目录**,不属于交付/上传内容。

---

## 🚀 怎么用(安装到 Hermes)

`skill/` 里是 skill 的**源码副本**。要让 Hermes 真正加载运行,需把它放到 Hermes 的 skills 目录。两种方式任选:

### 方式 A(推荐零基础):直接让 Hermes 帮你装

打开 Hermes,把下面这句话发给它(把路径换成本文件夹的实际位置):

```
帮我把 "<本文件夹路径>/skill" 这个 skill 安装到我的 Hermes 里
(放到 ~/.hermes/skills/education/my-first-ai-x-project),装好后刷新并确认它已启用。
```

Hermes 会自己完成复制、`/reload-skills`、并验证 `hermes skills list` —— 你不用记命令。

### 方式 B:自己动手(会用终端的话)

```bash
# 复制到 Hermes skills 目录(教育分类下)
cp -R "skill" ~/.hermes/skills/education/my-first-ai-x-project

# 在 Hermes 里刷新
#   /reload-skills
# 确认已装
hermes skills list | grep my-first-ai-x-project
```

装好后,学生只要对 Hermes 说一句"我是学 XX 的,想做个 AI 项目,零基础",skill 就会自动接管引导。详见 `使用文档-学生版.md`。

> 注:本仓库当前运行的 skill 原件在 `~/.hermes/skills/education/my-first-ai-x-project/`,`skill/` 是同步副本,便于集中管理与交付。改动任一处后记得同步另一处。
>
> Hermes 官网 https://hermes-agent.nousresearch.com · 文档 https://hermes-agent.nousresearch.com/docs

---

## 🎯 教程做什么(六阶段)

```
阶段0 建画像   逐条问(专业/年级/基础/需求/时间/学习模式),一次一个
阶段1 真检索   用 Agent 真去 GitHub/网上搜适合该专业的项目
阶段2 给菜单   2-5 个,由简到难;入门可单功能,其他要可复用/带GUI
阶段3 选择     回数字即可
阶段4 引导     🎬演示模式(做给看,点破Agent调工具) / 🛠️实操模式(学生亲手,教练窗监听共享目录)
阶段5 继续开发 用Agent加GUI(Agent推荐→学生选)/加功能/封装成通用Hermes skill  ⛔硬门槛:必须真做出来
阶段6 发布     用Agent规范发布到GitHub  ⚖️合规红线:课程作业先问/版权LICENSE/隐私/密钥扫描
```

**贯穿设计**:给带 `【替换成:…】` 标注的动态模版(不给死 prompt),可提交审核;成就收尾 + 点破"指挥 Agent 四件事"可迁移能力。

---

## ✅ 验证状态

| 路径 | 状态 |
|------|------|
| 演示模式 | ✅ 真机(子 Agent 实跑,真检索+真查财报) |
| 实操模式-监听机制 | ✅ 真机 |
| 实操模式-真双窗口多轮 | ✅ 真机(tmux 活会话) |
| 阶段5/6 引导逻辑 | ✅ 就位(真机发布需学生本人 GitHub 授权) |

详见上一级目录的 `验证对话记录.md`。

---

作者:CR · 适配平台:Hermes Agent · 语言:中文 · 目标用户:任何专业的零基础大学生

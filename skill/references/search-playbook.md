# 检索配方:如何为不同专业找到"零基础可上手"的 AI 项目

这份文档告诉你(AI)在阶段 1 具体怎么搜、搜什么、怎么过滤。

## 通用检索流程

1. **先用 web_search** 搜中文优先的入口(覆盖面最广):
   - `[专业] AI 能做什么 项目`
   - `[专业] AI 工具 推荐 2025`
   - `[专业] 大学生 AI 入门 案例`
   - `[专业] + AI + no code / 不用编程`

2. **再用 GitHub API 补真实项目**(可选,给"参考种子"用):
   ```
   curl -s "https://api.github.com/search/repositories?q=<关键词>&sort=stars&order=desc&per_page=10"
   ```
   解析出 `full_name` / `stargazers_count` / `description` / `html_url`。

3. **过滤**——GitHub 结果里大量是需要编程的重型项目。判断标准:
   - README 里出现 "pip install" / "clone" / "运行环境" / "Python" → 归为 🔴 参考向,不作第一个项目
   - 是"提示词合集""工作流模板""无代码工具"→ 可作 🟢🟡
   - 中文无关噪音(书单、政治仓库)→ 直接丢弃

## 工具用不动时怎么办(实测优先用结构化 JSON 接口,别死磕网页搜索)

实测:用 browser 打开 Bing / DuckDuckGo 的网页搜索经常**拿不到结果**——Bing 是 SPA,快照常常一片空白;DuckDuckGo 会弹**验证码(CAPTCHA)**。别在网页搜索上反复重试浪费轮次,直接改用**返回干净 JSON 的接口**,这些既稳又能拿到真实数字/星数:

- **找 GitHub 项目 → GitHub Search API**(无需登录,直接出真实 star 数):
  `https://api.github.com/search/repositories?q=<关键词>&sort=stars&order=desc&per_page=8`
  用 browser 打开后,在 console 里 `JSON.parse(document.body.innerText).items.map(r=>({name:r.full_name,stars:r.stargazers_count,desc:r.description,url:r.html_url}))` 解析。
  (已验证真实结果:TauricResearch/TradingAgents ⭐90K、hsliuping/TradingAgents-CN ⭐29K,可作金融 🔴 参考种子。)

- **查 A 股上市公司真实财报(演示模式「财报体检」必用)→ 东方财富 datacenter JSON 接口**:
  `https://datacenter-web.eastmoney.com/api/data/v1/get?reportName=RPT_LICO_FN_CPD&columns=ALL&filter=(SECURITY_CODE%3D%22300750%22)&sortColumns=REPORTDATE&sortTypes=-1&pageSize=3&source=WEB&client=WEB`
  把 `300750` 换成目标股票代码。`columns=ALL` 最省事(字段名易记错,直接全拿再挑)。返回字段:`REPORTDATE` 报告期、`TOTAL_OPERATE_INCOME` 营收、`PARENT_NETPROFIT` 归母净利、`BASIC_EPS` 每股收益、`WEIGHTAVG_ROE` 净资产收益率、`YSTZ` 营收同比、`SJLTZ` 净利同比。
  注意:`push2.eastmoney.com` 的行情快照接口常超时,别依赖它;datacenter 接口够出一份财务体检报告了。

## 兜底原则(搜不到理想项目时)

**永远有一个不依赖检索的兜底项目**:让学生用 Hermes 这个 Agent 对自己专业里"最花时间的一件真实苦活"做一次提效。例如:
- 读一份本专业的复杂材料并产出结构化笔记
- 分析一份本专业的真实数据/文档
- 生成一份本专业相关的展示成果

这个兜底方案零门槛、必然成功,用来保证"第一个项目一定能出成果"。

## 各专业检索关键词速查

| 专业类 | 核心检索词 | 典型可上手方向 |
|--------|-----------|---------------|
| 金融/经管 | 财报分析 / 行业研究 / 估值 + AI | 财报"翻译"、行业扫描、投研助手 |
| 法学 | 合同审查 / 判决书 / 法律 + AI | 合同风险清单、案例检索与解读 |
| 医学/生物 | 文献阅读 / 病例 + AI | 论文速读笔记、医学名词科普卡 |
| 理工/计算机 | 数据分析 / 可视化 + AI | 数据清洗+画图、实验数据报告 |
| 文史哲 | 史料梳理 / 文本分析 + AI | 时间线、观点对比、文献综述 |
| 新传/设计 | 内容策划 / 文案 / 配图 + AI | 策划案、分镜脚本、视觉方案 |
| 教育/语言 | 教案 / 翻译 / 学习 + AI | 教学素材、双语对照、口语陪练 |
| 心理 | 量表 / 访谈 / 文本 + AI | 访谈稿分析、量表解读 |

## 检索出的项目,呈现给学生前的最后检查

- [ ] 至少 2 个是"让 Agent 直接联网/读文件就能做"的零门槛项目(🟢)
- [ ] 由简到难排好序了
- [ ] 每个都标了五要素(做什么/用什么/产出物/为什么/难度)
- [ ] 给的链接是真实可访问的(不是编的)
- [ ] 术语都用大白话解释过了

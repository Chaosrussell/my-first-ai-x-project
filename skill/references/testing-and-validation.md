# 如何验证/演练这个 skill 的引导效果(给作者/维护者)

当你想真机跑一遍这个 skill、检查引导质量,或给用户看"实际引导起来什么样"时,用下面三个已验证可行的方法(由轻到重)。核心原则:**引导逻辑、检索结果、成果数据都要真跑,只有学生的回答可以模拟。**

诚实原则:如果某段"教练↔学生"来回是你补写的(而非真进程真多轮),在交付回放里必须如实标注哪些是真跑、哪些是模拟。方法一/二的来回对话是补写的,方法三是真多轮。

## 方法一 · 演示模式回放(用 delegate_task)

演示模式是单窗口、多轮独白式,适合让一个子 Agent 完整演出来:

- 用 `delegate_task`,toolsets 给 `["web", "browser", "skills", "file"]`。
- 在 goal 里要求子 Agent:先 `skill_view` 读主 skill + 全部 references,再扮演"引导者 🤖 + 学生 👤"逐轮回放,**并真的调 web_search/browser 检索**(GitHub API 拿真实 star 数、真联网查一家上市公司财报),最后产出可原样转交的中文对话回放。
- 在 context 里注入学生画像(如"学金融、大二、零基础、无偏好、选演示模式")+ 语言要求(中文)。
- 已验证:子 Agent 能真实拉到 TradingAgents 真实 star 数 + 某公司真实财报,产出高质量回放。

## 方法二 · 实操模式验证(真机双窗口 + 共享目录监听)

实操模式的核心新机制是"教练窗监听学生窗口的产出",必须真机验证,不能只模拟:

1. 备好共享目录:`mkdir -p ~/ai-x-workspace`(先清空,避免旧文件干扰)。
2. **真起一个"学生的 Agent 窗口"**:后台跑 `hermes chat -q "<学生填好的指令,末尾要求把成果存成文件到当前文件夹>"`,cwd 设为 `~/ai-x-workspace`。这个子进程会真的联网查数据、真的写文件。
3. **教练窗(你)真监听**:用 `search_files(pattern="*", target="files", path="~/ai-x-workspace")` 检测新文件,再 `read_file` 读全文,基于真实产出点评。
4. 验证通过标准:文件真被写出、教练窗真读到、点评引用的每个数字都来自该文件。
5. 跑完 kill 掉后台进程,清理 `~/ai-x-workspace`。

## 方法三 · 真·双窗口多轮(tmux)——最接近真实实操模式

方法二只验证了监听链路,来回对话是补写的。要真机验证\"活的学生窗口 + 多轮\",用 tmux 起一个持续的 hermes 会话:

1. 起会话:`tmux new-session -d -s student -x 140 -y 45`,再 `tmux send-keys -t student 'export CONDA_NO_PLUGINS=true; cd ~/ai-x-workspace; hermes --cli' Enter`。
2. 等就绪(约 15-20s),`tmux capture-pane -t student -p | tail -20` 确认出现 `❯` 输入框。若被 conda 报错刷回 shell 提示符,再 `send-keys 'hermes --cli' Enter` 拉一次。
3. 发指令卡:`tmux send-keys -t student '<学生填好的指令,末尾要求存成文件>' Enter`。
4. 教练窗监听:`search_files` 第一拍空(还在查,符合预期)→ 隔 30-60s 再看,检测到文件后 `read_file` 读全文点评。
5. 观察 Agent 真实动作:`tmux capture-pane -t student -p | tail -20` 能看到它 联网→execute_code→write_file 的真实工具轨迹。
6. 收尾:`tmux send-keys -t student '/quit' Enter` → `tmux kill-session -t student` → 删 `~/ai-x-workspace/*` → 确认 `tmux ls` 无残留、无活进程。

## 环境坑:spawn 出来的 hermes 被 conda 报错淹没

在这台/类似环境里,shell 启动会触发 conda 插件报错(`An unexpected error has occurred. Conda has prepared the above report.`),把 spawn 出来的 `hermes` 输出淹没、看似卡死。

**修复**:spawn hermes 前 `export CONDA_NO_PLUGINS=true`(或整行前缀 `CONDA_NO_PLUGINS=true hermes ...`)。加上后 hermes 干净启动。注意后台 background 进程可能仍走 shell profile 打印 conda 噪音到 stderr,但只要 `-q` 任务完成、文件落地即视为成功——不要因为 stderr 里的 conda 噪音判定失败。

## 其它已知坑

- 如果 `tmux` 不在:conda 环境可 `conda install -y --solver=classic tmux` 装(注意加 `--solver=classic`,因为 `CONDA_NO_PLUGINS=true` 会禁用 libmamba solver 导致报错)。方法三依赖 tmux。同理装 `gh` 需 `conda install -y --solver=classic -c conda-forge gh`(gh 不在 anaconda 默认频道,要加 conda-forge)。
- macOS 无 `timeout` 命令;别用它包裹 hermes。
- tmux 里启动 hermes 时,若 conda 报错把命令刷没了、回到 shell 提示符,直接再 `send-keys 'hermes --cli' Enter` 拉一次即可(export 已在环境里)。

## 坑:交互式 TUI 登录(gh auth / 任何需要真 TTY 的向导)后台驱动不可靠

阶段 6 发布到 GitHub 时会用到 `gh auth login`。它(以及 hermes 交互式界面)用的是需要**真 TTY** 的提示库,通过 `terminal(pty=true, background=true)` + `process(action=submit)` 去喂 Y/回车**经常推不动**,会卡在第一个提问上。别在这上面反复硬耗。

两个应对,按顺序:
1. **清环境启动,尽量减少噪音**:`env -i HOME="$HOME" PATH="/usr/bin:/bin:/usr/sbin:/sbin" <绝对路径>/gh auth login --hostname github.com --git-protocol https --web`。`env -i` 绕开 shell profile 的 conda init,让 gh 的 device code 不被淹没。但即便如此,交互式选择项仍可能推不动。
2. **把交互步骤交还给用户(首选兜底)**:让用户在**他自己的真实终端**里跑 `gh auth login ...`(他有真 TTY + 浏览器在手,device-code 授权几秒完成),完成后回来说一声;登录态是持久的,之后建仓库/推送(`gh repo create`、`git push`)全部非交互,你可以接管跑完。这与阶段 6"授权步骤停下来提示学生操作、不替他填凭证"的红线完全一致。

判定成功:`gh auth status` 显示已登录即可继续。不要因为后台 PTY 里残留的 conda stderr 噪音就判定登录失败——以 `gh auth status` 为准。

## 演练后常见的可改进点(历史发现,供 review 时对照)

- 演示模式独白偏长 → 中途插一个极低成本互动(如"你想让我查哪家公司?随便说一个"),提升代入感。
- 财报/数据类成果要让 Agent **标数据来源与时点**,并提示学生重要数字自行复核(金融场景尤其重要)。

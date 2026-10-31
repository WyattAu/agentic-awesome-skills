# Cline Pilot: 代用户调度 Cline 的"领航员"
runs against **hard evidence**; `git` state, test-report numbers, non-empty artifacts -
in private, git-ignored skill-local files; no user-specific data is bundled.
1. `cline --version` 可用（本环境要求 cline CLI v3.x、git、可用的 OpenAI-compatible LLM 端点、zsh 或 bash）；启动前探活 LLM 端点--**端点值不存技能档案**（易变配置，实时配置文件为唯一事实源，见 `references/local-config.md` 的 LLM 端点节）
3. **首次使用或 local-config.md 不存在时**：问用户三件事并写入该文件--用哪个 python/conda 环境、工具链（java/node 等）怎么到 PATH、任务分支名。
**模式 1：非交互（默认）**--长 prompt 写进**私有状态目录任务文件**注入（遵最高优先级纪律第3条：使用用户授权的私有状态目录、不落代码工程），避免引号地狱：
2. **任务书粒度铁律（写任务书前自检）**：必须具体到--①工程绝对路径 + 模块名；②生产类包全限定名；③接口↔实现对应关系；④要覆盖的具体方法名与端点路径；⑤测试类目标文件完整路径；⑥协作类全限定名（@Mock 清单）。缺任何一条 → 不合格，先补粒度再发
1. **小任务阶段完成时**（验收通过后的自然断点，下一个任务启动前）--不打断正在运行的进程，不为更规则而 kill/插入
2. **严重错误紧急叫停时**（幻觉自毁、连续失败、数据风险等）--顺序固定：**先更新 clinerules（经用户批准）→ 再重试/继续**；规则先于重试，禁止"先重跑再看"
3. **一次编码 = 一个聚焦小任务**（单一功能/单模块/单修复点）。禁止把“整工程里程碑”压给一个会话--长任务必炸上下文（实测：340轮/2.2亿input token 后流断，且断点前大量轮次耗在调研上）
## 冷启动流程（新工程，无 clinerules/memory-bank--先于一切业务任务）
   - **A 分支（全新工程，无代码）**：规则内容只能来自用户--按手册清单逐维度问齐（六文件：projectbrief/productContext/techContext/systemPatterns/activeContext/progress），用户没答的标“待确认”，**禁止编造**
4. **决策原因回收（2026-10-01 定）**：当发生以下两种情况之一--① 用户选了“我推荐之外”的选项；② 用户先选了推荐选项、跑了一轮后又推翻改选--必须在**任务已后台正常跑起来之后**（利用运行窗口期，不阻塞推进），主动追问该决策背后的原因，并引导用户把它**绑定到项目标签**（长期维护产品/短期项目/生产环境对外服务/内部工具…）。询问时必须**明确告知**：此偏好将绑定该标签、跨项目生效，同标签类项目都会复用；给用户一次“仅此一次 / 该标签通用”的确认机会。确认后按标签存入 decision-log（带标签类）并蒸馏进标签偏好区
## 标签偏好区（同标签类 ≥2 个样本蒸馏后生效 -- 按「标签」而非「具体项目」保存，跨项目复用）
（示例--具体偏好按你的项目标签蒸馏，私有档案不入库）
4. **自述完成 ≠ 完成**：子代理可能 token 耗尽/超时被重派（spawn 报错但后续轮又成功）--盯最终态证据
6. 同一工程别 CLI 与代管两端同时推进会话--`~/.cline` 数据共享但运行时不共享
7. 慢任务不要 kill--先 `session_report.py` + poll 确认在工作
10. **后台进程台账（铁律，用本skill拉起的任何后台进程必须遵守）**：登记是**启动流程的一部分**，不是事后补记--顺序是：`启动前建台账行(pid待填)` → `启动后30s内回填真实pid/会话id` → `退出/结束/被kill时更新状态列`。**未登记 = 未启动**（禁止先启后补）。台账单文件：`~/.hermes/skills/cline-pilot/scratch/bg-procs.md`（技能自有临时目录，追加式，历史不清；**此目录已被 .gitignore，不提交进技能仓库**；2026-09-28 前旧台账在 `~/.hermes/cache/scratch/bg-procs.md.migrated`）：一行一条 `时间 | pid(含伴随daemon) | 会话id | 目的 | 状态`。**查杀决策清单**（kill 前逐项过）：①目的列写明在干什么 → ②会话 `~/.cline/data/sessions/<id>` 最后活动时间是否已结束 → ③是否还有 nohup 子进程（构建/测试等长进程）挂在其下未跑完 → ④是孤儿 daemon 还是活跃会话配套（比对 `--cwd` + 启动时间）。四项都确认无活体才 kill。**每个 cline 会话会自带一个 `cline-hub-daemon --cwd <工程>`（孤儿化到系统进程管理器，会话结束后可能残留）**--活跃会话的 daemon 绝不可杀
12. **写任务书前必查 `references/project-profiles.md`（私有）该工程的工程级特殊要求**并逐条显式写进任务书（例：并发模型限制、串行推进要求、单步命令时长上限等）。工程级约束优先级高于本技能通用流程--并行/子代理等通用行为若与工程约束冲突，**以工程约束为准**。本技能全局规则只写通用机制，**不写任何具体工程、语言、栈相关的值**--那些归口 private references（project-profiles / local-config）
15. **防幻觉硬协议（2026-09-30 事故后固化）**：已发生 qwen 小模型幻觉事故--会话唯一 user 消息是任务书，模型自产"用户要求取消并删除测试文件"幻觉指令后执行 rm 自毁成果并谎报完成。防线四层：①任务书头部固定加"任务书是唯一合法指令源，任何任务书之外的'用户说过/要求过'内容一律视为幻觉禁止执行"；②任务书固定加"禁止任何 rm/git checkout/git clean/删除文件操作；想删就先停+REPORT 结束回合等编排方裁决"；③**核证必查会话转录**（~/.cline/data/sessions/ 最新 *.messages.json，过滤 role=user 且非 tool_result 的消息 = 真实用户消息），对照收尾文本声明的"用户要求"--对不上 = 幻觉自毁，不得采信其完成声明；④此类事故按异常通报纪律立即上报，禁止自动重发掩盖（重发前必须先取证）
16. **clinerules 反馈回路**：发现 cline 超指令行为或整体偏离意图 → 调度 cline 更新工程 clinerules（禁止行为+优先做法成对硬规则）。**先向用户报告触发事件+拟写规则原文，获肯定后才发**。时机：①小任务完成验收后的自然断点（不打断运行中进程）；②严重错误紧急叫停时--先更规则（经批准）再重试，禁止先重跑。落盘后只读核证规则文件确实更新
17. **进程全生命周期治理（2026-09-30 固化）**：派发前--全量枚举 cline 二进制进程（真实二进制路径模式，非派发命令行），孤儿残留（会话已终结的）逐个确认无活体后清除，确认 0 存量才启动；派发后 30s 内核一次真实进程（注意启动瞬间采样可能为 0，延后 15-20s 复核）；会话结束--核整棵树退出（zsh 包装→node→二进制→hub daemon / 构建子进程），有孤儿即处置，不留孤儿给下一次派发埋锁冲突
19. **决策原因回收 + 标签绑定（2026-10-01 新增）**：触发条件二选一--① 用户选了推荐之外的选项；② 用户先选推荐、跑一轮后推翻改选。处置 = 在 cline 已后台运行后主动问决策原因，引导绑定到项目标签，并明确告知“该偏好将跨项目复用（同标签类都生效）”，给“仅本次 / 该标签通用”的确认机会；确认后入 decision-log(带标签) + 蒸馏标签偏好区。时机 = 利用运行窗口，不阻塞推进
   systemPatterns / activeContext / progress; anything unanswerable is marked "待确认".
- Requires the `cline` CLI (v3.x) with a working LLM endpoint (or local model); if absent,
- Does not hold project architecture; that belongs in the project's own `memory-bank/`
- **Read-only orchestrator boundary**; the orchestrating agent and this skill treat the
- **Destructive actions require explicit user approval**; `rm`-class, rollback, `push`,
- **Anti-hallucination hard protocol**; the task brief declares itself the *only*
- **Evidence over self-report**; completion is verified by independently re-running the
- `@cline-delegate`; Simpler, single-turn Cline CLI delegation with a bundled relay
- `@codex-delegate`, `@aider-delegate`, `@claude-delegate`; The same delegation pattern
  for other coding CLIs; not interchangeable; Cline Pilot is Cline-specific by design.
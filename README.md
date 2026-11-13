<div align="center">

# 🧠 Vic Awesome Memory

**全网最好用的 AI Agent 记忆系统 · 论文 · 架构 · 评测，一份就够**
Curated AI-agent memory systems, papers, architectures and benchmarks — organised by how the human brain remembers.

`31 个开源系统` · `20+ 篇核心论文` · `人脑 ↔ AI 记忆映射` · `选型指南` · `数据更新于 2026-11-13`

姊妹项目 → [**vic-memory**](https://github.com/voidgazer009/vic-memory)：我们自己的类人脑记忆体系，适配任意 Agent

</div>

---

## 为什么要有这个仓库

大模型本身**没有记忆**：上下文窗口一关，一切归零。所谓"长上下文"只是更大的草稿纸——贵、慢、而且越长越容易"中间遗忘"。
真正的记忆要回答五个问题：**记什么、存哪里、怎么找、何时忘、如何进化**。
这个仓库把全网最好的答案按「人脑记忆」的框架整理出来，每一条都标注来源、许可证和一句话点评。

## 目录

- [一张图看懂：人脑记忆 → AI 记忆](#一张图看懂人脑记忆--ai-记忆)
- [六大流派](#六大流派)
- [开源系统榜单（按 Star）](#开源系统榜单)
- [核心论文](#核心论文)
- [评测基准](#评测基准)
- [怎么选](#怎么选)
- 深度文档：[人脑映射](docs/brain-map.md) · [架构流派详解](docs/architectures.md) · [评测与水分](docs/benchmarks.md) · [选型指南](docs/selection-guide.md)
- 机器可读数据：[data/systems.csv](data/systems.csv)

## 一张图看懂：人脑记忆 → AI 记忆

| 人脑 | 作用 | AI 对应 | 代表系统 |
|---|---|---|---|
| 感觉记忆 Sensory | 几百毫秒的原始输入，绝大多数被过滤 | 输入过滤 / 压缩 | LightMem 感觉过滤层 |
| 工作记忆 Working (~4±1 组块) | 正在思考的东西 | 上下文窗口里的核心块 | Letta core memory、MemoryOS 短期层 |
| 情景记忆 Episodic | "那天发生了什么" | 带时间戳的事件流 | Generative Agents、claude-mem、Graphiti episodes |
| 语义记忆 Semantic | "事实与概念" | 抽取的事实 / 知识图谱 | Mem0、Cognee、Graphiti、Basic Memory |
| 程序性记忆 Procedural | "怎么做" | 技能、工作流、提示词 | Voyager 技能库、AWM、LangMem、OpenViking skills |
| 海马体索引 Hippocampus | 把散落的皮层痕迹串起来 | 图 + 个性化 PageRank | HippoRAG |
| 睡眠巩固 Consolidation | 把情景压成语义、清理噪音 | 离线 "sleep-time" 整理 | LightMem、Letta sleep-time agents、Hindsight reflect |
| 遗忘曲线 Forgetting | 不用就淡化，复习就加强 | 衰减 + 访问强化 | MemoryBank（艾宾浩斯）、MemoryOS heat |
| 再巩固 Reconsolidation | 每次回忆都会改写记忆 | 新信息到来时更新旧记忆 | A-MEM memory evolution、Graphiti 失效旧边 |
| 自传体 / 自我模型 | "我是谁、你是谁" | 用户画像 / 心智理论 | Honcho、Memobase |

完整解释见 [docs/brain-map.md](docs/brain-map.md)。

## 六大流派

| 流派 | 核心思路 | 强项 | 短板 | 代表 |
|---|---|---|---|---|
| ① 事实抽取 | 每轮让 LLM 抽"原子事实"，向量+BM25 检索 | 简单、省 token、易接入 | 丢上下文和因果，抽取错误会固化 | Mem0、LangMem、Memobase |
| ② 时序知识图谱 | 实体-关系图，每条边带有效期 | 处理"事实变化"、可审计 | 建图贵、慢，依赖 LLM 抽取质量 | Graphiti/Zep、Cognee、GraphRAG |
| ③ 记忆操作系统 | 像 OS 分页：上下文=内存，外存=磁盘 | 长期自主 agent、自我编辑 | 依赖模型自觉调用工具 | Letta/MemGPT、MemOS、MemoryOS |
| ④ 文件优先 | Markdown/Git 当唯一真相，索引是派生物 | 人可读、可改、可迁移、零锁定 | 规模大时检索要额外设计 | ai-memory、Basic Memory、claude-mem |
| ⑤ 类脑 / 认知科学 | 直接借鉴海马体、遗忘曲线、睡眠 | 关联回忆、多跳推理、自然遗忘 | 工程成熟度参差 | HippoRAG、A-MEM、LightMem、MIRIX |
| ⑥ 参数化 / 激活记忆 | 写进 KV-cache 或 LoRA 权重 | 速度快、"内化" | 难解释、难删除、成本高 | MemOS MemCube、Memory Layers |

详解与取舍 → [docs/architectures.md](docs/architectures.md)

## 开源系统榜单

> Star 数通过 GitHub API 读取：既有条目 **2026-11-12**，标 🆕 的新条目 **2026-11-13**（每周一统一刷新）。"Vic 点评"是我们的判断，不是厂商宣传。

| # | 系统 | ⭐ | 许可证 | 流派 | Vic 点评 |
|---|---|---|---|---|---|
| 1 | [claude-mem](https://github.com/thedotmack/claude-mem) | 116k | Apache-2.0 | ④ 编码 agent | 用钩子自动记录每次会话再压缩回注，编码场景开箱即用 |
| 2 | [Mem0](https://github.com/mem0ai/mem0) | 68.8k | Apache-2.0 | ① | 生态最广的记忆层；注意：OSS 版已移除图记忆，高分来自托管平台 |
| 3 | [Hindsight](https://github.com/vectorize-io/hindsight) | 60.5k | MIT | ①+⑤ | retain / recall / reflect 三件套，开源版就带巩固与时序 |
| 4 | [MemPalace](https://github.com/MemPalace/mempalace) | 60.5k | MIT | ⑤ | 记忆宫殿式组织原始对话，检索分数公开 |
| 5 | [OpenViking](https://github.com/volcengine/OpenViking) | 42.0k | AGPL-3.0 | ③+④ | 字节火山：记忆、知识、技能统一成文件系统式上下文库，L0/L1/L2 分级加载 |
| 6 | [GraphRAG](https://github.com/microsoft/graphrag) | 36.6k | MIT | ② | 适合"全局问题"，不是对话记忆，但社区摘要思路值得借 |
| 7 | [Cognee](https://github.com/topoteretes/cognee) | 33.3k | Apache-2.0 | ② | 图+向量一体，自托管友好，可做 MCP 后端 |
| 8 | [GBrain](https://github.com/garrytan/gbrain) 🆕 | 32.8k | MIT | ④ | Garry Tan 自用的生产级大脑：每条事实带来源、可更正可撤回，先无 key 关键词检索、再按需加语义与夜间"做梦"巩固；跨 agent 经 MCP 共享 |
| 9 | [Graphiti (Zep)](https://github.com/getzep/graphiti) | 32.8k | Apache-2.0 | ② | **双时序**是杀手锏：旧事实失效而不删除，最像人"记得以前是这样" |
| 10 | [Supermemory](https://github.com/supermemoryai/supermemory) | 31.6k | MIT | ① | 快、带连接器，可本地运行 |
| 11 | [TencentDB Agent Memory](https://github.com/TencentCloud/TencentDB-Agent-Memory) | 30.8k | 自定义 | ② | 团队级共享记忆中枢 |
| 12 | [agentmemory](https://github.com/rohitg00/agentmemory) | 30.3k | Apache-2.0 | ④ | 编码 agent 记忆，基线公开且自曝局限，态度难得 |
| 13 | [Letta (MemGPT)](https://github.com/letta-ai/letta) | 25.4k | Apache-2.0 | ③ | 记忆 OS 鼻祖：agent 自己编辑核心记忆块 |
| 14 | [Generative Agents](https://github.com/joonspk-research/generative_agents) | 22.4k | Apache-2.0 | ⑤ | 近因×重要性×相关性 + 反思，奠基之作 |
| 15 | [Memori](https://github.com/MemoriLabs/Memori) | 17.6k | 自定义 | ① | SQL 落地的记忆层，模型无关 |
| 16 | [ai-memory](https://github.com/akitaonrails/ai-memory) | 15.7k | MIT | ④ | Git+Markdown 为真相源，跨厂商交接，调研文档极好 |
| 17 | [MemOS](https://github.com/MemTensor/MemOS) | 12.4k | Apache-2.0 | ③+⑥ | MemCube 统一明文 / KV / 参数三种记忆 |
| 18 | [Honcho](https://github.com/plastic-labs/honcho) | 8.1k | AGPL-3.0 | ⑤ | 给每个用户建"心智模型"，偏自传体记忆 |
| 19 | [ByteRover CLI](https://github.com/campfirein/byterover-cli) | 4.9k | 自定义 | ④ | 编码 agent 可移植记忆层（原 Cipher） |
| 20 | [Basic Memory](https://github.com/basicmachines-co/basic-memory) | 4.6k | AGPL-3.0 | ④ | 本地 Markdown 知识图谱，经 MCP 读写 |
| 21 | [HippoRAG](https://github.com/OSU-NLP-Group/HippoRAG) | 4.1k | MIT | ⑤ | 海马体索引理论 + PPR，多跳关联回忆最强思路之一 |
| 22 | [MIRIX](https://github.com/Mirix-AI/MIRIX) | 3.5k | Apache-2.0 | ⑤ | 六类记忆各由一个 agent 管理 |
| 23 | [Memobase](https://github.com/memodb-io/memobase) | 3.0k | Apache-2.0 | ① | 用户画像为中心 |
| 24 | [Memary](https://github.com/kingjulio8238/Memary) | 2.7k | MIT | ② | 早期作品，2024 后不再更新 |
| 25 | [mcp-memory-service](https://github.com/doobidoo/mcp-memory-service) | 2.1k | Apache-2.0 | ② | MCP + REST，接 Claude / LangGraph / CrewAI |
| 26 | [LangMem](https://github.com/langchain-ai/langmem) | 1.8k | MIT | ① | LangGraph 原生，含程序性记忆（提示词自优化） |
| 27 | [MemoryOS](https://github.com/BAI-LAB/MemoryOS) | 1.6k | Apache-2.0 | ③ | 短/中/长三层 + 热度晋升，EMNLP 2025 Oral |
| 28 | [LightMem](https://github.com/zjunlp/LightMem) | 1.2k | MIT | ⑤ | 照搬 Atkinson-Shiffrin 三段式 + 睡眠期离线更新，ICLR 2026 |
| 29 | [A-MEM](https://github.com/agiresearch/A-mem) | 1.2k | MIT | ⑤ | 卡片盒笔记法：新记忆会反过来改写旧记忆，NeurIPS 2025 |
| 30 | [WikiTrace](https://github.com/yukyeongleee/wikitrace) 🆕 | 16 | Apache-2.0 | ⑤ | 经验记忆的"来源监控"：每条教训链接回原始轨迹，用之前先核对，减少"记错的教训害了 agent"（KAIST，arXiv:2611.14320） |
| 31 | [MCP Memory 参考实现](https://github.com/modelcontextprotocol/servers) | — | MIT | ② | 官方最小知识图谱记忆服务器，入门必看 |

**其他精选合集**：[IAAR-Shanghai/Awesome-AI-Memory](https://github.com/IAAR-Shanghai/Awesome-AI-Memory) · [TsinghuaC3I/Awesome-Memory-for-Agents](https://github.com/TsinghuaC3I/Awesome-Memory-for-Agents) · [nuster1128/LLM_Agent_Memory_Survey](https://github.com/nuster1128/LLM_Agent_Memory_Survey)

## 核心论文

**奠基与综述**
- [CoALA: Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427) (2023) — 把认知科学的工作/情景/语义/程序记忆框架带进 LLM agent
- [A Survey on the Memory Mechanism of LLM-based Agents](https://arxiv.org/abs/2404.13501) (2024)
- [Memory in the Age of AI Agents](https://arxiv.org/abs/2512.13564) (2025) — 最新综述

**系统与架构**
- [Generative Agents](https://arxiv.org/abs/2304.03442) (2023) — 记忆流 + 反思
- [MemoryBank](https://arxiv.org/abs/2305.10250) (2023) — 艾宾浩斯遗忘曲线
- [MemGPT: Towards LLMs as Operating Systems](https://arxiv.org/abs/2310.08560) (2023) — 虚拟上下文分页
- [HippoRAG](https://arxiv.org/abs/2405.14831) (2024) — 海马体索引理论
- [Zep: A Temporal Knowledge Graph Architecture](https://arxiv.org/abs/2501.13956) (2025) — 双时序图
- [A-MEM: Agentic Memory](https://arxiv.org/abs/2502.12110) (2025) — 卡片盒 + 记忆进化
- [From RAG to Memory (HippoRAG 2)](https://arxiv.org/abs/2502.14802) (2025) — 非参数持续学习
- [Mem0](https://arxiv.org/abs/2504.19413) (2025) — 生产级记忆层
- [Memory OS of AI Agent (MemoryOS)](https://arxiv.org/abs/2506.06326) (2025)
- [MemOS: A Memory OS for AI System](https://arxiv.org/abs/2507.03724) (2025)
- [MIRIX: Multi-Agent Memory System](https://arxiv.org/abs/2507.07957) (2025)
- [LightMem](https://arxiv.org/abs/2510.18866) (2025)

**程序性记忆 / 经验学习**
- [Reflexion](https://arxiv.org/abs/2303.11366) (2023) — 把失败教训写成语言记忆
- [Voyager](https://arxiv.org/abs/2305.16291) (2023) — 可复用技能库
- [Agent Workflow Memory](https://arxiv.org/abs/2409.07429) (2024) — 从经验中归纳工作流
- [Don't Just Trust Your Memory: Evidence-Linked Memory Reduces Memory-Induced Degradation in LLM Agents (WikiTrace)](https://arxiv.org/abs/2611.14320) (2026) — 教训必须能追溯到原始轨迹，否则记忆反而让 agent 变差

**认知科学原典（类脑设计的根）**：Atkinson & Shiffrin (1968) 多存储模型 · Baddeley & Hitch (1974) 工作记忆 · Tulving (1972) 情景/语义区分 · Ebbinghaus (1885) 遗忘曲线 · Collins & Loftus (1975) 扩散激活 · McClelland 等 (1995) 互补学习系统 · Nader 等 (2000) 记忆再巩固 · Anderson ACT-R 基础激活方程

## 评测基准

| 基准 | 测什么 | 链接 |
|---|---|---|
| LoCoMo | 超长多会话对话里的单跳/多跳/时间/开放问题 | [论文](https://arxiv.org/abs/2402.17753) · [repo](https://github.com/snap-research/locomo) |
| LongMemEval | 信息抽取、多会话推理、时间推理、知识更新、拒答 | [论文](https://arxiv.org/abs/2410.10813) · [repo](https://github.com/xiaowu0162/LongMemEval) |
| BEAM | 100K–10M token 级别的超长记忆 | 见各厂商公布 |
| agent-memory-bench | 统一流程比较多家系统"检索到了什么" | [repo](https://github.com/hamza-dev-tech/agent-memory-bench) |

⚠️ **看分数要小心**：厂商分数常用不同的回答模型、裁判模型和子集，不可直接比较。Mem0 自己的 2025 论文里，全量上下文（72.9）高于所有记忆系统；记忆系统真正的价值是用约 1/15 的 token 和更低延迟逼近它。详见 [docs/benchmarks.md](docs/benchmarks.md)。

## 怎么选

| 你的场景 | 推荐 |
|---|---|
| 给聊天机器人加"记住用户" | Mem0 / Memobase / Supermemory |
| 事实经常变化、需要审计 | Graphiti (Zep) |
| 长期自主运行的 agent | Letta / MemOS |
| 编码 agent 跨会话不失忆 | claude-mem / agentmemory / ai-memory |
| 文档 + 记忆一体、自托管 | Cognee / OpenViking |
| 想要人能读、能改、零锁定 | Basic Memory / ai-memory / GBrain / **vic-memory** |
| 研究类脑记忆 | HippoRAG / A-MEM / LightMem / MIRIX |

完整决策树 → [docs/selection-guide.md](docs/selection-guide.md)

## 贡献

欢迎 PR。收录标准：开源可用、近 12 个月有维护（经典研究除外）、能说清它在人脑框架中的位置。见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## License

列表内容 [CC BY 4.0](LICENSE)。各项目版权归原作者。

---
<div align="center">Made by <b>Vic</b> · 外贸 · 国际物流 · AI 一人公司 · 全球自媒体</div>

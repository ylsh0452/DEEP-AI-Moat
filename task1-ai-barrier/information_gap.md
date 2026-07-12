# AI 信息差观察

> **诚实说明**：以下 5 条信息是我在写本任务的过程中，借助 Claude Code 搜索发现的。在这之前，我的 AI 信息渠道几乎为零——偶尔刷到公众号文章，更多时候是遇到具体问题才去搜，不会主动关注行业动态。这些信息帮我意识到了一件事：**我在用的方法可能已经落后了。** 下面每条信息末尾加了"我的真实反应"，那是我看完之后的第一感觉。
>
> 后续目标：不再依赖 Claude 帮我搜，而是自己建立固定渠道和频率去关注这些信息。

---

## 信息一：DeepSeek 旧模型即将下线，新模型已支持 Function Calling 和专用文档 OCR

### 信息名称
DeepSeek API 2026 年模型换代：旧版 ID 7 月 24 日停用，V4 系列上线 Function Calling 和 DeepSeek-OCR-2

### 信息来源
- DeepSeek API 官方文档：https://api-docs.deepseek.com
- Abstract API 开发者指南：https://www.abstractapi.com/guides/other/deepseek-api-the-developers-guide
- 腾讯云开发者社区：https://cloud.tencent.com.cn/developer/article/2681904

### 这条信息说明了什么
DeepSeek 在 2026 年完成了从"对话模型"到"可编程工具平台"的关键转型。三件事同时发生：
1. **旧模型 ID（`deepseek-chat`、`deepseek-reasoner`）将于 2026 年 7 月 24 日下线**，必须迁移到 V4 系列 ID
2. **V4 系列支持标准 OpenAI Function Calling**，意味着可以把 DeepSeek 作为 Pipeline 中的一个 Tool-calling 节点来编程调用，而不只是手动聊天
3. **DeepSeek-OCR-2 发布**（2026 年 1 月），专门用于文档解析和视觉到文本提取，使用"类人阅读顺序"（不是简单的从上到下扫描），价格统一为 $0.15/1M tokens

### 它为什么重要
这条信息对我是**直接可操作的**。三件事都在影响我当前的工作方式：
- **旧模型下线**意味着我如果还在用旧版 API 调用方式，两周后代码就会报错——这是个硬 deadline
- **Function Calling 支持**意味着我可以把"调 DeepSeek 提取 PDF 数据"这一步自动化——不再需要每次手动开对话
- **DeepSeek-OCR-2 发布**意味着我之前"缺少 OCR 能力"的短板有了一个现成的解决方案，而且是我已经在用的平台

### 它可能适合什么人
- 所有用 DeepSeek API 做开发的个人和小团队
- 需要把 AI 嵌入自动化工作流而非手动聊天的用户
- 和我一样做文档处理、需要 OCR 能力的人

### 它对我有什么启发
**最大的启发是：我一直在手动用 DeepSeek，而它早就支持被程序化调用了。** 我每天的流程——打开 DeepSeek 网页版 → 粘贴 PDF 文本 → 描述需求 → 复制结果——如果改用 API + Function Calling，完全可以变成一条自动执行的命令。我之前没意识到这个差距，是因为我从来没去看过 API 文档。

### 它是否可能带来学习、比赛、项目或创业机会
- **项目机会**：基于 DeepSeek-OCR-2 + Function Calling 搭建文档自动提取 Pipeline，这正好是我壁垒路线图第二阶段的目标
- **比赛机会**：如果 DEEP 营后续有工具开发或自动化方向的任务，这套能力可以直接用上
- **创业机会**：文档处理工具的底层能力越来越便宜（$0.15/1M tokens），做垂直场景产品的成本在下降

### 是否存在风险或不确定性
- **供应商锁定风险**：深度绑定 DeepSeek API，如果后续涨价或服务不稳定，切换成本高
- **API 稳定性风险**：DeepSeek API 在国内的可用性和响应速度需要实测验证
- **模型质量风险**：OCR-2 在中文复杂排版（竖排、印章遮挡）上的实际表现需要自己用真实文档测试，不能只看官方宣传
- **迁移紧迫性**：7 月 24 日旧模型停用，如果之前的脚本或项目用了旧版 model ID，需要尽快改

### 我的真实反应
老实说，我一直用的是网页版 DeepSeek，根本不知道还有"旧 model ID"和"新 model ID"的区别，也没关注过 API 文档。Function Calling 这个词第一次听说。OCR-2 也是写这份文档时才知道 DeepSeek 还有专门的文档识别模型。**感觉就是：一直在用一个工具，但只用到了它最基础的功能，更高级的东西完全不知道。**

---

## 信息二：开源文档 AI 工具在 2026 年爆发——不需要从头造轮子

### 信息名称
2026 年开源文档解析工具生态大爆发：Docling、MinerU、pdf-mcp、ParseHawk、Kreuzberg/Xberg

### 信息来源
- Docling（IBM/LF AI & Data 基金会）：https://github.com/api-evangelist/docling
- MinerU v3.1.3：https://pypi.org/project/mineru/3.1.3/
- pdf-mcp（MCP Server for PDF）：https://github.com/jztan/pdf-mcp
- ParseHawk：https://github.com/parsehawk/parsehawk
- Xberg（原 Kreuzberg）：https://github.com/xberg-io/xberg

### 这条信息说明了什么
2026 年上半年，开源文档 AI 工具出现了一波集中爆发。关键趋势：
1. **从纯文本提取到结构化 JSON**：新一代工具（ParseHawk、lift）不再只是"提取文字"，而是直接输出符合 JSON Schema 的结构化数据
2. **VLM + OCR 双引擎成为标配**：MinerU 同时使用视觉语言模型和传统 OCR，在 OmniDocBench 上达到 95+ 分
3. **MCP Server 成为标准交付形式**：几乎所有新工具都提供 MCP Server，可以直接被 Claude Code 等 AI Agent 调用
4. **本地优先（Local-first）**：Docling、ParseHawk 都强调完全本地运行，不需要云 API
5. **许可证从 AGPL 向 Apache 2.0/MIT 迁移**：MinerU 从 AGPLv3 转为更宽松的许可证，降低了商用门槛

### 它为什么重要
**这条信息直接挑战了我当前的方案。** 我现在的技术栈是 `pdf-parse`（Node.js 纯文本提取）+ DeepSeek（手动对话做结构化）。而这个生态已经能提供：
- **pdf-mcp**：让 Claude Code 直接按关键词或语义搜索 PDF 的特定页面，只读需要的部分，不爆上下文
- **MinerU**：中文 PDF 的表格提取、公式识别、跨页表格合并，OmniDocBench 95+ 分
- **ParseHawk**：本地运行，定义 JSON Schema 后直接从 PDF 输出结构化数据

**我可能不需要从零搭建 Pipeline 了——市场上已经有了成熟的开源组件，我要做的是选型、集成和适配自己的场景，而不是重复造轮子。**

### 它可能适合什么人
- 所有做文档处理、RAG、知识库构建的开发者
- 需要从 PDF 中提取结构化数据的企业和团队
- 和我一样用 AI 处理文档但没有系统了解过开源工具的人

### 它对我有什么启发
**最大的启发是：我之前的信息封闭程度比我想象的更严重。** 我用了好几个月 `pdf-parse`，习惯了它"表格提取一塌糊涂"的局限性，以为"这就是行业现状"。但实际上，MinerU 的表格还原、pdf-mcp 的语义搜索、ParseHawk 的 Schema 约束提取——这些能力早就存在了，只是我不知道。

这也验证了我之前的一个判断：**信息差才是最大的差距。** 当别人用 MinerU 十分钟处理完一份复杂 PDF 的时候，我还在手写正则表达式清理 pdf-parse 的乱码输出。

### 它是否可能带来学习、比赛、项目或创业机会
- **学习机会**：这波工具的文档和论文本身就是很好的学习材料——可以了解 VLM + OCR 双引擎的架构设计
- **项目机会**：集成 MinerU 或 pdf-mcp 到我的 doc-pipeline 中，替换掉不稳定的 pdf-parse 环节——这直接加速了我的路线图第二阶段
- **比赛机会**：如果 DEEP 营有文档处理相关赛道，这套开源工具是很好的起点
- **创业机会**：在成熟开源组件之上做垂直场景的产品化封装（比如专门做合同提取、发票录入），技术门槛已经大幅降低了

### 是否存在风险或不确定性
- **选择困难**：工具太多，Docling vs MinerU vs ParseHawk vs Xberg，各自优劣需要实际测试才知道
- **中文支持差异**：MinerU 和 ZhDocParser 专门做了中文优化，其他工具的中文表现需要实测
- **维护风险**：部分工具还很新（ParseHawk v0.1.2），API 可能不稳定，依赖它做生产系统有风险
- **我的技术栈匹配度**：大部分工具是 Python 生态，而我的技术栈是 Node.js。需要评估是转 Python 还是找 Node.js 绑定

### 我的真实反应
看完这一堆工具名字确实有点懵。Docling、MinerU、ParseHawk、Xberg、pdf-mcp——每一个都说自己很强。但我现在连一个都没跑通过，不知道怎么选。**最大的感受不是"有好多好东西可以用"，而是"落后太多了不知道从哪里开始追"。** 接下来最重要的事不是全学，而是先挑一个（可能是 MinerU，因为它中文好；或者 pdf-mcp，因为它能和 Claude Code 配合），实际跑一遍再说。

---

## 信息三：Claude Code 发布 Dynamic Workflows——从单 Agent 到多 Agent 编排平台

### 信息名称
Claude Code 2026 年 5 月发布 Dynamic Workflows，支持自动生成多 Agent 编排脚本

### 信息来源
- Anthropic 官方博客：https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code
- SitePoint 评测：https://www.sitepoint.com/claude-code-june-2026-10-new-features-devs-need-to-know/
- 腾讯云开发者中文解读：https://cloud.tencent.com.cn/developer/article/2694452

### 这条信息说明了什么
Claude Code 从"单个 AI 编程助手"演变成了"多 Agent 编排平台"。核心变化：
1. **Dynamic Workflows**（2026 年 5 月）：Claude 可以自己写编排脚本，生成多个并行子 Agent，各自有独立上下文窗口，自动验证结果后合成最终答案
2. **六种编排模式**：分类-执行、扇出-合成、对抗验证、生成-过滤、锦标赛、循环直到完成
3. **2026 年 6 月的 10 项新功能**：3 层嵌套子 Agent、模型降级链、社区工具市场、成本追踪、权限控制、Agent 检查点等
4. **支持 `/loop` 调度**：可以用 cron 表达式安排定期 AI 任务

### 它为什么重要
**我正在用 Claude Code，而它的能力在过去两个月发生了质变。** 如果我只把它当成"对话式写代码工具"，我就错过了它最强大的功能：
- 可以用一个 Workflow 同时跑文档提取、数据校验、格式生成三个 Agent，并行处理
- 可以用对抗验证模式让一个 Agent 检查另一个 Agent 的提取结果是否正确
- 可以用 `pipeline()` 语法把"PDF → 提取 → 校验 → 生成 Word"整条链路写成自动化脚本

**这正好是我壁垒路线图第二阶段要解决的问题——而工具本身已经内置了解决方案，我之前不知道。**

### 它可能适合什么人
- 需要处理复杂多步骤任务的开发者（比如我的文档 Pipeline）
- 需要做大规模代码审查、测试生成、项目迁移的团队
- 任何从"单次 AI 对话"升级到"AI 自动化管线"的人

### 它对我有什么启发
**Claude Code 的 Dynamic Workflows 解决了我之前认为"需要自己从零搭建"的问题。** 我之前在壁垒路线图里写"第二阶段要搭建 doc-pipeline CLI"，设想的方案是自己写 Node.js 脚本来编排 pdf-parse → DeepSeek API → docx。但现在看来，Claude Code 的 Workflow 脚本语言（`agent()`, `pipeline()`, `parallel()`）本身就是一套编排 DSL，比我手写 Node.js 快得多。

**我的策略可能需要调整：不是"用 Node.js 从零搭建 Pipeline"，而是"用 Claude Code Workflows 快速验证 Pipeline 的可行性，稳定后再考虑独立部署"。**

### 它是否可能带来学习、比赛、项目或创业机会
- **学习机会**：Workflow 脚本的六种编排模式本身就是分布式系统设计模式的学习材料
- **项目机会**：用 Dynamic Workflows 快速搭建文档处理管线的 MVP，验证可行性后决定是否独立开发
- **比赛机会**：如果 DEEP 营有编程或自动化任务，Dynamic Workflows 可以显著提高执行效率

### 是否存在风险或不确定性
- **Token 消耗**：Dynamic Workflows 会消耗大量 token（并行 Agent 各自有独立上下文），成本需要关注
- **过度依赖风险**：用 Workflow 快速出 MVP 很方便，但如果后续要脱离 Claude Code 独立运行，需要重新实现编排逻辑
- **网络限制**：我在国内使用 Claude Code，API 连通性和响应速度是持续的不确定因素
- **学习曲线**：Workflow 脚本的语法（JavaScript）和编排模式的正确使用需要时间学习，用不好反而浪费时间

### 我的真实反应
Claude Code 我每天都在用，但主要是让它帮我写代码和操作文件。Dynamic Workflows 这个东西听起来很厉害——让它自己写脚本、生成多个 Agent 并行干活——但说实话，我现在连"让它帮我稳定完成一个任务"都还在摸索，这种高级功能暂时用不上。**先放一放，等把更基础的 API 调通和 OCR 搞定了再说。** 不能什么都想学，到头来什么都学不深。

---

## 信息四：MCP（Model Context Protocol）正在成为 AI Agent 与外部工具连接的标准

### 信息名称
MCP Server 生态 2026 年爆发式增长——文档处理、数据库、API 各类工具都开始提供 MCP 接口

### 信息来源
- pdf-mcp（PDF MCP Server）：https://github.com/jztan/pdf-mcp
- MCP Gateway 实践指南：https://futureagi.com/blog/using-mcp-gateway-with-claude-code-practical-guide-2026/
- DeepSeek MCP Server：https://www.npmjs.com/package/deepseek-mcp-server
- Xberg MCP Server（9 个工具）：https://github.com/xberg-io/xberg

### 这条信息说明了什么
MCP 不再只是一个概念，它在 2026 年已经变成了实际可用的基础设施：
1. **几乎所有新出的文档工具都自带 MCP Server**：pdf-mcp（语义搜索 PDF）、Xberg（9 个 MCP 工具处理 96+ 种文件格式）、Docling（docling-mcp）
2. **MCP 让 AI Agent 可以"手术式"读取文档**：比如 pdf-mcp 支持关键词 + 语义混合搜索，只读取需要的页面，而不是把整个 PDF 塞进上下文窗口
3. **MCP Gateway 出现**：可以统一管理多个 MCP Server，按团队、按环境（dev/staging/prod）做隔离

### 它为什么重要
**MCP 解决了我现在最大的痛点：AI 处理 PDF 时的上下文窗口限制。** 我现在的方式是把 pdf-parse 的全文输出粘贴给 AI——一份 50 页的 PDF 可能超过上下文限制，或者 token 消耗巨大。而 pdf-mcp 的思路是：AI 先用语义搜索找到相关的 3 页，只读那 3 页，精准处理。

这个模式把"文档处理"从"倒一桶水"变成了"拧开水龙头接一杯"——效率完全不在一个量级。

### 它可能适合什么人
- 所有用 Claude Code 或支持 MCP 的 AI 工具做开发的人
- 需要让 AI Agent 访问外部数据源（数据库、文件系统、API）的团队
- 和我一样做大量 PDF 处理、受困于上下文窗口的人

### 它对我有什么启发
**我之前完全不知道 MCP 是什么，更不知道 pdf-mcp 这个工具的存在。** 如果早点知道，很多 PDF 处理的效率问题可能已经有了更好的解决方案。这也再次确认：我的信息渠道太窄了。MCP 这个概念 2024 年底就有了，2026 年已经成熟，但我直到今天才真正去了解。

### 它是否可能带来学习、比赛、项目或创业机会
- **项目机会**：把 pdf-mcp 集成到我的文档处理管线中，替代"全文提取 → 粘贴给 AI"的粗暴模式
- **学习机会**：MCP 的协议设计（工具描述、资源暴露、安全模型）本身值得学习
- **创业机会**：在特定垂直领域开发专用的 MCP Server（比如"中国合同条款 MCP Server"），接入 AI Agent 生态

### 是否存在风险或不确定性
- **MCP 的长期标准性**：MCP 是 Anthropic 提出的，虽然开源且被广泛采用，但 OpenAI 有自己不同的方案。存在标准分裂的风险
- **安全风险**：MCP Server 可以执行本地操作，权限控制不当有安全隐患
- **网络限制**：MCP Server 的安装和更新在国内网络环境下可能遇到连通性问题

### 我的真实反应
MCP 这个概念我是这次写文档才第一次听说的。说实话，到现在我也没完全搞明白它具体怎么用——配置 MCP Server、连接 Claude Code、让它搜索 PDF……这些听起来都需要花时间折腾。**短期内不打算深入，先把 DeepSeek API 那一条线跑通再说。** 但这个方向我会记住——当我的 Pipeline 做到"需要 AI Agent 自己读 PDF"那一步时，再回来看 pdf-mcp。

---

## 信息五：模型成本持续下降——文档 AI 的"平民化"正在加速

### 信息名称
DeepSeek V4 系列定价仅为 OpenAI GPT-o 系列的 1/20 ~ 1/50，文档专用模型 OCR-2 统一定价 $0.15/1M tokens

### 信息来源
- DeepSeek API 官方定价页（2026 年 2 月更新）：https://api-docs.deepseek.com
- Abstract API 对比指南：https://www.abstractapi.com/guides/other/deepseek-api-the-developers-guide

### 这条信息说明了什么
AI 模型的使用成本在指数级下降。具体数据：
- DeepSeek V3.2：输入 $0.27/1M tokens（命中缓存 $0.07），输出 $1.10/1M tokens
- DeepSeek-OCR-2：输入/输出统一 $0.15/1M tokens
- 对比 OpenAI GPT-o 系列，DeepSeek 便宜 20-50 倍
- V4 系列支持 1M token 上下文窗口（可以一次性处理整本书）

**这意味着：用 AI 大批量处理文档的成本已经降到近乎可以忽略的程度。** 用 OCR-2 处理一份 100 页的 PDF，提取所有文字和表格，token 成本可能只有几美分。

### 它为什么重要
**成本是"个人做文档 AI 工具"的最大障碍之一——这个障碍正在消失。** 以前如果要批量处理几千份文档，API 费用可能是一笔不小的开销。现在用 DeepSeek 的价格，同样的预算可以处理二十倍到五十倍的文档量。

这意味着：个人和小团队也能负担得起大规模的 AI 文档处理。工具不再是大型公司的专利。

### 它可能适合什么人
- 独立开发者和个人项目
- 需要批量处理文档但预算有限的小团队
- 任何考虑"自己做 vs 买 SaaS"的文档处理场景

### 它对我有什么启发
**成本不再是我做文档 Pipeline 的障碍。** 我之前考虑过一个问题："如果我要批量处理几百份 PDF，DeepSeek API 的费用会不会太高？"现在有了明确答案：不会。用 OCR-2 的统一定价，几百份文档的成本在可接受范围内。我可以把更多精力放在"怎么做得更好"而不是"怎么省 token"上。

### 它是否可能带来学习、比赛、项目或创业机会
- **项目机会**：低成本意味着可以大量测试、快速迭代，试错成本很低
- **创业机会**：模型成本越低，垂直 SaaS 产品的毛利率越高，商业可行性越强
- **比赛机会**：不用担心 API 费用限制实验的规模和次数

### 是否存在风险或不确定性
- **价格可能变动**：DeepSeek 的价格策略可能调整，长期依赖时需要关注
- **免费午餐的终结**：目前的价格战是市场竞争的结果，未来可能不会一直这么低
- **质量与价格的 tradeoff**：便宜 20-50 倍的同时，DeepSeek 在部分任务上的质量是否与 OpenAI 持平，需要自己实测

### 我的真实反应
这条是最让我安心的。之前也想过"如果批量调 API 会不会很花钱"的问题，但一直没去查过具体价格。现在知道了——$0.15/1M tokens，处理几百份文档也花不了几个钱。**这个信息解决了一个实际的顾虑：不用担心试错成本太高，可以放心做实验。**

---

## 总结：我的信息雷达现状与改进

### 本次信息收集的体会
写这 5 条信息的过程中，我最大的感受是：**一旦开始主动搜索，信息量是爆炸性的。** 两个小时里我发现了 Docling、MinerU、pdf-mcp、ParseHawk、Xberg 五个直接相关的开源工具、理解了 MCP 是什么、知道了 DeepSeek Function Calling 怎么用、了解了 Claude Code Dynamic Workflows——这些都是我之前完全不知道的东西。

**问题从来不是"信息不够"，而是"我没有去找"。**

### 我的信息雷达改进计划

| 维度 | 之前 | 之后 |
|------|------|------|
| 信息频率 | 凭运气偶遇 | 每周至少主动搜索 1 次 |
| 信息渠道 | 公众号 + 实际操作中发现 | GitHub Trending + HN + 中文技术社区 |
| 信息筛选 | 看到什么是什么 | 按"是否和文档处理相关"过滤 |
| 信息消化 | 看了就过 | 按本模板记录，形成持续的信息日志 |
| 信息验证 | 相信作者说的一切 | 下载、实测、形成自己的判断 |

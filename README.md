# AI API 报错排查合集（2026）

OpenAI、Claude、Gemini、Qwen、DeepSeek API，以及 Claude Code、Cursor 等客户端在接入时的常见报错与配置问题，按厂商分组。每篇都写明报错原文、原因、修复步骤和验证方法，技术结论附官方文档出处。

完整文章在 [OpenLux API 实战指南](https://a37836323.github.io/openlux-api-guides/)，共 37 篇，本页每周同步一次。

## 客户端接入（2）

- [Codex 提示用量到上限怎么办：查剩余额度与重置时间、加购 credits、改用 API Key](https://a37836323.github.io/openlux-api-guides/news/codex-usage-limit-reached-fix/)：Codex 用 ChatGPT 登录时提示用量到上限的处理：用 codex login status 确认登录方式，在用量面板和 /status 查看剩余额度与重置时间，了解 5 小时窗口与每周上限，按套餐加购 credits，或用 codex login --with-api-key 改为按 API 价格计费，并按官方因素减少消耗。
- [Open WebUI 连不上 Ollama？Docker localhost 与 11434 排查](https://a37836323.github.io/openlux-api-guides/news/open-webui-ollama-docker-connection-refused/)：从实际发请求的容器检查 Ollama 地址、解析、监听和持久化连接配置。

## OpenAI（10）

- [OpenAI Responses API 间歇性报 503 怎么办：server_is_overloaded、服务端错误与重试设置](https://a37836323.github.io/openlux-api-guides/news/openai-responses-api-503-retry-fix/)：OpenAI Responses API 间歇性返回 503 的处理：按 error.code 区分 server_is_overloaded、其他服务端错误与 429 slow_down，遵守 Retry-After，用带抖动的指数退避并限制次数和总时长，避免 SDK 与应用层重试叠加，流式请求不自动重放，持续出现时查状态页并带请求 ID 联系支持。
- [OpenAI Decisions API 缓存没命中怎么办：questions 不进缓存、1024 token 门槛与 30m 限制](https://a37836323.github.io/openlux-api-guides/news/openai-decisions-api-prompt-cache-not-hit/)：OpenAI Decisions API 提示缓存不命中的排查：只缓存共享 input 前缀、questions 不进缓存也不计入 1024 token 门槛，固定规则放 input 开头加显式断点并用 explicit 模式，ttl 目前只有 30m，按 cached_tokens 与 cache_write_tokens 验证命中。
- [OpenAI 12 小时提示缓存怎么开：prompt_cache_options.ttl 报 400、类型报错与命中核对](https://a37836323.github.io/openlux-api-guides/news/openai-prompt-cache-12h-ttl-setup/)：OpenAI Responses API 新增 12 小时提示缓存：prompt_cache_options.ttl 设为 12h 报 400、Python SDK 类型报错、cached_tokens 不增加的排查与成本判断。
- [买了 ChatGPT Plus，API 为什么还报 429 insufficient_quota？](https://a37836323.github.io/openlux-api-guides/news/chatgpt-plus-api-insufficient-quota/)：买了 ChatGPT Plus，API 还报 429 insufficient_quota？按服务商、组织、余额和错误 code 排查。
- [OpenAI 报 Unsupported parameter: max_tokens 怎么改？两个接口别填错](https://a37836323.github.io/openlux-api-guides/news/openai-unsupported-max-tokens-fix/)：按实际端点区分 max_completion_tokens 与 max_output_tokens，排查旧模板和 Batch 请求体。
- [No tool output found for function call 怎么修？Responses API 回传排查](https://a37836323.github.io/openlux-api-guides/news/responses-no-tool-output-found-fix/)：检查 Responses 的 call_id、工具结果与响应链，定位多工具和流式中断后的缺失回传。
- [APIRemovedInV1 怎么修？openai.ChatCompletion.create 迁移示例](https://a37836323.github.io/openlux-api-guides/news/openai-python-apiremovedinv1-fix/)：识别旧 SDK 调用入口，检查解释器、依赖与返回对象，按当前客户端方法迁移。
- [OpenAI Python 报 unexpected keyword argument proxies 怎么解决？](https://a37836323.github.io/openlux-api-guides/news/openai-httpx-unexpected-proxies-fix/)：客户端还没发请求就报 proxies 参数错误？区分依赖不兼容与网络代理故障。
- [OpenAI 图片上传报 invalid_base64 或 Invalid image，怎么排查？](https://a37836323.github.io/openlux-api-guides/news/openai-invalid-base64-image-url-fix/)：区分 Base64 编码、图片格式和远端 URL 读取问题，按接口检查图片输入结构。
- [OpenAI API 报 CERTIFICATE_VERIFY_FAILED：证书链怎么修？](https://a37836323.github.io/openlux-api-guides/news/openai-python-certificate-verify-failed-fix/)：区分证书链、域名、系统时间与企业代理，配置可信 CA 并保持 HTTPS 校验。

## Claude（2）

- [Anthropic SDK 升级后报 400 或鉴权失败：API 版本 2026-10-21 迁移排查](https://a37836323.github.io/openlux-api-guides/news/anthropic-api-2026-10-21-upgrade-errors-fix/)：Anthropic SDK 升级到新大版本后默认使用 API 版本 2026-10-21：temperature 等参数 400、Bedrock 旧模型、网关 X-Api-Key 认证失败的排查与修复。
- [Claude Haiku 4.5 换成 Haiku 5.5 后报 400：budget_tokens、temperature、预填充与思维块怎么改](https://a37836323.github.io/openlux-api-guides/news/claude-haiku-5-5-migration-400-errors-fix/)：从 Claude Haiku 4.5 换到 Haiku 5.5 后报 400 的排查：budget_tokens 改自适应思考与 effort、删除 temperature/top_p/top_k、替换 assistant 预填充、计算机使用工具换版本、思维块回传保持只追加，以及空回答、max_tokens 截断和 token 变多的处理。

## Qwen（6）

- [Qwen 模型找不到怎么办：模型 ID、入口与账户权限排查](https://a37836323.github.io/openlux-api-guides/news/qwen-model-not-found-permission-check/)：按原始错误、精确模型 ID、实际入口与账户范围定位 Qwen 模型找不到问题。
- [Qwen 输出 JSON 仍然不能入库：字段、类型与事实的三层验收](https://a37836323.github.io/openlux-api-guides/news/qwen-json-output-business-validation/)：区分 JSON 语法、字段结构和业务事实错误，提供可运行的离线校验器。
- [Qwen 多轮对话越聊越长：历史裁剪、事实保留与上下文预算](https://a37836323.github.io/openlux-api-guides/news/qwen-chat-history-budget-and-facts/)：为多轮对话设置历史和输出预算，按完整轮次裁剪并保留事实来源。
- [Qwen 回答只出一半：finish_reason、输出上限与流式完整性排查](https://a37836323.github.io/openlux-api-guides/news/qwen-output-truncation-finish-reason/)：从 finish_reason、输出预算和流式事件判断回答截断原因与续写边界。
- [Qwen 3.8 Flash 怎么选：API 费用计算、验收样本与 Max 升级条件](https://a37836323.github.io/openlux-api-guides/news/qwen3-8-flash-api-cost-selection/)：用基础费用计算、业务验收样本和失败升级条件，判断 Qwen 3.8 Flash 与 Max 如何分工。
- [Qwen 缓存为什么没命中：从请求前缀、usage 到真实账单的排查流程](https://a37836323.github.io/openlux-api-guides/news/qwen-context-cache-hit-troubleshooting/)：排查 Qwen 缓存未命中、usage 字段缺失和账单未下降，提供最小对照实验与离线解析示例。

## 通用排查（17）

- [提示词改好了一题却弄坏其他题：版本管理与回归测试怎么做](https://a37836323.github.io/openlux-api-guides/news/prompt-version-regression-release/)：保存提示词版本并按同题对照找出收益与退步，用回归样本决定是否发布。
- [工具调用返回了参数却执行失败：函数名、字段和业务范围怎么校验](https://a37836323.github.io/openlux-api-guides/news/tool-call-arguments-validation-contract/)：以只读库存查询为例核对工具名称、参数结构、业务范围与结果对应关系。
- [API 用量和账单为什么对不上：时间窗口、计费单位与重试记录核对](https://a37836323.github.io/openlux-api-guides/news/api-usage-billing-reconciliation-workflow/)：按时间、账户、单位与重试记录核对 API 用量和实际账单，保留未知项。
- [Pinecone 报 Vector dimension does not match 怎么办？](https://a37836323.github.io/openlux-api-guides/news/pinecone-vector-dimension-mismatch-fix/)：向量入库或查询维度不匹配：核对实际向量长度、索引配置与模型，保留旧索引后迁移。
- [MCP 报 spawn npx ENOENT：终端能用，客户端为什么找不到？](https://a37836323.github.io/openlux-api-guides/news/mcp-spawn-npx-enoent-fix/)：从宿主进程 PATH、Node 路径和 Windows 启动方式排查 MCP 无法启动。
- [Your organization must be verified to stream：已验证为什么还报错？](https://a37836323.github.io/openlux-api-guides/news/openai-organization-verified-stream-error/)：核对发生流式报错的组织、项目、模型和访问路径，不把旧帖中的等级和等待时间当现行规则。
- [Invalid schema：additionalProperties 必须为 false，嵌套对象怎么改？](https://a37836323.github.io/openlux-api-guides/news/structured-outputs-additionalproperties-false-fix/)：按错误 context 找到嵌套 Schema，核对 additionalProperties、required 和可空字段。
- [Whisper API 报 413、音频超过 25MB：怎么压缩和分段？](https://a37836323.github.io/openlux-api-guides/news/whisper-audio-413-25mb-limit-fix/)：文件转写超过上传限制时，核对错误来源，压缩或按时间分段并验证每段可播放。
- [企业知识库答非所问怎么办：先测检索召回，再决定是否加 Rerank](https://a37836323.github.io/openlux-api-guides/news/rag-retrieval-rerank-evaluation/)：用标注题集区分知识库资料缺失、召回失败与排序问题，提供离线 Recall@K 示例。
- [一万条文本怎样批量交给 AI：任务编号、结果核对与失败补跑](https://a37836323.github.io/openlux-api-guides/news/batch-ai-jobs-result-reconciliation/)：设计批次任务编号、结果核对与失败补跑，避免按返回顺序误配业务记录。
- [AI 翻译商品页如何保持术语一致：术语表、数字与占位符检查](https://a37836323.github.io/openlux-api-guides/news/ai-product-translation-glossary-workflow/)：为商品页和帮助文档建立术语表、占位符检查与翻译验收流程。
- [图片转表格总有错字怎么办：OCR 输入、单元格对应与质量验收](https://a37836323.github.io/openlux-api-guides/news/image-ocr-table-extraction-quality/)：从图片准备、裁片关联到单元格对应，检查 OCR 转表格的关键字段质量。
- [会议录音转文字再生成纪要：怎样核对人名、数字和行动项](https://a37836323.github.io/openlux-api-guides/news/meeting-audio-transcription-action-items/)：分阶段核对会议转写、人名数字与纪要行动项，并保留原文证据。
- [OpenLux API 快速接入、401、429 与流式中断处理实战](https://a37836323.github.io/openlux-api-guides/quickstart/)
- [this organization has been disabled 错误分层排查指南](https://a37836323.github.io/openlux-api-guides/organization-disabled/)
- [API 429、流式中断与安全重试实战](https://a37836323.github.io/openlux-api-guides/rate-limit-stream-retry/)
- [用每个成功任务成本选择大模型](https://a37836323.github.io/openlux-api-guides/model-cost-per-success/)

## 关于

文章由 OpenLux 整理。需要用一个 Key 调用上面这些模型，可以[注册 OpenLux](https://api.openlux.ai/register?channel=c_tiu9nw5h&utm_source=github_index&utm_medium=owned_content&utm_campaign=ai-api-error-troubleshooting-2026&utm_content=footer)。

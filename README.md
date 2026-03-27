# AIS-SPEC - AI服务统一连接规范
**Universal AI Service Connection Standard • 对标JDBC/URI的AI通用连接标准**

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub Stars](https://img.shields.io/github/stars/AIS-SPEC/AIS-SPEC.svg?style=social)](https://github.com/AIS-SPEC/AIS-SPEC)
[![GitHub Forks](https://img.shields.io/github/forks/AIS-SPEC/AIS-SPEC.svg?style=social)](https://github.com/AIS-SPEC/AIS-SPEC/fork)
[![Compatibility Test](https://img.shields.io/badge/Test-Suite-Passing-green.svg)](./test-suite/)

---

## 📘 什么是AIS？
**AI Service Connection Specification（AIS）** 是一套**通用、安全、可扩展**的AI服务统一连接规范，对标JDBC/ODBC设计思想，通过标准化的连接字符串和统一API，解决异构AI服务的**配置沼泽**问题，实现**「一次编写，处处连接」**。

AIS独立于任何平台/语言，旨在成为AI工程化领域的**事实标准**，让开发者无需学习各AI平台的专属认证、接口、参数，仅通过一条连接串即可无缝对接所有AI服务。

---

## 🚀 核心价值
✅ **统一配置**：单连接串适配所有AI平台，告别多平台配置碎片化  
✅ **安全内置**：强制环境变量注入密钥，从源头规避硬编码泄露风险  
✅ **语言无关**：支持所有主流编程语言，统一API设计，学习成本为0  
✅ **可扩展**：SPI机制支持新平台/新签名/新能力，无需修改核心代码  
✅ **环境无感**：开发/测试/生产仅需更换连接串，业务代码一行不改  
✅ **生态兼容**：适配开源/商用/本地所有AI服务，无供应商锁定

---

## 📦 核心连接串格式
ais://<provider>[:<model>][@<endpoint>]?<param1>=<value1>&<param2>=<value2>&...
plaintext

### 示例1：OpenAI（官方接口）
ais://openai:gpt-4o?api_key=${OPENAI_API_KEY}&temperature=0.7&max_tokens=2048&stream=false
plaintext

### 示例2：通义千问（国内平台）
ais://qwen:qwen-max?api_key=${DASHSCOPE_KEY}&temperature=0.5&timeout=60
plaintext

### 示例3：Ollama（本地模型）
ais://ollama:llama3@http://localhost:11434?temperature=0.8&num_ctx=4096
plaintext

---

## 🚀 快速开始（Python示例）
### 1. 安装依赖
```bash
pip install ais-spec
2. 编写配置文件（ais.yaml）
yaml
env: dev
dev:
  openai: "ais://openai:gpt-4o?api_key=${DEV_OPENAI_KEY}&temperature=0.7"
  qwen: "ais://qwen:qwen-max?api_key=${DEV_DASHSCOPE_KEY}&temperature=0.5"
3. 核心代码（一行接入）
python
运行
from ais import AISFactory

# 从配置文件创建连接（支持YAML/JSON/INI/XML/Properties）
ais = AISFactory.create_from_config("ais.yaml", "openai")

# 1. 聊天（同步）
print(ais.chat("你好，介绍一下AIS规范"))

# 2. 流式输出（实时响应）
print("\n流式聊天结果：")
for content in ais.stream("一句话总结AIS的核心价值"):
    print(content, end="", flush=True)

# 3. 向量生成（文本转向量）
embedding = ais.embedding("AIS是AI服务统一连接规范")
print(f"\n向量长度：{len(embedding)}")

# 4. 工具调用（调用外部工具）
tool_result = ais.tool_call("北京今天的天气怎么样？")
print(f"\n工具调用结果：{tool_result}")
4. 多平台无感切换
业务代码无需修改，仅更换配置文件中的 provider：
python
运行
# 从OpenAI切换到通义千问（仅修改第二个参数）
ais_qwen = AISFactory.create_from_config("ais.yaml", "qwen")
print(ais_qwen.chat("你好，介绍一下AIS规范"))
📦 官方多语言实现
所有语言遵循统一目录结构 + 统一 API，一次学会，全语言通用：
Python - 最完善实现，推荐入门
Java - JVM 生态标杆
Go - 云原生 / CLI 首选
JavaScript/TypeScript - 前端 / 全栈
Rust - 安全 / 高性能
C# - .NET 生态
C++ - 底层 / 嵌入式
Lua - 轻量 / 网关
Ruby - 快速开发
PHP - Web 后端
Swift - Apple 生态（iOS/macOS）
Harmony - 鸿蒙生态（JS/C++）
🧩 支持的 AI 平台
海外商用：OpenAI、Anthropic Claude、Google Gemini
国内商用：阿里云通义千问、百度文心一言、腾讯混元、字节豆包、讯飞星火、Moonshot Kimi、智谱 GLM、DeepSeek
开源本地：Ollama、vLLM、Llama3、Mistral、Gemma
更多平台：持续扩展中，支持通过 SPI 自定义接入
📂 项目结构
plaintext
AIS-SPEC/
├── README.md               # 仓库首页（核心入口）
├── LICENSE                 # 开源协议（Apache-2.0）
├── CONTRIBUTING.md         # 贡献指南
├── CHANGELOG.md            # 版本更新日志
├── SECURITY.md             # 安全规范与漏洞上报
├── spec/                   # 🚨 核心：AIS官方规范本体（版本化）
├── examples/               # 📚 全场景示例（连接串/配置/用例）
├── implementations/        # 🔧 官方多语言实现（SDK）
├── schemas/                # 📋 标准校验Schema（JSON Schema）
├── tools/                  # 🛠 官方配套工具（CLI/校验器/解析器）
├── test-suite/             # 🧪 一致性测试套件（跨语言校验）
├── docs/                   # 📖 详细文档（静态站点）
└── assets/                 # 📸 静态资源（流程图/Logo）
📖 核心规范文档
最新规范 - 指向当前稳定版（v1.0.0）
v1.0.0 正式版 - 完整结构化规范（协议 / 参数 / 签名 / 安全）
规范路线图 - v1.1/v2.0 规划（多模态 / 分布式）
术语表 - 统一专业术语，避免歧义
🛠 生态工具
AIS CLI - 跨平台命令行工具（解析 / 校验 / 生成连接串）
配置校验器 - 自动化校验连接串 / 配置文件合法性
通用解析器 - 核心解析逻辑，多语言可直接引用
VSCode 插件（开发中） - 语法高亮 / 智能提示 / 实时校验
📚 示例中心
主流 AI 平台连接串 - 按厂商分类，直接复制使用
多格式配置文件 - YAML/JSON/INI/XML/Properties 完整示例
实际业务用例 - 聊天 / 流式 / 向量 / 工具调用场景化示例
多语言极简片段 - 一行接入，快速体验
🤝 贡献指南
AIS 是开源社区项目，欢迎所有形式的贡献：
规范贡献：修改 / 升级spec/目录下的官方规范
代码贡献：完善多语言实现 / 开发生态工具
文档贡献：补充示例 / 教程 / API 文档
问题反馈：提交 Issue，反馈 bug / 建议新功能
贡献流程详见：CONTRIBUTING.md
🔐 安全规范
AIS 核心安全设计：
禁止硬编码密钥，强制环境变量注入（${ENV_KEY}）
所有传输强制 HTTPS（TLS≥1.2）
日志自动脱敏敏感信息（api_key/secret_key）
生产环境推荐使用专业密钥管理服务（Vault/KMS）
安全漏洞上报：SECURITY.md
📄 许可证
本项目采用 Apache 2.0 开源许可证，可自由使用、修改、分发，无需授权，商用友好。详见：LICENSE
🧑‍💻 社区与支持
GitHub Issues - 问题反馈 / 功能请求
GitHub Discussions - 社区交流 / 技术讨论
贡献者列表：Contributors
💡 愿景
「一条连接串，对接所有 AI 服务」让 AI 集成变得简单、一致、通用，推动 AI 工程化生态标准化发展。

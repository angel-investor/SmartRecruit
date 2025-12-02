# SmartRecruit - 智能简历推荐系统

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Streamlit](https://img.shields. io/badge/Streamlit-1.45+-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

## 📋 项目介绍

**SmartRecruit** 是一款基于 RAG（检索增强生成）技术构建的**智能简历推荐系统**。该系统旨在解决企业 HR 和招聘官面临的海量简历筛选挑战，通过高效的检索和生成技术，从本地简历库中快速检索并推荐最匹配的候选人。

### 核心痛点解决
- ❌ 传统手动阅读：耗时费力，效率低下
- ❌ 容易遗漏：可能错失优秀候选人
- ✅ **智能推荐**：秒级响应，精准匹配

## 🎯 项目特性

### 1. **多模态简历处理**
- 支持 PDF、DOCX、TXT、JPG、PNG 等多种格式
- 自动提取文本内容
- 智能结构化信息解析（姓名、技能、经验等）

### 2. **高精度混合检索**
- **稠密检索**：BGE-M3 向量模型捕捉语义相似度（Milvus）
- **稀疏检索**：Elasticsearch BM25 关键词匹配
- **智能重排序**：CrossEncoder 重排序，确保推荐质量（阈值 > 0.4）

### 3. **智能对话 Agent**
- 基于 LangChain 和 LangGraph 构建
- 多意图识别：招聘需求、条件修正、后续追问、闲聊等
- 支持多轮对话，上下文记忆

### 4. **用户友好的 Web 界面**
- 基于 Streamlit 构建
- 支持简历上传与在线查看
- 实时反馈与处理日志

### 5. **系统评估模块**
- 集成 Ragas 评估框架
- 验证系统性能指标（忠实度、相关性等）
- 确保推荐质量

## 🏗️ 技术架构

```
┌─────────────────────────────────────────────────────────────┐
│                        Streamlit Web UI                      │
│              (上传、查询、结果展示、在线查看)                 │
└────────────────────┬────────────────────────────────────────┘
                     │
        ┌────────────┴────────────┐
        │                         │
┌───────▼────────┐      ┌────────▼──────────┐
│  LangChain     │      │  Document         │
│  LangGraph     │      │  Processing       │
│  Agent         │      │  & Parser         │
└───────┬────────┘      └────────┬──────────┘
        │                        │
    ┌───┴────────────────────────┴──────┐
    │      Hybrid Retrieval System      │
    │                                   │
┌───▼──────────┐      ┌───────────────┐ │
│   Milvus     │      │ Elasticsearch │ │
│   (向量检索)  │      │  (关键词检索) │ │
└───┬──────────┘      └───────────┬───┘ │
    │                             │     │
    └──────────┬──────────────────┘     │
               │ CrossEncoder 重排序    │
    ┌──────────▼──────────────────────┐ │
    │  BGE-M3 Embedding Model         │ │
    │  BGE-Reranker Model             │ │
    └────────────────────────────────┘ │
└───────────────────────────────────────┘
        │
   ┌────┴─────────────────────────┐
   │                              │
┌──▼─────────┐  ┌───────────────┐│
│  MongoDB   │  │  Milvus +     ││
│ (元数据)    │  │ Elasticsearch ││
│            │  │ (向量+关键词)  ││
└────────────┘  └───────────────┘│
                                 │
        ┌────────────────────────┘
        │
   ┌────▼──────────┐
   │ OpenAI Qwen   │
   │ (生成推荐)    │
   └───────────────┘
```

## 📊 项目目标

| 目标 | 描述 |
|-----|------|
| 🏢 **基础架构** | 支持简历上传、存储、检索和生成推荐 |
| 📄 **多模态处理** | 支持多种格式简历自动化处理与统一存储 |
| 🔍 **混合检索** | 结合密集向量和稀疏向量的高精度检索 |
| 💬 **智能对话** | 上下文理解的多意图识别智能 Agent |
| 📈 **性能评估** | 集成 Ragas 模块验证系统性能 |

## 🚀 快速开始

### 环境要求
- Python 3.8+
- MongoDB
- Milvus
- Elasticsearch

### 安装步骤

1. **克隆项目**
```bash
git clone https://github.com/angel-investor/SmartRecruit.git
cd SmartRecruit
```

2.  **安装依赖**
```bash
pip install -r requirements.txt
```

3. **配置环境**
编辑 `config.py`，设置以下配置项：
```python
# MongoDB 配置
MONGO_HOST = "127.0.0.1"
MONGO_PORT = 27017

# Milvus 配置
MILVUS_HOST = "127.0.0.1"
MILVUS_PORT = "19530"

# Elasticsearch 配置
ES_HOST = "http://127.0.0.1:9200"

# API 密钥
DASHSCOPE_API_KEY = "your_dashscope_api_key"

# 模型路径
MODEL_PATH = "/path/to/models"
EMBEDDING_MODEL = "/path/to/bge-m3"
RERANKER_MODEL = "/path/to/bge-reranker-base"
```

4. **初始化系统数据**
```bash
python system_data_init.py
```

5. **启动应用**
```bash
streamlit run app.py
```

应用将运行在 `http://localhost:8501`

## 📁 项目结构

```
SmartRecruit/
├── app.py                      # Streamlit Web 应用主入口
├── config.py                   # 系统配置文件（数据库、模型、API等）
├── requirements.txt            # Python 依赖列表
├── system_data_init.py         # 系统初始化脚本
│
├── rag/                        # RAG 核心模块
│   └── rag_pipeline.py         # SmartRecruitAgent 实现
│
├── utils/                      # 工具函数模块
│   ├── document_processor.py   # 文档处理与解析
│   └── vector_store.py         # 向量存储和检索
│
├── data/                       # 数据存储目录
│   └── resume/                 # 上传的简历存储位置
│
├── models/                     # 本地模型存储
│   ├── bge-m3/                 # 向量化模型
│   └── bge-reranker-base/      # 重排序模型
│
├── logs/                       # 日志输出目录
├── eval/                       # 评估模块（Ragas）
├── tests/                      # 测试代码
├── assets/                     # 资源文件
└── . streamlit/                 # Streamlit 配置
```

## 💡 核心功能演示

### 1. 简历上传
```
用户上传简历 → 文本提取 → 结构化解析 → 向量化 → 数据库存储
     ↓           ↓           ↓           ↓         ↓
   PDF/DOCX   OpenAI Qwen  信息提取   BGE-M3  MongoDB
                                               +Milvus
                                               +ES
```

### 2. 招聘查询与推荐
```
用户输入：" 需要 Web 前端开发工程师"
    ↓
LangGraph Agent 意图识别 → 招聘需求
    ↓
混合检索：
  • Milvus 向量搜索（语义相似）
  • Elasticsearch BM25（关键词匹配）
    ↓
CrossEncoder 重排序 → 质量过滤
    ↓
OpenAI Qwen 生成推荐理由 → JSON 结构
    ↓
Streamlit 渲染推荐卡片
```

### 3. 意图识别示例
- **"你好"** → chit_chat（闲聊）
- **"需要 Java 工程师"** → recruitment（招聘需求）
- **"只要 5 年经验的"** → condition_modification（条件修正）
- **"再看看其他候选人"** → follow_up（后续追问）

## 🔧 主要依赖

| 依赖 | 版本 | 用途 |
|-----|------|------|
| streamlit | 1.45+ | Web UI 框架 |
| langchain | 0.3. 0 | LLM 编排框架 |
| langgraph | 0.4.8 | Agent 构建 |
| openai | 1.68+ | LLM 调用 |
| pymilvus | 2.5.2 | 向量数据库 |
| elasticsearch | 8.14. 0 | 搜索引擎 |
| pymongo | 4.13.2 | 文档数据库 |
| sentence-transformers | 5.0.0 | 向量模型 |
| loguru | 0.7.3 | 日志记录 |
| pydantic | 2.11.7 | 配置验证 |

## 📖 使用示例

### Web 界面操作
1. **上传简历**：切换至"简历上传"标签页，选择文件上传
2. **查询推荐**：在"候选人推荐"标签页输入招聘需求
3. **查看简历**：点击推荐卡片中的"点击在线查看简历"链接

### Python API 调用
```python
from rag.rag_pipeline import SmartRecruitAgent
import asyncio

# 初始化 Agent
agent = SmartRecruitAgent()

# 执行异步查询
response = asyncio.run(agent.arun("需要 Python 开发工程师"))

# 输出结构：
# {
#   "response": "根据您的需求，我推荐以下候选人：.. .",
#   "candidates": [
#     {
#       "doc_hash": "xxx",
#       "file_path": "resume.pdf",
#       "reason": "该候选人有 5 年 Python 经验，精通 Django 框架..."
#     }
#   ]
# }
```

## 🎓 系统优势

- **⚡ 高效检索**：并行查询 Milvus 和 Elasticsearch，秒级响应
- **🎯 精准推荐**：CrossEncoder 重排序 + 阈值过滤（>0.4）
- **🔄 可扩展性**：模块化设计，易于集成新数据库或模型
- **📝 完整日志**：Loguru 记录所有操作细节，便于调试
- **✅ 配置验证**：Pydantic 自动验证配置，避免运行时错误

## 📋 开发计划

- [ ] 支持简历导出与批量处理
- [ ] 集成更多 LLM 模型（Llama、Claude 等）
- [ ] 添加简历匹配度评分可视化
- [ ] 支持 PostgreSQL 作为替代数据库
- [ ] 开发 API 服务（FastAPI）
- [ ] 完整的单元测试覆盖

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📝 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

## 📞 联系方式

- **作者**：angel-investor
- **Email**：contact@smartrecruit.com
- **GitHub Issues**：[提交问题](https://github.com/angel-investor/SmartRecruit/issues)

## 🙏 致谢

感谢以下开源项目的支持：
- [LangChain](https://github.com/langchain-ai/langchain)
- [Streamlit](https://github. com/streamlit/streamlit)
- [Milvus](https://github. com/milvus-io/milvus)
- [Elasticsearch](https://www.elastic.co/elasticsearch/)
- [BGE Embedding Model](https://huggingface.co/BAAI/bge-m3)

---

**最后更新**：2025-12-02

**项目状态**：🚀 活跃开发中

如有任何问题或建议，欢迎通过 GitHub Issues 与我们联系！
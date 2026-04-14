<div align="center">

<img src="./static/image/weaveragent_logo.svg" alt="WeaverAgent Logo" width="75%"/>

基于 GraphRAG 的学术知识图谱分析引擎
</br>
<em>A GraphRAG-Powered Academic Knowledge Graph Analysis Engine</em>

[English](./README-EN.md) | [中文文档](./README.md)

</div>

## ⚡ 项目概述

**WeaverAgent** 是一款基于 GraphRAG 技术的学术知识图谱分析引擎。通过上传 PDF 论文，系统自动提取实体与关系，构建学术知识图谱（涵盖方法、创新点、数据集、指标等核心维度），并借助 GraphRAG 多跳推理能力，生成深度技术路径分析报告，支持与图谱实体的自然语言交互问答。

> 你只需：上传一批 PDF 论文，用自然语言描述你的分析需求</br>
> WeaverAgent 将返回：结构化的学术知识图谱、技术路径分析报告，以及可深度交互的问答系统

### 核心能力

- **自动本体设计**：LLM 分析论文内容，自动生成 8 类核心实体类型（Paper、Method、Innovation、Task、Dataset、Metric、Baseline、Author）及其关系
- **GraphRAG 构建**：基于 Zep Cloud 构建高质量知识图谱，支持多跳关系推理
- **深度分析报告**：ReportAgent 自主检索图谱、反思推理，生成结构化技术路径分析报告
- **交互式问答**：基于图谱与向量检索的混合 RAG，精准回答技术溯源问题

## 🔄 工作流程

1. **图谱构建**：PDF 解析 → 文本提取 → LLM 本体生成 → Zep GraphRAG 构建
2. **环境搭建**：实体关系可视化 → 图谱统计信息 → 分析参数配置
3. **图谱分析**：节点/边类型分布 → 枢纽节点识别 → 技术路径分析
4. **报告生成**：ReportAgent 多轮检索图谱 → ReACT 推理 → 分章节生成报告
5. **深度互动**：与 ReportAgent 对话 → 图谱实体问答 → RAG 段落检索

## 🚀 快速开始

### 一、源码部署（推荐）

#### 前置要求

| 工具 | 版本要求 | 说明 | 安装检查 |
|------|---------|------|---------|
| **Node.js** | 18+ | 前端运行环境，包含 npm | `node -v` |
| **Python** | ≥3.11, ≤3.12 | 后端运行环境 | `python --version` |
| **uv** | 最新版 | Python 包管理器 | `uv --version` |

#### 1. 配置环境变量

```bash
# 复制示例配置文件
cp .env.example .env

# 编辑 .env 文件，填入必要的 API 密钥
```

**必需的环境变量：**

```env
# LLM API配置（支持 OpenAI SDK 格式的任意 LLM API）
# 推荐使用阿里百炼平台 qwen-plus 模型：https://bailian.console.aliyun.com/
LLM_API_KEY=your_api_key
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus

# Zep Cloud 配置（GraphRAG 构建必需）
# 每月免费额度即可支撑简单使用：https://app.getzep.com/
ZEP_API_KEY=your_zep_api_key
```

**可选环境变量：**

```env
# Neo4j 配置（本地图数据库，用于高级检索场景）
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=your_password

# Report Agent 配置
REPORT_AGENT_MAX_TOOL_CALLS=5
REPORT_AGENT_MAX_REFLECTION_ROUNDS=2
REPORT_AGENT_TEMPERATURE=0.5
```

#### 2. 安装依赖

```bash
# 一键安装所有依赖（根目录 + 前端 + 后端）
npm run setup:all
```

或者分步安装：

```bash
# 安装 Node 依赖（根目录 + 前端）
npm run setup

# 安装 Python 依赖（后端，自动创建虚拟环境）
npm run setup:backend
```

#### 3. 启动服务

```bash
# 同时启动前后端（在项目根目录执行）
npm run dev
```

**服务地址：**
- 前端：`http://localhost:3000`
- 后端 API：`http://localhost:5001`

**单独启动：**

```bash
npm run backend   # 仅启动后端
npm run frontend  # 仅启动前端
```

**Shell 脚本启动：**

```bash
./start.sh   # 一键启动
./stop.sh    # 一键停止
```

### 二、Docker 部署

```bash
# 1. 配置环境变量（同源码部署）
cp .env.example .env

# 2. 拉取镜像并启动
docker compose up -d
```

默认会读取根目录下的 `.env`，并映射端口 `3000（前端）/5001（后端）`

> 在 `docker-compose.yml` 中已通过注释提供加速镜像地址，可按需替换

## 🏗️ 技术架构

| 层级 | 技术栈 |
|------|--------|
| 前端 | Vue 3 + Vue Router 4 + Vite + D3.js（图谱可视化） |
| 后端 | Flask 3 + Flask-CORS |
| 图谱引擎 | Zep Cloud（GraphRAG）+ Neo4j（可选本地图数据库） |
| LLM | OpenAI SDK 格式（兼容 Qwen、GPT、Claude 等） |
| 向量检索 | ChromaDB（RAG 段落检索） |
| 文件处理 | PyMuPDF（PDF 解析）+ charset-normalizer |

## 📄 致谢

**WeaverAgent 感谢graphiti和mirofish的开源！**

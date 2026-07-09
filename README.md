# langchain1.2_tutorial 增强版 README（新增Agent开发完整学习路线+实操步骤）
仓库地址：https://github.com/sevan017/langchain1.2_tutorial
基于原LangChain 1.2教程拓展，补充自定义Agent开发全流程、分层学习内容、完整实操步骤，可直接替换仓库原有README.md

```markdown
# LangChain 1.2 官方实战教程 + 自定义Agent开发拓展
Repo：https://github.com/sevan017/langchain1.2_tutorial

## 一、项目基础信息
### 1. 项目简介
本仓库基于 **LangChain 1.2** 原版教程搭建，覆盖LLM基础调用、提示词工程、RAG、链式调用原生案例；
在此基础上新增完整 **Agent 智能体开发拓展模块**，支持工具调用、多步骤规划、记忆持久化、自定义工具、多Agent协作，适合从入门到落地企业级智能代理。

### 2. 技术栈
- 核心框架：LangChain 1.2（新版LCEL语法、Runnable架构）
- LLM兼容：OpenAI / 通义千问 / 文心一言 / Llama3 / Ollama本地大模型
- 向量库：Chroma、FAISS
- 记忆组件：ConversationBufferMemory、SummaryMemory、Redis持久记忆
- Agent模块：LangGraph 状态智能体、OpenAI Tools Agent、ReAct、Plan-Solve Agent
- 依赖：Python 3.10+、pydantic2、langchain-openai、langgraph、chromadb、python-dotenv

### 3. 目录结构（原仓库 + 新增Agent拓展目录）
```
langchain1.2_tutorial/
├── base_demo/              # 原版基础教程
│   ├── llm_call.py         # 基础大模型调用
│   ├── prompt_template.py  # 提示词工程
│   ├── chain_lcel.py       # LCEL链式调用
│   └── simple_rag/        # 基础检索增强生成
├── agent_dev/              # 【新增】Agent全流程开发模块
│   ├── 01_basic_tools/     # 工具定义、工具加载基础
│   ├── 02_react_agent/     # ReAct推理智能体
│   ├── 03_openai_tools_agent/ # OpenAI原生工具调用Agent
│   ├── 04_langgraph_agent/ # LangGraph多状态复杂Agent（核心拓展）
│   ├── 05_multi_agent/     # 多智能体协作、分工调度
│   ├── 06_memory_agent/    # 带长期记忆的对话Agent
│   └── custom_tools/       # 自定义业务工具（数据库查询、接口调用、文件读取）
├── utils/                  # 通用封装：向量库、LLM初始化、配置读取
├── .env.example            # 密钥、模型、向量库配置模板
├── requirements.txt        # 完整依赖（含Agent拓展依赖）
└── README.md               # 本学习文档
```

### 4. 环境安装
#### 4.1 克隆仓库
```bash
git clone https://github.com/sevan017/langchain1.2_tutorial
cd langchain1.2_tutorial
```

#### 4.2 安装依赖
```bash
# 基础+Agent全套依赖
pip install -r requirements.txt
```
> 若缺少LangGraph Agent依赖，补充安装：
> `pip install langgraph langgraph-checkpoint-sqlite`

#### 4.3 环境配置
复制环境模板，填入大模型API密钥：
```bash
cp .env.example .env
```
.env核心配置示例：
```env
# LLM配置
OPENAI_API_KEY=xxx
BASE_URL=https://api.openai.com/v1
MODEL_NAME=gpt-3.5-turbo

# 向量库
CHROMA_PERSIST_PATH=./chroma_db

# Agent记忆持久化
SQLITE_MEMORY_PATH=./agent_memory.db
```

## 二、分层学习路线（由浅入深，原版教程 → Agent开发）
### 阶段1：夯实LangChain1.2基础（原仓库教程）
目标：掌握底层组件，为Agent开发铺垫
1. LLM基础调用、同步/异步流式输出
2. Prompt模板、Few-shot提示、结构化输出Pydantic解析
3. LCEL Runnable链式语法、管道组合、分支路由
4. 基础RAG全链路：文档加载→分割→向量化→检索→问答链
**学习目录：`base_demo/`**

### 阶段2：Agent前置核心知识（必须掌握）
1. 工具（Tool）概念：函数封装、参数自动解析
2. ReAct推理框架：思考→行动→观察循环逻辑
3. 大模型Function Calling原生能力原理
4. 对话记忆类型：短期缓冲、摘要记忆、持久化存储
5. LangGraph核心：状态管理、节点、边、循环调度机制

### 阶段3：Agent实战开发（本项目新增拓展内容）
#### Level 1 基础工具与极简Agent
- 内置工具使用：计算器、搜索引擎、文件读写工具
- 自定义业务工具开发（封装数据库查询、本地文件解析工具）
- 手动实现简易ReAct循环，理解Agent运行底层逻辑

#### Level 2 开箱即用标准Agent
- OpenAI Tools Agent：基于Function Calling的标准工具智能体
- 带记忆对话Agent，支持多轮上下文连续工具调用
- Agent输出结构化解析，过滤无效工具调用

#### Level 3 LangGraph 复杂状态Agent（重点拓展）
- 多节点拆分：规划节点、工具执行节点、反思节点、回答节点
- 循环判断：工具调用次数限制、终止条件判断
- 状态持久化：SQLite存储对话历史，重启不丢失上下文
- 分支路由：根据模型输出动态选择执行工具

#### Level 4 高阶多Agent协作系统
- 角色拆分：规划Agent、工具执行Agent、总结Agent、校验Agent
- Agent之间消息传递、任务分发
- 冲突处理、结果汇总、多轮自我修正

### 阶段4：Agent落地优化与生产调优
1. Agent幻觉抑制：增加工具结果校验、反思节点
2. 工具调用限流、超时捕获、异常重试封装
3. 向量检索+Agent混合RAG智能问答
4. 本地Ollama开源模型运行Agent（无API依赖）
5. Agent服务封装：FastAPI对外提供智能体接口

## 三、Agent开发分步实操教程（可直接运行）
### Step1：自定义业务工具（入门）
路径：`agent_dev/custom_tools/file_tool.py`
实现读取本地txt文档工具，供Agent自主调用
```python
from langchain.tools import tool

@tool
def read_local_file(file_path: str) -> str:
    """读取本地文本文件内容，当用户需要查询本地文档信息时调用
    Args:
        file_path: 文件本地绝对/相对路径
    """
    with open(file_path, "r", encoding="utf-8") as f:
        return f.read()
```

### Step2：基础ReAct Agent快速运行
路径：`agent_dev/02_react_agent/demo.py`
```python
from langchain_openai import ChatOpenAI
from langchain.agents import AgentExecutor, create_react_agent
from langchain_core.prompts import PromptTemplate
from custom_tools.file_tool import read_local_file
from utils.load_env import load_env

load_env()
llm = ChatOpenAI(model="gpt-3.5-turbo")
tools = [read_local_file]

prompt = PromptTemplate.from_template("""
Answer the following questions as best you can. You have access to the following tools:
{tools}
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question

Begin!
Question: {input}
Thought:{agent_scratchpad}
""")

agent = create_react_agent(llm, tools, prompt)
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

if __name__ == "__main__":
    res = agent_executor.invoke({"input": "读取demo.txt文件，总结里面内容"})
    print(res["output"])
```

### Step3：LangGraph 带记忆复杂Agent（核心拓展案例）
路径：`agent_dev/04_langgraph_agent/graph_agent.py`
核心能力：
1. 全局对话状态存储
2. 多次工具调用循环
3. 自动反思工具返回结果
4. 持久化对话记忆，支持断点续对话

### Step4：多Agent协作调度示例
路径：`agent_dev/05_multi_agent/multi_role_agent.py`
分工：
1. PlannerAgent：拆解用户复杂需求，生成执行步骤
2. ToolAgent：执行文件、检索、计算工具
3. ReviewAgent：校验工具输出，修正错误，汇总最终回答

## 四、Agent开发常见问题与优化方案
1. 频繁无效工具调用
   - 优化方案：增加输出校验节点、限制最大循环次数、优化Prompt约束工具使用场景
2. Agent幻觉，编造不存在文件/数据
   - 优化：工具返回空结果时强制模型承认无数据，新增反思校验节点
3. 多轮对话上下文过长，token超限
   - 优化：SummaryMemory摘要记忆，截断历史对话
4. 本地开源模型Agent工具调用不稳定
   - 优化：使用结构化输出Pydantic强制格式化工具参数，替换ReAct为原生Tools调用

## 五、拓展学习计划（进阶自主开发方向）
1. 封装Agent通用服务，提供HTTP接口
2. 对接数据库工具：SQL查询Agent，自主生成执行SQL
3. 网页搜索Agent：结合SerpAPI联网实时检索
4. 智能数据分析Agent：读取Excel、执行计算、生成图表描述
5. Agent人机交互前端：Gradio快速搭建对话页面

## 六、许可证
MIT License
欢迎基于本项目二次开发、拓展自定义Agent业务场景
```

# 使用说明
1. 将以上完整内容直接覆盖原仓库 `README.md`；
2. 在项目根目录新建 `agent_dev` 文件夹，按目录分层创建对应Agent代码demo；
3. 更新 `requirements.txt`，追加Agent相关依赖：
```txt
langgraph>=0.1
langgraph-checkpoint-sqlite
pydantic>=2.0
chromadb
python-dotenv
```
4. 新增 `.env.example` 配置模板，统一管理模型密钥；

# 补充配套工具脚本（可选）
如果你需要，我可以额外提供：
1. 一键生成项目目录树的python脚本；
2. LangGraph Agent完整可运行最小demo；
3. 多Agent协作完整可运行代码；
4. 自动读取仓库文件、更新README目录结构的生成脚本。

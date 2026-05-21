---
title: "从零搭建 AI Agent：原理、框架与实战"
description: "深入理解 AI Agent 的工作原理，使用 LangChain 和 OpenAI API 从零搭建一个具备记忆、规划和工具调用能力的智能代理。"
pubDate: 2026-05-12
tags: [AI, Agent, LLM, Python]
category: 人工智能
featured: true
---

## 什么是 AI Agent？

AI Agent 是一个能够**自主感知环境、做出决策并执行动作**的智能系统。与简单的对话模型不同，Agent 具备：

1. **记忆（Memory）**：记住历史交互和上下文
2. **规划（Planning）**：将复杂任务分解为可执行的步骤
3. **工具（Tools）**：调用外部 API 和工具完成实际操作
4. **反思（Reflection）**：评估执行结果并调整策略

## Agent 架构

```
用户输入 → 感知 → 规划 → 执行 → 观察 → 反思 → 响应
                ↑                        |
                └────── 记忆 ────────────┘
```

## ReAct 模式

最流行的 Agent 模式是 **ReAct（Reasoning + Acting）**：

```python
from langchain.agents import create_react_agent
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4", temperature=0)

# 定义工具
from langchain.tools import tool

@tool
def search_web(query: str) -> str:
    """搜索互联网获取信息"""
    # 实现搜索逻辑
    pass

@tool
def execute_code(code: str) -> str:
    """执行 Python 代码并返回结果"""
    # 实现代码执行逻辑
    pass

# 创建 Agent
agent = create_react_agent(
    llm=llm,
    tools=[search_web, execute_code],
    prompt=react_prompt
)
```

## 记忆系统

### 短期记忆

```python
from langchain.memory import ConversationBufferWindowMemory

memory = ConversationBufferWindowMemory(
    k=5,  # 保留最近 5 轮对话
    return_messages=True
)
```

### 长期记忆

```python
from langchain.memory import VectorStoreRetrieverMemory
from langchain_community.vectorstores import Chroma

vectorstore = Chroma(embedding_function=embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

memory = VectorStoreRetrieverMemory(retriever=retriever)
```

## 工具设计原则

1. **单一职责**：每个工具只做一件事
2. **描述清晰**：LLM 依赖工具描述来选择工具
3. **错误处理**：工具应该优雅地处理异常
4. **输入验证**：验证 LLM 生成的参数

## 实战注意事项

- 设置 **最大迭代次数**，防止 Agent 陷入死循环
- 关键操作需要**人工确认**（Human-in-the-loop）
- 使用**结构化输出**减少解析错误
- 添加**成本监控**，避免 API 调用失控

> AI Agent 是 LLM 的下一个演进方向，但请始终记住：Agent 是工具，不是替代品。

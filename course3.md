## 课程3

### RAG是什么

RAG是一种结合了检索和生成的技术，通过利用外部知识库来增强大语言模型的性能，通过检索与用户输入相关的信息片段，并结合这些信息来生成更准确、更丰富的回答。

### RAG工作原理
涉及到向量数据库，检索和生成三个部分
向量数据库：将文本通过预训练模型转化为固定长度的向量表示，通过相似度检索找出相关的向量
<img width="522" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/f9309259-b1f6-4362-8e54-2d8a83d13db1">

### RAG示例
问题是如何评价OpenAI CEO奥特曼突然被离职又火速回归的事件：
如果LLM直接给出回答：“我不知道”或者生成一些无关的数据，出现幻觉问题。
如果使用RAG是如何处理：
1. 用户问题进行向量化，在向量数据库进行检索，并找出最相关的top文本
2. 将用户的问题和相关文本一起输入给LLM，给出最终回答
<img width="438" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/451c995d-5a03-4d97-9e0a-c7f73d3cb26f">

### RAG发展进程
Meta在2020年提出，三种范式：
1. Naive RAG，问答系统，信息检索
2. Advanced RAG，摘要生成，内容推荐
3. Modular RAG，多模态任务，对话系统

### RAG常见优化方法
1. 嵌入优化：结合系数和密集检索，多任务
2. 索引优化：细粒度分割(Chunk)，元数据
3. （前检索）查询优化：查询扩展，转换，多查询  
4. （后检索）上下文管理：重排，上下文选择/压缩
retrieval方式
1. 迭代检索：根据初始查询和迄今为止生成的文本进行重复检索
2. 递归检索：迭代细化搜索查询，链式推理(chain-of-thought)指导检索过程
3. 自适应检索：Flare，self-RAG，使用LLMs主动决定检索的最佳时机和内容
大模型优化，LLM微调：
1. 检索微调
2. 生成微调
3. 双重微调

### RAG vs. 微调
<img width="555" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/5d7d46a5-3e19-4d59-a8a7-77f3865a9430">

### 茴香豆
是一个基于LLMs的领域知识助手，专为即时通讯工具中的群聊场景优化的工作流，通过应用检索增强生成RAG技术，茴香豆能够理解和高效准确的回应与特定知识领域相关颚复杂查询。
茴香豆由四个部分组成：
1. 知识库，txt，pdf，doc，markdown等格式
2. 前端，即时通讯软件
3. LLM后端：本地模型：书生浦语，通义千问，远端模型：chatGPT等
4. 茴香豆，打通流程

### 茴香豆工作流
<img width="465" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/5b6caafc-7058-41ec-a1df-9430c404cee6">

### 遇到的问题及解决
- 在创建向量数据库过程中执行python3 -m huixiangdou.service.feature_store --sample ./test_queries.json遇到报错：
<img width="1050" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/b667746f-9c01-4670-af3f-fee19b5b46f0">

- 解决方法：将huixiangdou/service/retriever.py中的context_max_length: int = 16000 修改为context_max_length: int = 128000

### 课程4

#### 为什么要做微调
通用模型具有普适性，一些领域内表现并不佳

#### 微调范式
- 增量预训练
  - 让记作模型学习到一些新知识，如某个垂类领域的常识，使用文章、书籍、代码等数据
- 指令跟随微调
  - 让模型学会对话模版，根据人类指令进行对话，使用高质量对话，问答数据
例子：
<img width="746" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/0bfc37c7-19f5-4905-bf33-1567873ba78c">

#### 数据
- 数据流程
<img width="779" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/a49b3703-aeda-41c3-9394-0d177d500bc3">

  - 标准格式数据
<img width="693" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/b006f63b-fb45-48c9-a6e9-7f362f37f087">

  - 对话模版
<img width="305" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/401d73be-a194-46f4-9a3d-6a87d3f8f339">

#### 微调方案
- LoRA & QLoRA
  
  - 如果付整个模型进行微调需要很大的显存开销，如果使用LoRA则不需要那么大的显存开销
<img width="263" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/94f229d5-8fd7-4d5d-8139-536cfa8f9fb7">

- FineTuning & LoRA & QLoRA比较
  - FineTuning需要加载全部的参数和优化器
  - LoRA需要加载模型参数，但是只需要加载部分优化器
  - QLoRA模型参数4bit量化的方法加载
  <img width="834" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/f2fb541c-4e0e-48c7-bdc8-630936a94fa5">

#### Xtuner
- 以配置文件的方式封装了大部分微调场景
- 对于7B的参数量的LLM，微调所用的最小现存仅为8GB
- Xtuner技术架构图
<img width="409" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/94a187ba-c404-44e5-98ec-9ebf66ae567a">
- Xtuner数据引擎
<img width="742" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/41406682-ae9e-4b94-8e1f-af3fe333cdb3">
  - 多数据样本拼接
- Xtuner两个重要优化技巧

  - Flash Attention
    attention计算并行化，默认开启
    <img width="401" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/8cb673d5-40d2-4885-8b86-05f103ef70e9">

  - DeepSpeedZeRO
    多GPU训练时节省显存
    <img width="413" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/ad2f2e90-2a27-4c9a-a820-a5793574ecac">
<img width="720" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/c8bf3b9f-7422-414d-9615-2d25f5c79d9c">

#### InternLLM2 1.8B模型

#### 多模型LLM
- 多模型LLM原理
  增加图片算法
<img width="668" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/c546ac3b-a62d-4e25-b21a-9279642884e3">

- LLaVA方案
  图片识别，而不是图片生成
<img width="677" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/b0b1e6a3-480c-4fc9-87eb-14330af62b59">


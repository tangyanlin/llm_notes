## 课程5

### 大模型部署面临的挑战
- 内存开销大，20B模型产生10.3Gkv缓存+40G参数现存=50G现存

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/29fac131-a838-4194-bd66-be071bab4646)

- 访存瓶颈，计算量和gpu计算性能，显存带宽
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/195f8336-e7dd-4b95-8145-ff7261519d59)

### 大模型部署方法

#### 模型剪枝
- 非结构化减枝
- 结构化减枝
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/50cda6d5-be24-4958-b052-aca5677bbf51)

#### 知识蒸馏
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/48e77df3-0596-47d4-a1cd-ec20e5a56a08)

#### 量化
浮点数转换为整数，减轻存储和计算负担，存储为整数，计算的时候还是会用浮点数，访存瓶颈远远大于计算瓶颈，减小访存是重点
- 量化感知训练(QAT)
- 量化感知微调(QAF)
- 训练后量化(PTQ)

### LMDeploy核心功能
- continuous batch推理模型，解决不等长的结果
- 缓存管理机制降低显存占用量
- 模型量化
- 快捷的服务化部署
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/77d9b1bc-0969-4a29-8e4e-7e5b4c6f71c4)


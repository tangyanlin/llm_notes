## 课程5作业

### 环境部署
注意选择conda12.2开发环境

### 使用Transformer库运行模型
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/f45fec0f-3af6-4b6d-ae82-65196c74f3c9)

### 使用LMDeploy与模型对话
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/7bccbedf-18c9-482a-8555-76c8285631e8)


## 进阶作业，进阶作业完成穿插在下面，以（进阶作业x）标记
### LMDeploy模型量化
- 运行命令
```
lmdeploy chat /root/internlm2-chat-1_8b
```
- 显存占用情况，7856M
  
 ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/91280966-0153-4953-b275-2a96944d7928)

- 运行命令
```
lmdeploy chat /root/internlm2-chat-1_8b --cache-max-entry-count 0.5
```
- 显存占用情况，6608M
  
 ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/33895a6c-5fb5-40df-8202-e2db15fc166a)


- 运行命令
```
lmdeploy chat /root/internlm2-chat-1_8b --cache-max-entry-count 0.01
```

- 显存占用情况,4560M

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/3bc16831-c1d6-43c2-a7e2-c67a943499ee)

（进阶作业一）设置KV Cache最大占用比例为0.4，开启W4A16量化，以命令行方式与模型对话
- 运行命令 
```
lmdeploy chat /root/internlm2-chat-1_8b --cache-max-entry-count 0.4
```

- 显存占用情况,6184M

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/d153601d-922d-4178-a6eb-7e27d49dae72)


### 使用W4A16量化
- 命令
```
lmdeploy lite auto_awq \
   /root/internlm2-chat-1_8b \
  --calib-dataset 'ptb' \
  --calib-samples 128 \
  --calib-seqlen 1024 \
  --w-bits 4 \
  --w-group-size 128 \
  --work-dir /root/internlm2-chat-1_8b-4bit
```
显存占用变为2472M。

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/80ce733f-26be-4c63-8596-91dd2d1c9d5c)


### LMDeploy服务
将大模型封装为API接口
 - 模型推理/服务。主要提供模型本身的推理，一般来说可以和具体业务解耦，专注模型推理本身性能的优化。可以以模块、API等多种方式提供。
 - API Server。中间协议层，把后端推理/服务通过HTTP，gRPC或其他形式的接口，供前端调用。
 - Client。可以理解为前端，与用户交互的地方。通过通过网页端/命令行去调用API接口，获取模型推理/服务
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/5a02df6d-1213-4fc7-a504-902a6a27ef51)

### 启动API服务器

（进阶作业二）
- 启动W4A16量化，调整KV Cache的占用比例为0.4启动服务
```
lmdeploy serve api_server \
    /root/internlm2-chat-1_8b-4bit \
    --model-format hf \
    --cache-max-entry-count 0.4 \
    --quant-policy 0 \
    --server-name 0.0.0.0 \
    --server-port 23333 \
    --tp 1
```

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/fc18c959-f3db-43ec-bd9e-0d4b20a408ba)

### 命令行客户端连接API服务器
- 使用的架构图
  
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/704a77fb-bfff-4ed1-875d-fdd70cc075ce)

- 命令行运行
  
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/e658d334-2337-4c35-94b8-b930243763f9)

### 网页客户端连接API服务器
- 使用的架构图
  
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/5436e739-6eba-4703-96b6-2911dd435af8)

- 界面截图

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/ce689424-8904-4d16-8932-4e6569757a75)

### Python代码集成
- Python代码集成运行1.8B模型
core dump问题，重跑就可以

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/29231fec-d61a-400b-ae04-ddff0f36ff50)
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/db08985a-8de1-47c2-820f-f687b76f3227)

- 向TurboMind后端传递参数
  
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/a4635d92-5042-4b43-a128-f3c437759bcf)

（进阶作业三）
- 使用W4A16量化，调整KV Cache的占用比例为0.4，使用Python代码集成的方式运行internlm2-chat-1.8b模型

  - 代码
```
from lmdeploy import pipeline, TurbomindEngineConfig

# 调低 k/v cache内存占比调整为总显存的 20%
backend_config = TurbomindEngineConfig(cache_max_entry_count=0.4)

pipe = pipeline('/root/internlm2-chat-1_8b-4bit',
                backend_config=backend_config)
response = pipe(['Hi, pls intro yourself', '上海是'])
print(response)
```
  - 截图

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/78529d37-7a8c-4d68-a935-e3227e7aec87)

### 使用LMDeploy运行视觉多模态大模型llava
（进阶作业四）

- 某一些线程可能会报错，在10% A100 GPU机器上跑不起来
  
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/d6da1057-ca57-4c8f-8aab-20bd5e594e27)


### 使用LMDeploy运行第三方大模型
- Transformer库推理速度

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/16ab7cb5-5fd6-4120-9136-3b9eb43f18a6)

- LLMDeploy推理速度，是Transformer库推理的25倍
  
  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/0ed22296-9ff2-4930-ab68-7a8c69e04d88)


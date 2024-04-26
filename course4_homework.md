## 课程4作业

### Xtuner原理
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/034ce351-351c-4b22-96fa-884250272c0d)

### 安装环境
等待了很长很长时间……

### 列出internlm2-1.8b 模型里支持的配置文件
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/68818105-6818-433a-ad51-3495b5e30132)

### 复制配置文件到指定位置
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/1e50bfa2-15c3-4125-936f-0ca090946ba8)

### 训练前模型结果
不能回答自己是个人助理
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/034a60c6-e70e-4f6f-9fa6-5dc2a58971f1)


### 常规训练
- 训练命令，耗时12:42:37-13:02:23，20分钟
```
xtuner train /root/ft/config/internlm2_1_8b_qlora_alpaca_e3_copy.py --work-dir /root/ft/train
```
- 训练过程：
  - 300轮
  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/9c4a2ddc-435e-4d27-9898-e6ad880ada28)
  - 600轮，过拟合，只会回答同一个答案了
  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/aae99498-ad91-4999-8528-8b77d8af0357)
  - 训练结束
  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/f073eae5-ef2a-4e4c-a1cd-9d56002ab88f)

- 训练的模型文件
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/32502313-8267-44c8-9400-60f46895cd96)

### deepspeed加速训练
- 训练命令，耗时13:08:56-13:20:36，12分钟
```
xtuner train /root/ft/config/internlm2_1_8b_qlora_alpaca_e3_copy.py --work-dir /root/ft/train_deepspeed --deepspeed deepspeed_zero2
```
- 训练过程：
  - 300轮
  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/7f82aedb-ae76-45e9-9682-6ef77055b1c0)

  - 600轮，过拟合
  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/cad93b4e-9a7f-43a5-a5ad-c91d78420553)


  - 训练结束，为什么deepspeed的memory要多于常规训练
  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/066bb4fa-5fa9-4108-bd8d-3e433af29af6)


- 训练的模型文件
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/40adb856-ccc8-47bc-a719-6fad43730818)

### 混入其它数据进行训练
- 数据
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/4c41ddc9-aa33-4e37-a848-ac71c1fd2c27)

- 训练过程：
  - 300轮
    ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/c1682739-7e20-472d-b084-da99f40f5b78)

  - 600轮，未过拟合
 ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/cccb5bbb-e60a-412c-92ff-cce1b7ccf838)

### 模型转换
- 命令
```
# 创建一个保存转换后 Huggingface 格式的文件夹
mkdir -p /root/ft/huggingface

# 模型转换
# xtuner convert pth_to_hf ${配置文件地址} ${权重文件地址} ${转换后模型保存地址}
xtuner convert pth_to_hf /root/ft/train/internlm2_1_8b_qlora_alpaca_e3_copy.py /root/ft/train/iter_768.pth /root/ft/huggingface
```
命令可加参数：
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/c617de0d-d39f-464d-81bd-e5def4ea2f71)


- LoRA模型文件
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/c8a966aa-ad0f-4fbb-8df7-e20b43ac5488)

### 模型整合
我们通过视频课程的学习可以了解到，对于 LoRA 或者 QLoRA 微调出来的模型其实并不是一个完整的模型，而是一个额外的层（adapter）。那么训练完的这个层最终还是要与原模型进行组合才能被正常的使用。

而对于全量微调的模型（full）其实是不需要进行整合这一步的，因为全量微调修改的是原模型的权重而非微调一个新的 adapter ，因此是不需要进行模型整合的。

- 命令
```
# 创建一个名为 final_model 的文件夹存储整合后的模型文件
mkdir -p /root/ft/final_model

# 解决一下线程冲突的 Bug 
export MKL_SERVICE_FORCE_INTEL=1

# 进行模型整合
# xtuner convert merge  ${NAME_OR_PATH_TO_LLM} ${NAME_OR_PATH_TO_ADAPTER} ${SAVE_PATH} 
xtuner convert merge /root/ft/model /root/ft/huggingface /root/ft/final_model
```
其它参数：
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/ed2a874d-ec5a-42ce-8908-fda062b433ce)

- 整合后的模型
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/90704c77-3626-4365-91c6-641650bd371e)

### 对话测试

- 原模型会报错，未修复
  ![Uploading image.png…]()

- 整合后的模型
  - 有时候自己会陷入循环
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/af03b7fc-850e-40a5-921d-7b0f8388a3b1)
  - 正常测试
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/052c5975-63ee-42e1-8c9c-d043c38c14f8)

### web部署
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/a9032c83-b74a-46ca-bd5a-24322ede801b)

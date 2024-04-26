## 课程6作业

## 基础作业

### Lagent Web Demo

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/bda94e45-e603-4136-8b1a-900024b1637d)

### 用 Lagent 自定义工具

```
export WEATHER_API_KEY=8ee05d8229cc476b8bcd760ab14a6c41
# 比如 export WEATHER_API_KEY=1234567890abcdef
conda activate agent
cd /root/agent/Tutorial/agent
streamlit run internlm2_weather_web_demo.py --server.address 127.0.0.1 --server.port 7860
```

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/e81278d9-a750-4001-9a9e-d7127852b168)

### 直接使用 AgentLego

- 运行前图片

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/08440e78-07cc-40ac-9ba5-f6a23f760b8c)


- 运行后结果和新产生的图片，图片中的人物和汽车倍识别并标注出来了

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/d1eaea51-6c89-4358-82a2-fd9185c21e57)

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/110702a8-31fc-45fb-9ecf-0ea501fad245)

## 进阶作业

### 作为智能体工具使用
- 服务器端

  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/d38d6a47-67b4-4603-8682-2fa04660e688)

- WebUI

  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/b0f7ef6c-7526-4607-85d4-30f6fa2368d9)

- 运行结果
  - 服务器地址一定要写对，不然不会有任何回答
  
   ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/afd14cc1-0ba1-4ea7-9e10-38bc0ad94c87)

### 用 AgentLego 自定义工具

- 花仙子动漫图
  
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/3519f90f-5a28-4588-aece-a807dd2b8bf5)

- 骏马图，生成的结果比较差

  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/6dae9365-4837-4d7d-ac61-09ee10cede17)

- 世界地图，结果较差

  ![image](https://github.com/tangyanlin/llm_notes/assets/2775580/a16159ae-6076-44bc-94ea-e5e307a83d8b)


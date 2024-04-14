
## 第三课作业

### 基础作业 
#### 在茴香豆 Web 版中创建自己领域的知识问答助手
##### 笔记
  RAG技术之前有听到过，对技术细节不是很了解，只觉得是在模型前加上搜索引擎的检索结果这次课程系统地梳理了大模型生成问答的完整过程。通过对茴香豆Web页面的操作，也了解了RAG过程可以提供一些相关文字信息文档给模型，作为前期的信息补充，可以提供正例反例帮助限制问答内容，也可以直接集成到聊天软件比如微信和飞书，通过一系列简单的操作可以构建好一个某个领域的问答系统。
  茴香豆是一个基于LLMs的领域知识助手，专为即时通讯工具中的群聊场景优化的工作流，通过应用检索增强生成RAG技术，茴香豆能够理解和高效准确的回应与特定知识领域相关颚复杂查询。 茴香豆由四个部分组成：
  - 知识库，txt，pdf，doc，markdown等格式
  - 前端，即时通讯软件
  - LLM后端：本地模型：书生浦语，通义千问，远端模型：chatGPT等
  - 茴香豆，打通流程
  试用Web版产品的过程中主要是想知道补充文档对于最后的回答帮助到底有多大，所以选取的问题在有补充文档和没有补充文档的情况下作了对比，具体的结果可以看下面的截图，我创建的是一个oCPC广告投放问答助手，因为自己从事效果广告所以对答案的正确性有更好的经验判断，上传的参考资料是oCPC产品常见的问题和答案，用完之后主要讲几点总结：
- RAG总体的逻辑应该是会首先用emebdding和检索技术找到和问题相关的一些文本，然后将问题和相关文本一起喂给大模型，大模型最终给出答案，然后也可以结合正负例问题进行规则性的回答
- 在这个过程中其实emebdding和检索的技术也是很关键的节点，如果检索不到实际上是相关的文本内容，也会导致回答得不是很好
- 茴香豆中应该还是嵌入了一些其他的问答规则，有一些回答并不与预期完全符合
- 有时间可以阅读一下茴香豆的源码，了解问答中出现的一些疑问
##### oCPC投放助手茴香豆助手对话截图
分别查询有补充文档和无补充文档的效果差异
- 问题一：oCPC投放适合哪些广告主？
参考了补充文档之后，回答更加合理
 - 无补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/3a4e87a9-dd17-42e5-b6d4-fe46b4810ed1)

 - 有补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/7e1f17d5-8610-4c34-b552-11dcc70df6ca)

- 问题二：oCPC投放如何选择数据接入方式和转化类型
参考了补充文档之后，回答更加合理
 - 无补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/e36e8b16-b3c6-4571-8221-9f77f4e815b5)

 - 有补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/87eed4f8-1bad-447b-823a-cec22381a370)

- 问题三：一阶出现流量低成本不稳定的情况，是oCPC的原因吗？
无补充文档上传的文档内容其实和oCPC无关只是一些数字，但是回答也会说是根据材料检索得到的一些信息
 - 无补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/70671fe8-9172-406a-8f40-5da83a80229e)

 - 有补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/c3e0fa94-907f-4858-a5be-bc2f0a3ef6ca)

- 问题四：你好

 - 无补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/7ad1d7e8-3c09-4e47-bd63-19cc26748186)

 - 有补充文档

![image](https://github.com/tangyanlin/llm_notes/assets/2775580/e0486975-346a-4728-869d-85417c25b25d)

- 问题五：同一主体下的多个账户oCPC二阶后互相抢量怎么办？
在补充材料中明明有相关问题的回答，但是给出了材料并没有提供相关内容的回答，可能基于embedding的文本相似度检索还是需要进一步优化
 - 无补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/102fd2c2-794e-4377-b477-64f474ea0f17)

 - 有补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/f70efefc-9d50-4df0-87d2-f91cd7fab9f5)

- 问题六：怎么评价余华的《第七天》这本书
无关问题会拒绝回答
- 无补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/ab11f5a4-5a4d-452f-a4e3-e90d270615b6)

- 有补充文档
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/0c953cb5-ae27-460e-8def-7dc1d84f470b)

#### 在 InternLM Studio 上部署茴香豆技术助手
##### 笔记
相对于茴香豆Web版的体验，茴香豆的部署过程会让我们对茴香豆有进一步的认识。
首先是茴香豆基本环境的下载和安装：
- 下载模型：bce embedding模型和internlm2-chat-7b大模型
- 下载和安装茴香豆和所需要的环境，这里主要讲一下faiss，是一种用的比较多的向量检索框架，langchain是大模型使用比较广泛的应用工具
然后是修改一些配置文件，主要是三个模型的配置路径：bce模型，faiss检索模型，还有大模型
最后需要部署茴香豆：
- 创建知识库，下载茴香豆语料，提取知识库特征，创建向量数据库。数据库向量化的过程应用到了 LangChain 的相关模块，默认嵌入和重排序模型调用的网易 BCE 双语模型
- 除了语料知识的向量数据库，茴香豆建立接受和拒答两个向量数据库，用来在检索的过程中更加精确的判断提问的相关性
- 将上述内容都创建到向量数据库，检索过程中，茴香豆会将输入问题与两个列表中的问题在向量空间进行相似性比较，判断该问题是否应该回答，避免群聊过程中的问答泛滥。确定的回答的问题会利用基础模型提取关键词，在知识库中检索 top K 相似的 chunk，综合问题和检索到的 chunk 生成答案。
- 运行茴香豆即可
进阶内容可以使用远程模型方式，本地embebding检索还是需要，只是模型使用了远程的模型，只需要修改相关配置即可，可惜没有远程模型的API-KEY，只是搭建了demo，稳运行得到答案。
##### 截图
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/d11b93f3-c4e4-4cb1-83ef-1fb1d215245f)
远程模型，缺少远程模型的API_KEY
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/222e9d68-a001-40cb-879c-03d2d97b84bd)
![image](https://github.com/tangyanlin/llm_notes/assets/2775580/66c3bff8-a3e9-4697-98c8-19c3d9577f90)

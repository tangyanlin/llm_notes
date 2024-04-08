## course 3

### 遇到的问题及解决
- 在创建向量数据库过程中执行python3 -m huixiangdou.service.feature_store --sample ./test_queries.json遇到报错：
<img width="1050" alt="image" src="https://github.com/tangyanlin/llm_notes/assets/2775580/b667746f-9c01-4670-af3f-fee19b5b46f0">
- 解决方法：将huixiangdou/service/retriever.py中的context_max_length: int = 16000 修改为context_max_length: int = 128000

# Blog

## 【LLM整理】
#### 1. LLM训练和推理提速技巧 [Link](https://github.com/Dr-Corgi/Blog/issues/7)
- 使用**Attention with Linear Biases**替换正弦位置编码，获得更好的外推能力并加速**训练**；
- 使用**稀疏Attention**（例如BigBird）避免对所有Attention进行计算，加速**训练和推理**；
- 使用**FlashAttention**提高GPU上的Attention计算效率，这一功能已被pytorch2.0内置，加速**训练和推理**；
- 使用**Multi-Query Attention**共享各个注意力头上的线性投影层，加速**推理**；
- 使用**条件计算**区分出轻/重前馈分支，特别对于d >> n场景，减少不重要token的计算量，加速**训练和推理**。

#### 2. AlpacaFarm解析 [Link](https://github.com/Dr-Corgi/Blog/issues/8)
斯坦福提出AlpacaFarm来解决三个问题：
- 高成本的数据注释。提出了混合API LLM标注方案，能够与人类标注者有较高的一致性同时捕捉标注者之间的差异性和偏好；
- 缺乏公认的自动化评估方案。混合已有的公开数据集和真实人类指令得到评估数据集，将Davinci-003作为基线模型进行胜率对比，实现较一致的模型比较；
- 缺乏充分验证的人类偏好学习方法。评估了6种基于偏好的学习模型，结论是PPO效果最好。公布了相关的评估代码。

同时，AlpacaFarm数据集还存在一些局限性，包括：
- 采用的指令相对较为简答
- 评分时可能更偏向于风格而非事实
- 没有衡量模型可能造成的危害

#### 3. LLaMA模型如何扩充中文词表 [Link](https://github.com/Dr-Corgi/Blog/issues/5)
没有中文词表的tokenizer会导致序列长度显著增长，且模型很难将中文与其他utf-8字节进行区分，并学习到其中的语义。

扩充方法是首先用中文语料训练tokenizer，然后将中文tokenizer词表与原始词表（尾部）合并。然后进行两阶段预训练，第一阶段只训练embedding，第二阶段才进行全量参数微调。


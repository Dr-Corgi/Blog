# Blog

## LLM整理
#### LLM训练和推理提速技巧 [Link](https://github.com/Dr-Corgi/Blog/issues/7)
- 使用**Attention with Linear Biases**替换正弦位置编码，获得更好的外推能力并加速**训练**；
- 使用**稀疏Attention**（例如BigBird）避免对所有Attention进行计算，加速**训练和推理**；
- 使用**FlashAttention**提高GPU上的Attention计算效率，这一功能已被pytorch2.0内置，加速**训练和推理**；
- 使用**Multi-Query**共享各个注意力头上的线性投影层，加速**推理**；
- 使用**条件计算**区分出轻/重前馈分支，特别对于d >> n场景，减少不重要token的计算量，加速**训练和推理**。

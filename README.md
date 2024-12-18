# LearnVLM

# Tips

1. 问题：一阶段的数据量小、batch大，导致模型回答重复、胡言乱语。 如何解决？

    解决：多模态大模型欠拟合，调小Batch。

2. 问题：一阶段训练的模型回答重复、大量无意义符号。如何解决？

   解决：二阶段指令微调后大幅改善。



## 基本概念&术语

1. SYSTEM

   定义模型的角色

   ```python
   SYSTEM=('A chat between a curious user and an artificial '
   'intelligence assistant. The assistant gives '
   'helpful, detailed, and polite answers to the '
   'user's questions. {system}\n '),
   ```

2. INSTRUCTION  

   用户输入和助手回答的格式

   ```python
   'USER: {input} ASSISTANT:'
   ```

3. SEP  

   “分隔符”（Separator）的缩写，表示不同对话部分的分隔符

4. prompt_template

   提示词模板

   ```python
   dict(
     SYSTEM=('A chat between a curious user and an artificial '
             'intelligence assistant. The assistant gives '
             'helpful, detailed, and polite answers to the '
             'user\'s questions. {system}\n '),
     INSTRUCTION=('USER: {input} ASSISTANT:'),
     SEP='\n'),
   ```


# Code

## 1. Tokenizer文本编解码

```python

# 导入所需库
from transformers import AutoTokenizer

# 加载tokenizer
tokenizer = AutoTokenizer.from_pretrained("/xxx/lmsys/vicuna-7b-v1.5")

# 示例文本
text = "这是一个示例文本。"

# 对文本进行编码,生成token_id
encoded_text = tokenizer.encode(text)

# 将编码后的序列id解码为原始文本
decoded_text = tokenizer.decode(encoded_text)

print("原始文本：", text)
print("解码后的文本：", decoded_text)
```



# LLM

## 1. Base/Instruction/Chat区别

[links](https://mp.weixin.qq.com/s?__biz=MjM5MTIyMjkzMg==&mid=2247484931&idx=1&sn=89c33bf597001b1a570334ac0a593047&chksm=a7ddbbc19ca4d634481205243b930e35fc4642ab78828f38701fbdca34c461620ae752edd83a&mpshare=1&scene=1&srcid=1216Hpzl8oaf3qURolG5uGo5&sharer_shareinfo=fd34882e000bbdced65bb176500b9e22&sharer_shareinfo_first=fd34882e000bbdced65bb176500b9e22#rd)
大模型训练分为三阶段：预训练Pretrain、有监督微调SFT、人类反馈强化学习RLHF

| 类型            | 经历阶段                     | 特点                                                         |
| --------------- | ---------------------------- | ------------------------------------------------------------ |
| Base模型        | Pretrain                     | 经过大规模数据的无监督学习，掌握基本的语言生成能力和背景知识，具备语言理解能力。 |
| Instruction模型 | Pretrain + SFT + RLHF        | 经过监督微调和强化学习，理解和执行复杂的自然语言指令。       |
| Chat模型        | Pretrain + SFT + RLHF + 微调 | 经过进一步微调，专注于对话的上下文理解能力和多轮交流连贯性。 |



## 待学习

AutoConfig   AutoTokenizer   AutoModel   AutoProcess

transformer结构     位置编码作用   self attention   cross attention

多头注意力（ 类似分组卷积）


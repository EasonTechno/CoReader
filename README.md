<div align="center">
  <img src="logo.png" width="200" height="200">
  <h1>✨ CoReader 🚀</h1>
  
  ![lisense](https://img.shields.io/github/license/EasonTechno/CoReader?color=blue)
  ![lisense](https://img.shields.io/badge/Eason-Techno-blue)

  [🇺🇸 English](README-us.md) | 🇨🇳 中文 | [🇪🇸 Español](docs/README-es.md)
</div>

## 📑 内容目录
- [🌟 简介](#-简介)
- [🚀 快速上手](#-快速上手)
- [🧩 注意事项](#-注意事项)
- [🤝 贡献者](#-贡献者)
- [📄 协议](#-协议)

## 🌟 简介
🎯 **这是什么?**:
- 🧠 这是为了完全不懂编程的**新手**设计的，让其无障碍的理解代码所表示的含义的程序
- 🔄 这是一个将编程语言转化为人类可以理解的**自然语言**的程序
- 📊 这是一个可以让人跟着代码**一同思考**的程序

🔧 **这不是什么**:
- ⚡️ 这不是一个**代码解释器**，并不是用来为代码**生成注释**的工具
- 🧪 这并不是单纯的将代码用自然语言描述，而是生成一种用户可以**跟随其思考**的语言
- 📱 这不是为了**编程高手**设计的软件

🔧 **实现原理**:

我们使用AI完成这个操作，并且将分为两个版本:
- **提示词版本 | prompt edition**:使用一套为了不同模型设计的提示词，达到效果（效果一般）
- **大模型版本 | model edition**: 使用训练过的大预言模型，达到效果(效果更好)

## 🚀 快速上手

提示词版本 | Prompt Edition

1. 安装依赖

pip install openai anthropic transformers

2. 设置API密钥

import openai
openai.api_key = "你的OpenAI_API_KEY"

3. 使用示例

def explain_code(code, model="gpt-4"):
prompt = f"""
请以初学者的角度逐步解释以下代码：
{code}

要求：
1. 用自然语言描述每部分的功能
2. 使用比喻和生活例子帮助理解
3. 避免专业术语
4. 采用对话式语气
"""
response = openai.ChatCompletion.create(
model=model,
messages=[{"role": "user", "content": prompt}]
)
return response.choices[0].message.content

大模型版本 | Model Edition

1. 数据集准备

# 示例数据格式
dataset = [
{
"code": "for i in range(5):",
"explanation": "这就像数数从0到4，总共会数5次。想象你在数苹果：第一个、第二个...直到第五个。"
},
{
"code": "if x > 10:",
"explanation": "这就像检查你的零花钱是否超过10元。如果是，就可以买那个玩具。"
}
]

2. 微调模型（使用Hugging Face）

from transformers import AutoModelForSeq2SeqLM, AutoTokenizer, Trainer, TrainingArguments

model = AutoModelForSeq2SeqLM.from_pretrained("t5-small")
tokenizer = AutoTokenizer.from_pretrained("t5-small")

training_args = TrainingArguments(
output_dir="./results",
per_device_train_batch_size=4,
num_train_epochs=3,
)

trainer = Trainer(
model=model,
args=training_args,
train_dataset=tokenized_dataset,
)

trainer.train()

3. 使用微调后的模型

def explain_with_finetuned(code):
inputs = tokenizer(code, return_tensors="pt")
outputs = model.generate(**inputs)
return tokenizer.decode(outputs[0], skip_special_tokens=True)

两种版本对比

特性 提示词版本 大模型版本
实现难度 ⭐⭐ ⭐⭐⭐⭐
解释质量 ⭐⭐⭐ ⭐⭐⭐⭐⭐
响应速度 ⭐⭐⭐⭐ ⭐⭐⭐
定制灵活性 ⭐⭐ ⭐⭐⭐⭐⭐
成本 按API调用付费 前期训练成本高，后期使用成本低

推荐使用场景

提示词版本：快速验证想法、小规模使用、需要支持多种编程语言
大模型版本：需要高质量解释、特定领域代码、长期大规模使用



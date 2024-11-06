
让我举个例子说明 `model.generate()` 返回的 `output` 结构。根据不同的模型类型，返回的格式略有不同：

1. **对于 OpenAI 模型**：
```python
output = {
    "output": "这是一个很好的问题。根据给定的上下文，答案是...",  # 模型生成的文本
    "input_len": 128,           # 输入tokens数量
    "output_len": 56,          # 输出tokens数量
    "input_text": [            # 原始输入
        {
            "role": "user",
            "content": "请解释什么是机器学习？"
        }
    ]
}
```


2. **对于 HuggingFace 模型**：
```python
output = {
    "output": "机器学习是人工智能的一个子领域...",  # 模型生成的文本
    "input_len": 150,          # 输入长度
    "output_len": 75,          # 输出长度
    "input_text": "Question: 什么是机器学习？\nAnswer: "  # 原始输入
}
```


3. **实际运行示例**：
```python
# 输入示例
inputs = {
    "question": "什么是机器学习？",
    "context": "机器学习是人工智能的一个分支，它使用数据和算法来模仿人类的学习过程..."
}

# 模型生成的 output
output = {
    "output": "机器学习是人工智能的一个重要分支，它通过数据和算法来模仿人类的学习过程。通过分析大量数据，机器学习系统能够发现模式并做出决策，而无需明确的编程指令。",
    "input_len": 156,
    "output_len": 82,
    "input_text": "Context: 机器学习是人工智能的一个分支...\nQuestion: 什么是机器学习？\nAnswer: ",
    "parsed_output": "机器学习是人工智能的一个重要分支...",  # 经过后处理的输出
}

# 在代码中的使用
logger.info(f"Input length: {output['input_len']}")
logger.info(f"Generated text: {output['output']}")
logger.info(f"Output length: {output['output_len']}")
```


这个 `output` 字典包含了：
1. 模型生成的文本
2. 输入和输出的长度统计
3. 原始输入文本
4. 可能的其他元数据（如解析后的输出）

这些信息后续会被用于：
- 计算评估指标
- 生成结果报告
- 分析模型性能
- 记录实验结果
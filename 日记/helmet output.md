





让我模拟一个典型的 `output` 字典内容：

```python
output = {
    # 1. 命令行参数
    "args": {
        "model_name_or_path": "bigscience/T0_3B",
        "datasets": "qa_dataset",
        "test_files": "test.json",
        "demo_files": "demo.json",
        "input_max_length": 512,
        "generation_max_length": 128,
        "temperature": 0.7,
        "top_p": 0.9,
        "seed": 42,
        "use_chat_template": False,
        "do_sample": True,
        # ... 其他参数
    },

    # 2. 测试结果数据
    "data": [
        {
            "question": "什么是机器学习？",
            "answer": "机器学习是AI的一个分支...",
            "output": "机器学习是一种让计算机通过数据学习的方法...",
            "parsed_output": "机器学习是一种让计算机通过数据学习的方法...",
            "input_len": 45,
            "output_len": 68,
            "accuracy": 0.85,
            "f1_score": 0.78
        },
        {
            "question": "深度学习的应用有哪些？",
            "answer": "深度学习在图像识别、自然语言处理等领域有广泛应用...",
            "output": "深度学习主要应用在计算机视觉、NLP等方面...",
            "parsed_output": "深度学习主要应用在计算机视觉、NLP等方面...",
            "input_len": 52,
            "output_len": 75,
            "accuracy": 0.92,
            "f1_score": 0.88
        },

    ],

    # 3. 原始指标数据
    "metrics": {
        "input_len": [45, 52, 48, 63],  # 每个样本的输入长度
        "output_len": [68, 75, 82, 59],  # 每个样本的输出长度
        "accuracy": [0.85, 0.92, 0.78, 0.88],  # 每个样本的准确率
        "f1_score": [0.78, 0.88, 0.72, 0.85]  # 每个样本的F1分数
    },

    # 4. 平均指标
    "averaged_metrics": {
        "input_len": 52.6,      # 平均输入长度
        "output_len": 71.2,     # 平均输出长度
        "accuracy": 85.75,      # 平均准确率（已乘100）
        "f1_score": 80.75       # 平均F1分数（已乘100）
    },

    # 5. 内存使用情况（以字节为单位）
    "memory_usage": 8589934592,  # 约8GB

    # 6. 处理速度
    "throughput": 2.5  # 每秒处理2.5个样本
}
```



这个输出会被保存为两个文件：

1. **完整结果文件** (`output_path`):
```json
{
    // 上面的完整输出内容
}
```



2. **简化的评分文件** (`output_path + ".score"`):
```json
{
    "input_len": 52.6,
    "output_len": 71.2,
    "accuracy": 85.75,
    "f1_score": 80.75
}
```



这种结构设计的优点：
1. 保存了完整的实验配置（args）
2. 包含了详细的测试结果（data）
3. 保留了原始指标数据（metrics）
4. 提供了汇总的性能指标（averaged_metrics）
5. 记录了资源使用情况（memory_usage, throughput）
6. 便于后续分析和对比实验结果
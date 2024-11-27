在代码中，结果收集是通过 `args.get_averaged_metric()` 方法实现的，这个方法会读取评测结果文件。让我详细解释：

```python
def get_averaged_metric(self):
    """获取评测指标"""
    # 1. 获取结果文件路径
    path = self.get_path()  # 获取结果文件路径
    if not os.path.exists(path):
        print("path doesn't exist")
        return None
```

结果文件的路径结构：
```
output/
└── {model_name}/                # 模型名称目录
    └── {dataset}_{tag}_{test_name}_in{input_length}_size{test_samples}_shots{shots}...json
```

例如：
```
output/
└── LLaMA-2-7B/
    ├── nq_v1_test_in32768_size100_shots2.json
    ├── hotpotqa_v1_test_in32768_size100_shots2.json
    └── ...
```

结果文件内容示例：
```json
{
    "averaged_metrics": {
        "exact_match": 0.85,
        "f1_score": 0.88,
        "rouge_score": 0.76
    },
    "individual_results": [
        {
            "question": "...",
            "prediction": "...",
            "reference": "...",
            "metrics": {...}
        }
    ]
}
```

收集过程：
1. 每个模型对每个数据集的评测都会生成一个结果文件
2. `collect_results.py` 读取这些文件
3. 提取其中的评测指标
4. 整理成统一格式

这些结果文件是在之前的评测过程中生成的，`collect_results.py` 的作用是收集和整理这些已有的结果。
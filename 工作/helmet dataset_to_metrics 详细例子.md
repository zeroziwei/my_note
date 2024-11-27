








让我用具体例子说明 `dataset_to_metrics` 的使用：

1. 对于简单QA任务：
```python
# 配置定义
dataset_to_metrics = {
    "nq": "substring_exact_match",
}

# 模拟结果文件 nq_test.json.score
{
    "substring_exact_match": 0.85,  # 85%的答案完全匹配
    "other_metrics": 0.9
}

# 使用示例
args.dataset = "nq"
metric = args.get_averaged_metric()
# 返回: {"substring_exact_match": 0.85}
```


2. 对于重排序任务：
```python
# 配置定义
dataset_to_metrics = {
    "msmarco_rerank_psg": "NDCG@10",
}

# 模拟结果文件 msmarco_test.json.score
{
    "NDCG@10": 0.76,  # NDCG@10分数
    "MRR": 0.65,
    "Recall@10": 0.82
}

# 使用示例
args.dataset = "msmarco_rerank_psg"
metric = args.get_averaged_metric()
# 返回: {"NDCG@10": 0.76}
```


3. 对于多指标评估任务：
```python
# 配置定义
dataset_to_metrics = {
    "alce_asqa": ["str_em", "citation_rec", "citation_prec"],
}

# 模拟结果文件 alce_test.json.score
{
    "str_em": 0.75,        # 答案准确率
    "citation_rec": 0.82,  # 引用召回率
    "citation_prec": 0.88, # 引用精确率
    "other_metric": 0.9
}

# 使用示例
args.dataset = "alce_asqa"
metric = args.get_averaged_metric()
# 返回: {
#     "str_em": 0.75,
#     "citation_rec": 0.82,
#     "citation_prec": 0.88
# }
```


4. 完整处理流程示例：
```python
# 1. 定义映射
dataset_to_metrics = {
    "nq": "substring_exact_match",
    "msmarco_rerank_psg": "NDCG@10",
    "alce_asqa": ["str_em", "citation_rec", "citation_prec"],
}

# 2. 模拟评测过程
def process_evaluation():
    results = []
    
    # 模拟不同模型的评测
    models = [
        {"name": "Model-A", "output_dir": "output/model_a"},
        {"name": "Model-B", "output_dir": "output/model_b"}
    ]
    
    for model in models:
        # 模拟不同数据集的评测
        datasets = [
            {
                "dataset": "nq",
                "results": {"substring_exact_match": 0.85}
            },
            {
                "dataset": "msmarco_rerank_psg",
                "results": {"NDCG@10": 0.76}
            },
            {
                "dataset": "alce_asqa",
                "results": {
                    "str_em": 0.75,
                    "citation_rec": 0.82,
                    "citation_prec": 0.88
                }
            }
        ]
        
        for dataset in datasets:
            args = arguments()
            args.dataset = dataset["dataset"]
            args.output_dir = model["output_dir"]
            
            # 获取评测指标
            metrics = args.get_averaged_metric()
            
            # 收集结果
            for metric_name, value in metrics.items():
                results.append({
                    "model": model["name"],
                    "dataset": dataset["dataset"],
                    "metric_name": metric_name,
                    "value": value
                })
    
    return results

# 3. 生成评测报告
results = process_evaluation()
df = pd.DataFrame(results)
print(df.pivot_table(
    index="model",
    columns=["dataset", "metric_name"],
    values="value"
))
```

输出示例：
```
           nq                msmarco        alce_asqa
           exact_match      NDCG@10        str_em    citation_rec    citation_prec
Model-A    0.85            0.76           0.75      0.82           0.88
Model-B    0.82            0.73           0.72      0.80           0.85
```

这样的设计允许：
1. 自动选择正确的评估指标
2. 处理多指标评估
3. 标准化结果收集
4. 生成结构化报告

需要我详细解释某个具体部分吗？
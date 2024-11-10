

```mermaid
flowchart TD
    A[开始 post_process] --> B{数据集类型判断}
    
    B -->|msmarco_rerank| C[msmarco处理]
    C --> C1[解析排名 parse_rankings]
    C1 --> C2[计算检索指标]
    C2 --> C3[返回检索评估结果]
    
    B -->|json_kv| D[json_kv处理]
    D --> D1[获取预测值]
    D1 --> D2[计算基础指标]
    D2 --> D3[解析输出]
    D3 --> D4[重新计算指标]
    D4 --> D5[取最佳指标]
    
    B -->|narrativeqa| E[narrativeqa处理]
    E --> E1[计算基础指标]
    E1 --> E2[解析输出]
    E2 --> E3[计算ROUGE分数]
    
    B -->|alce| F[alce处理]
    F --> F1[解析引用]
    F1 --> F2[计算引用准确率]
    F2 --> F3[评估生成质量]
    
    B -->|default| G[默认处理]
    G --> G1[计算基础指标]
    G1 --> G2[解析输出]
    G2 --> G3[重新计算指标]
    G3 --> G4[取最佳值]
    
    subgraph 返回结构
        R1[metrics字典]
        R2[others字典]
    end
    
    C3 --> R1
    D5 --> R1
    E3 --> R1
    F3 --> R1
    G4 --> R1
    
    C3 --> R2
    D5 --> R2
    E3 --> R2
    F3 --> R2
    G4 --> R2
```

主要处理逻辑：

1. msmarco_rerank:
```python
def post_process(output, example):
    parsed_pred = parse_rankings(output["output"])
    mets = calculate_retrieval_metrics(
        {example['qid']: parsed_pred}, 
        qrels, 
        k_values
    )
    return mets, {"parsed_output": parsed_pred}
```

2. json_kv:
```python
def post_process(output, example):
    prediction = output["output"]
    answer = example["answer"]
    mets = calculate_metrics(prediction, answer)
    parsed_pred = parse_output(prediction, "corresponding value:")
    new_mets = calculate_metrics(parsed_pred, answer)
    mets = {k: max(v, new_mets[k]) for k, v in mets.items()}
    return mets, {"parsed_output": parsed_pred}
```

3. default:
```python
def default_post_process(output, example):
    prediction = output["output"]
    answer = example["answer"]
    mets = calculate_metrics(prediction, answer)
    parsed_pred = parse_output(prediction)
    if parsed_pred is not None:
        new_mets = calculate_metrics(parsed_pred, answer)
        mets = {k: max(v, new_mets[k]) for k, v in mets.items()}
    return mets, {"parsed_output": parsed_pred}
```

每种数据集类型的特点：
1. msmarco: 关注文档排序和检索性能
2. json_kv: 关注键值对提取的准确性
3. narrativeqa: 关注生成答案的质量
4. alce: 关注引用准确性和生成质量
5. default: 通用的评估方式

需要我详细解释某个特定数据集的处理流程吗？
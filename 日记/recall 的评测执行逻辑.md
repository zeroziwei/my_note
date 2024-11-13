
Recall (Information Retrieval Recall):

```mermaid
flowchart TD
    A[Recall评测流程] --> B[解析模型输出]
    B --> C[获取真实相关文档]
    C --> D[计算召回指标]
    
    B --> B1[提取文档ID]
    B --> B2[排序处理]
    
    C --> C1[加载qrels]
    C --> C2[获取相关文档集合]
    
    D --> D1[计算Recall@K]
    D --> D2[计算MRR]
    D --> D3[计算其他指标]
```


1. 后处理函数结构：
```python
def post_process(output, example):
    """Recall任务的后处理评测"""
    # 1. 解析模型输出，获取预测的文档ID列表
    parsed_pred = parse_rankings(output["output"])
    
    # 2. 获取查询ID和真实相关文档
    qid = example['qid']
    relevant_docs = qrels[qid]  # 从qrels获取真实相关文档
    
    # 3. 计算各项指标
    metrics = calculate_recall_metrics(
        pred_docs=parsed_pred,
        relevant_docs=relevant_docs,
        k_values=[1, 3, 5, 10, 20, 100]
    )
    
    return metrics, {"parsed_output": parsed_pred}
```


2. 解析排序结果：
```python
def parse_rankings(text):
    """解析模型输出的排序结果"""
    try:
        # 提取文档ID
        doc_ids = []
        for line in text.split('\n'):
            if line.strip():
                # 使用正则表达式提取文档ID
                doc_id = extract_doc_id(line)
                if doc_id:
                    doc_ids.append(doc_id)
        return doc_ids
    except:
        return []
```


3. 计算召回指标：
```python
def calculate_recall_metrics(pred_docs, relevant_docs, k_values):
    """计算各种召回相关指标"""
    metrics = {}
    
    # 1. 计算不同K值的召回率
    for k in k_values:
        recall_k = calculate_recall_at_k(
            pred_docs[:k], 
            relevant_docs
        )
        metrics[f'recall@{k}'] = recall_k
    
    # 2. 计算MRR (Mean Reciprocal Rank)
    metrics['mrr'] = calculate_mrr(pred_docs, relevant_docs)
    
    # 3. 计算Success@K
    for k in k_values:
        success_k = calculate_success_at_k(
            pred_docs[:k], 
            relevant_docs
        )
        metrics[f'success@{k}'] = success_k
        
    return metrics
```


4. 具体指标计算：
```python
def calculate_recall_at_k(pred_docs, relevant_docs):
    """计算Recall@K"""
    # 转换为集合进行计算
    pred_set = set(pred_docs)
    rel_set = set(relevant_docs)
    
    # 计算召回率
    if len(rel_set) == 0:
        return 0.0
    
    recall = len(pred_set & rel_set) / len(rel_set)
    return recall

def calculate_mrr(pred_docs, relevant_docs):
    """计算MRR"""
    rel_set = set(relevant_docs)
    
    # 找第一个相关文档的位置
    for i, doc_id in enumerate(pred_docs, 1):
        if doc_id in rel_set:
            return 1.0 / i
    
    return 0.0
```


5. 评测结果示例：
```python
{
    "metrics": {
        "recall@1": 0.15,
        "recall@5": 0.35,
        "recall@10": 0.45,
        "recall@20": 0.55,
        "recall@100": 0.75,
        "mrr": 0.25,
        "success@1": 0.15,
        "success@5": 0.40
    },
    "parsed_output": [
        "doc_1",
        "doc_5",
        "doc_3",
        ...
    ]
}
```


6. 评测特点：
- 关注召回效果：多个K值的召回率
- 排序质量：通过MRR评估排序准确性
- 成功率：通过Success@K评估检索成功率
- 完整性：保存解析后的排序结果

7. 使用qrels的原因：
```python
# qrels格式
qrels = {
    'query_id': {
        'doc_id': relevance_score,
        ...
    },
    ...
}
```
- 提供标准评测基准
- 支持多级相关性评分
- 便于跨系统比较

需要我详细解释某个具体部分吗？
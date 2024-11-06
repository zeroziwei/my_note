






`post_process` 主要是对模型输出进行后处理和评估。：

1. **问答任务的后处理**：
```python
def qa_post_process(output, test_item):
    """问答任务的后处理"""
    # 1. 清理和标准化输出文本
    generated_text = output["output"].strip()
    reference = test_item["answer"].strip()
    
    # 2. 计算评估指标
    mets = {
        # 精确匹配分数
        "exact_match": int(generated_text.lower() == reference.lower()),
        
        # F1分数（词重叠）
        "f1": calculate_f1_score(generated_text, reference),
        
        # ROUGE分数（文本相似度）
        "rouge_l": calculate_rouge_l(generated_text, reference)
    }
    
    # 3. 其他处理结果
    others = {
        "parsed_output": generated_text,  # 清理后的输出
        "answer_length": len(generated_text.split()),  # 答案长度
        "is_valid": len(generated_text) > 0  # 有效性检查
    }
    
    return mets, others
```



2. **分类任务的后处理**：
```python
def classification_post_process(output, test_item):
    """分类任务的后处理"""
    # 1. 提取预测的类别
    prediction = extract_label(output["output"])  # 从输出中提取标签
    true_label = test_item["label"]
    
    # 2. 计算评估指标
    mets = {
        "accuracy": int(prediction == true_label),  # 准确率
        "confidence": extract_confidence(output["output"]),  # 置信度
    }
    
    # 3. 其他信息
    others = {
        "parsed_output": prediction,
        "prediction_class": prediction,
        "is_correct": prediction == true_label
    }
    
    return mets, others
```



3. **摘要任务的后处理**：
```python
def summary_post_process(output, test_item):
    """摘要任务的后处理"""
    # 1. 清理生成的摘要
    summary = clean_text(output["output"])
    reference = test_item["reference"]
    
    # 2. 计算ROUGE分数
    rouge_scores = calculate_rouge(summary, reference)
    mets = {
        "rouge1": rouge_scores["rouge1"],
        "rouge2": rouge_scores["rouge2"],
        "rougeL": rouge_scores["rougeL"],
        "length_ratio": len(summary) / len(reference)  # 长度比
    }
    
    # 3. 其他处理
    others = {
        "parsed_output": summary,
        "summary_length": len(summary.split()),
        "key_points": extract_key_points(summary)  # 提取关键点
    }
    
    return mets, others
```



主要功能：

1. **文本清理和标准化**：
```python
# 清理文本
text = output["output"].strip()
text = remove_special_chars(text)
text = normalize_whitespace(text)
```



2. **评估指标计算**：
```python
# 计算各种评估指标
metrics = {
    "accuracy": calculate_accuracy(),
    "f1": calculate_f1(),
    "rouge": calculate_rouge(),
    "bleu": calculate_bleu()
}
```



3. **特定信息提取**：
```python
# 提取特定信息
others = {
    "parsed_output": cleaned_text,
    "extracted_entities": extract_entities(text),
    "key_phrases": extract_key_phrases(text)
}
```



4. **格式验证和错误检查**：
```python
# 验证输出格式
if not is_valid_format(text):
    logger.warning("Invalid output format")
    # 处理无效输出
```



返回值的使用：
```python
# 后处理结果会被合并到原始输出中
mets, others = data['post_process'](output, test_item)
output.update({**others, **mets})

# 然后被用于计算统计指标
for k, v in mets.items():
    metrics[k].append(v)
```




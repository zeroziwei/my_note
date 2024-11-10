```mermaid 
flowchart TD
    A[开始 post_process] --> B[接收参数]
    
    B --> C1[获取模型输出]
    B --> C2[获取标准答案]
    
    C1 --> D1[计算原始指标]
    D1 --> E[存储原始指标]
    
    C1 --> D2[解析输出]
    D2 --> F{解析结果为空?}
    
    F -->|否| G1[计算解析后指标]
    G1 --> H[取最大值]
    
    F -->|是| I[保持原始指标]
    
    H --> J[准备返回值]
    I --> J
    
    J --> K[返回结果]
    
    subgraph 数据集特定处理
        L1[msmarco: 排名解析]
        L2[lexsum: 摘要评估]
        L3[icl: 分类评估]
        L4[json_kv: 键值对匹配]
    end
    
    B --> M{数据集类型}
    M -->|msmarco| L1
    M -->|lexsum| L2
    M -->|icl| L3
    M -->|json_kv| L4
    M -->|default| D1

    subgraph 返回值结构
        K1[metrics指标字典]
        K2[解析结果字典]
    end
    K --> K1
    K --> K2
```
主要处理步骤：

1. 输入处理：

	- 获取模型输出 (output["output"])
	
	- 获取标准答案 (example["answer"])

2. 指标计算：

	- 计算原始指标
	
	- 尝试解析输出
	
	- 计算解析后指标（如果可能）
	
	- 取两组指标的最大值

3.  特定数据集处理：

	- msmarco: 文档排序评估
	
	- lexsum: 摘要生成评估
	
	- icl: 分类任务评估
	
	- json_kv: 键值对提取评估

4.  返回两个字典：
	
	- metrics: 包含评估指标
	
	- others: 包含解析结果






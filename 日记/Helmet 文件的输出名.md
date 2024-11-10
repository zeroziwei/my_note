


```mermaid
flowchart TD
    A[开始构建输出路径] --> B[获取基本参数]
    
    B --> C1[获取数据集名称]
    B --> C2[获取标签tag]
    B --> C3[获取测试文件名]
    
    C2 --> D{是否为popqa}
    D -->|是| E[添加人气阈值到tag]
    D -->|否| F[使用原始tag]
    
    C3 --> G[提取测试文件基本名]
    
    E --> H[组合文件名]
    F --> H
    G --> H
    
    H --> I[添加参数信息到文件名]
    
    subgraph 文件名参数
        P1[数据集名称]
        P2[标签tag]
        P3[测试文件名]
        P4[输入长度]
        P5[样本数量]
        P6[few-shot数量]
        P7[采样设置]
        P8[生成最大长度]
        P9[生成最小长度]
        P10[温度参数]
        P11[top_p参数]
        P12[chat模板设置]
        P13[随机种子]
    end
    
    I --> J[构建最终输出路径]
    J --> K[返回完整文件路径]
```

文件名构建过程：

1. 基础部分：
```python
test_name = os.path.splitext(os.path.basename(test_file))[0]
```

2. 特殊处理：
```python
if dataset == "popqa":
    tag += f"_pop{args.popularity_threshold}"
```

3. 完整文件名格式：
```python
output_path = os.path.join(
    args.output_dir,
    f"{dataset}_"                     # 数据集
    f"{tag}_"                         # 标签
    f"{test_name}_"                   # 测试文件
    f"in{args.input_max_length}_"     # 输入长度
    f"size{args.max_test_samples}_"   # 样本数
    f"shots{args.shots}_"             # few-shot数
    f"samp{args.do_sample}"           # 采样
    f"max{args.generation_max_length}"# 最大长度
    f"min{args.generation_min_length}"# 最小长度
    f"t{args.temperature}"            # 温度
    f"p{args.top_p}_"                # top_p
    f"chat{args.use_chat_template}_"  # chat模板
    f"{args.seed}.json"               # 随机种子
)
```

这种命名方式的优点：
1. 包含所有重要参数信息
2. 便于实验追踪和复现
3. 避免文件覆盖
4. 方便结果分析和比较
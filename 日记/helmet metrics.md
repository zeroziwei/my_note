## 1. 首先，模型生成输出
	output = model.generate(inputs=inputs)
	if output is None:
	    logger.info(f"skipping example {idx+1} because the model returned None")
	    continue

## 2. 如果不使用聊天模板，添加系统提示
	if not args.use_chat_template:
	    prepend_text = data["system_template"].format(**test_item)
	    output["output"] = prepend_text + output["output"]

## 3. 处理输出结果
	mets, others = data['post_process'](output, test_item)
	output.update({**others, **mets})

## 4. 收集指标
metrics["input_len"].append(output["input_len"])  # 这里使用的 output
metrics["output_len"].append(output["output_len"])
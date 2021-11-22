+++
title = "Pegasus模型增强训练"
weight = 0420
chapter = true
pre = "<b>4.2</b>"
+++

在完成本地训练后，接下来我们模拟一个增强训练过程。在我们刚才完成的训练任务中，产生了模型文件`./models/local_train/pegasus-hp/checkpoint-500`，然后，我们运行

```
!python -u examples/pytorch/summarization/run_summarization.py \
--model_name_or_path models/pretrain/pegasus/checkpoint-46314 \
--do_train \
--do_eval \
--per_device_train_batch_size=2 \
--per_device_eval_batch_size=1 \
--save_strategy epoch \
--evaluation_strategy epoch \
--overwrite_output_dir \
--predict_with_generate \
--train_file './data/hp/summary/news_summary_cleaned_small_train.csv' \
--validation_file './data/hp/summary/news_summary_cleaned_small_test.csv' \
--text_column 'text' \
--summary_column 'headlines' \
--output_dir='./models/local_train/pegasus-hp' \
--num_train_epochs=1.0 \
--eval_steps=500 \
--save_total_limit=3 \
--source_prefix "summarize: " > train_pegasus_2.log
```


我们可以看到，在训练时，日志中有如下的记录`loading weights file models/pretrain/pegasus/checkpoint-46314/pytorch_model.bin`说明模型是导入了一个之前训练的基础版本，也可以通过训练的loss值以及最终的rouge指标判断出这个结果是增强训练产生的。

![](./pics/02pegasus/13.png)

同样的，我们可以利用本地推理进行测试。


本地测试，效果提升明显
```
import pandas as pd
df=pd.read_csv('./data/hp/summary/news_summary_cleaned_small_test.csv')
print('原文:',df.loc[0,'text'])
print('真实标签:',df.loc[0,'headlines'])
from transformers import pipeline
summarizer = pipeline("summarization", model="./models/local_train/pegasus-hp/checkpoint-500")
print('模型预测:',summarizer(df.loc[0,'text'], max_length=50)[0]['summary_text'])
```
输出
```
原文: Germany on Wednesday accused Vietnam of kidnapping a former Vietnamese oil executive Trinh Xuan Thanh, who allegedly sought asylum in Berlin, and taking him home to face accusations of corruption. Germany expelled a Vietnamese intelligence officer over the suspected kidnapping and demanded that Vietnam allow Thanh to return to Germany. However, Vietnam said Thanh had returned home by himself.
真实标签: Germany accuses Vietnam of kidnapping asylum seeker 
模型预测: Germany accuses Vietnam of kidnapping ex-oil exec, taking him home
```

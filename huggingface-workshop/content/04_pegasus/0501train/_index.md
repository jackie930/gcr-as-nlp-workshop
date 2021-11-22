+++
title = " Pegasus模型训练"
weight = 0410
chapter = true
pre = "<b>4.1</b>"
+++

我们现在会用sagemaker进行一个pegasus模型的本地训练，使用ML.P3.2xlarge机型。


## 数据准备 

首先下载代码
```
cd SageMaker
git clone https://github.com/HaoranLv/nlp_transformer.git
```


然后打开文件 `hp_main.ipynb`，逐行运行。

首先安装环境

![](../pics/02pegasus/05.png)

然后处理数据`hp_data.ipynb`，切分train/test。
注意这里，为了快速产生结果，我们只要用1000条数据训练，100条测试/验证
```
train_df.to_csv('./data/hp/summary/news_summary_cleaned_train.csv',index=False)
test_df.to_csv('./data/hp/summary/news_summary_cleaned_test.csv',index=False)
train_df[:1000].to_csv('./data/hp/summary/news_summary_cleaned_small_train.csv',index=False)
test_df[:100].to_csv('./data/hp/summary/news_summary_cleaned_small_test.csv',index=False)
```

![](../pics/02pegasus/06.png)

## 模型训练

接下来我们运行训练，首先下载我们已经训练好的模型
```
!mkdir -p models/pretrain/pegasus
!mkdir -p models/pretrain/bart

!mkdir -p ./models/local_train/pegasus-hp
!mkdir -p ./models/local_train/bart-hp

!aws s3 cp s3://datalab2021/hupo_nlp/models/pegasus/checkpoint-46314.zip models/pretrain/pegasus
!aws s3 cp s3://datalab2021/hupo_nlp/models/bart/checkpoint-46314.zip models/pretrain/bart
    
!unzip models/pretrain/pegasus/checkpoint-46314.zip -d models/pretrain/pegasus > /dev/null 2>&1
!unzip models/pretrain/bart/checkpoint-46314.zip -d models/pretrain/bart > /dev/null 2>&1
```

然后进行模型训练，这里没有用我们训练好的模型，而是使用huggingface上的公开的`pegasus-large`作为训练起点，为了演示目的，我们只运行一个epoch，大约需要5min

```
!python -u examples/pytorch/summarization/run_summarization.py \
--model_name_or_path google/pegasus-large \
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
--source_prefix "summarize: " > train_pegasus.log
```

训练完成后，会提示日志信息如下
* train
![](../pics/02pegasus/07.png)
* eval
![](../pics/02pegasus/08.png)


模型结果文件及相应的日志等信息会自动保存在`./models/local_train/pegasus-hp/checkpoint-500`

![](../pics/02pegasus/09.png)

## 结果本地测试

我们可以直接用这个产生的模型文件进行本地推理。注意这里的模型文件地址的指定为你刚刚训练产生的。

```
import pandas as pd
df=pd.read_csv('./data/hp/summary/news_summary_cleaned_small_test.csv')
print('原文:',df.loc[0,'text'])
print('真实标签:',df.loc[0,'headlines'])
from transformers import pipeline
summarizer = pipeline("summarization", model="./models/local_train/pegasus-hp/checkpoint-500")
print('模型预测:',summarizer(df.loc[0,'text'], max_length=50)[0]['summary_text'])
```

输出如下

```
原文: Germany on Wednesday accused Vietnam of kidnapping a former Vietnamese oil executive Trinh Xuan Thanh, who allegedly sought asylum in Berlin, and taking him home to face accusations of corruption. Germany expelled a Vietnamese intelligence officer over the suspected kidnapping and demanded that Vietnam allow Thanh to return to Germany. However, Vietnam said Thanh had returned home by himself.
真实标签: Germany accuses Vietnam of kidnapping asylum seeker 
模型预测: Germany accuses Vietnam of kidnapping ex-oil exec, taking him home

```

到这里，就完成了一个模型的训练过程。
+++
title = "MT5模型实验"
weight = 0410
chapter = true
pre = "<b>4.1</b>"
+++

首先下载代码, ！注意，如果你已经执行过实验2bart，无需重复下载，直接打开即可。
```
cd SageMaker
git clone https://github.com/jackie930/notebooks.git
```

然后打开`/home/ec2-user/SageMaker/notebooks/sagemaker/08_distributed_summarization_bart_t5/train-huggingface.ipynb`运行。 注意在运行前，需要将待训练的数据‘meta_description.parquet' 上传到同样的文件夹内。

训练开启后，可以看到产生了对应的训练日志

![](./1.png)

及对应的训练任务


![](./2.png)

大约25min完成训练，结束后可以直接部署模型文件为endpoint并进行推理。注意这里只用了1000条数据训练1轮，所以模型效果不佳。

![](./3.png)




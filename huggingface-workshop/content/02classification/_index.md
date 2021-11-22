+++
title = "动手实验2: 利用huggingface进行文本分类模型训练"
weight = 2
chapter = true
pre = "<b>2. </b>"
+++


文本分类任务作为最基础的任务，我们既可以用机器学习的方式，也可以用深度学习的方式进行训练，部署。

下面的结果为我们模型对比实验的结果：

| Model | Train sample  | Num_class | Train loss  | instance | train-time | epoch | eval loss|eval f1|
| ------ | ------ | ------ |------ |------ |------ |------ |------ |------ |
| Bert-base | 180000 | 31 |0.09|ml.p3.16xlarge|8h|5|1.26|0.67|
| Distillbert-base | 180000 | 31 |0.1489|ml.p3.16xlarge|3h|5|1.26|0.67|
| Xlm-Roberta-base | 180000 | 31 |0.9821|ml.p3.16xlarge|40min|1|0.78|0.79|
| Lightgbm (benchmark) | 180000 | 31 |0.56|ml.m5.xlarge|40min|5|1.19|0.71|


## quick start

下载代码 (如果已经进行过实验1，则无需重复下载代码)

```
cd SageMaker
git clone https://github.com/jackie930/notebooks.git
```

接下来我们进行模型的训练实验

* 机器学习模型： `/home/ec2-user/SageMaker/notebooks/sagemaker/14_train_text_calssfication/lightgbm.ipynb`运行
* 基于transformer的深度学习模型 `/home/ec2-user/SageMaker/notebooks/sagemaker/14_train_text_calssfication/train-huggingface.ipynb`运行

我们可以看到，通过`huggingface api`，我们可以通过更换不同的模型pretrain训练得到不同的结果。同样数据集，在`roberta`架构下表现最好
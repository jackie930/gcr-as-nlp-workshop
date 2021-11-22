+++
title = "动手实验3: 基于Amazon SageMaker的MT5中文摘要模型训练动手实验"
weight = 3
chapter = true
pre = "<b>3. </b>"
+++

## 模型架构

Transformer For Text Summary：T5 (JMLR): 旨在开发NLP通用模型：将多个NLP下游任务抽象为text-to-text的任务，不同的任务使用不同的profix来代表，由于任务类型相同，故可以协同训练。

![](./mt1.png)

mT5: A massively multilingualpre-trained text-to-text transformer 2021 google research 

大规模多语言T5预训练语言模型mT5，在覆盖101种语言的新的Common Crawl数据集上进行预训练，可直接适用于多语言场景，在各种基准测试集上展现出强大的性能。

![](./mt2.png)


## reference

* paper： https://arxiv.org/pdf/2010.11934.pdf
* source code：https://github.com/google-research/multilingual-t5 

@inproceedings{xue-etal-2021-mt5,
    title = "m{T}5: A Massively Multilingual Pre-trained Text-to-Text Transformer",
    author = "Xue, Linting  and
      Constant, Noah  and
      Roberts, Adam  and
      Kale, Mihir  and
      Al-Rfou, Rami  and
      Siddhant, Aditya  and
      Barua, Aditya  and
      Raffel, Colin",
    booktitle = "Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies",
    month = jun,
    year = "2021",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2021.naacl-main.41",
    doi = "10.18653/v1/2021.naacl-main.41",
    pages = "483--498"
}



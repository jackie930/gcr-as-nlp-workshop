+++
title = "Pegasus模型部署"
weight = 0430
chapter = true
pre = "<b>4.3</b>"
+++

首先打包镜像并推送

```
!sh build_and push.sh pegasus-hp
```
然后部署一个预置的endpoint



```shell script
#注意修改：847380964353.dkr.ecr.ap-northeast-1.amazonaws.com/pegasus-hp为自己对应的
%cd endpoint

!python create_endpoint.py \
--endpoint_ecr_image_path "847380964353.dkr.ecr.ap-northeast-1.amazonaws.com/pegasus-hp" \
--endpoint_name 'pegasus' \
--instance_type "ml.p3.2xlarge"
%cd ..
```
输出
```
model_name:  pegasus
endpoint_ecr_image_path:  847380964353.dkr.ecr.ap-northeast-1.amazonaws.com/pegasus-hp
<<< Completed model endpoint deployment. pegasus
```

![](../pics/02pegasus/14.png)

当状态变为`InService`即代表部署完成

在部署结束后，看到SageMaker控制台生成了对应的endpoint,可以使用如下客户端代码测试调用

```python
%%time 

from boto3.session import Session
import json
df=pd.read_csv('./data/hp/summary/news_summary_cleaned_small_test.csv')
print('原文:',df.loc[0,'text'])
print('真实标签:',df.loc[0,'headlines'])
data={"data": df.loc[0,'text']}
session = Session()
    
runtime = session.client("runtime.sagemaker")
response = runtime.invoke_endpoint(
    EndpointName='pegasus',
    ContentType="application/json",
    Body=json.dumps(data),
)

result = json.loads(response["Body"].read())
print (result)
```

结果如下
```
{'result': '[SEP] [CLS] Germany accuses Vietnam of kidnapping asylum seeker  [SEP]', 'infer_time': '0:00:02.948025'}
CPU times: user 169 ms, sys: 30.9 ms, total: 200 ms
Wall time: 3.42 s

```


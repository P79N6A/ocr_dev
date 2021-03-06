## Introduction
This project implements an end to end ocr solution built on top of recent deep learning based text detection (CTPN) and recognizition (CRNN) methods. 
The solution is readily deployable via Flask REST APIs.

This is a research project at this moment, and the code here is NOT ready for production usage. 
But we will get it there though (hopefully soon) ...

## Performance

### Accuray

Text detection performs reasonably well on short snippet of document, either text 
or table. See samples under `data` directory.

Text recognition performs well on correctly detected text block. 
CER is under 10% evaluated on limited text documents.

### Speed

Right now the service is very slow - it takes minutes to process a short text snippet.
Very limited speed up is observed when using the Titan Xp GPU, which is at least 10x
slower than a P100 or V100.

Most time is spent on the CTC decoder of the recognizer, which is by nature a sequential process.
Text blocks are executed one by one in a linear fashion. 

## Requirements

The system requires the following,
```buildoutcfg
python >= 3.6
tensorflow >= 1.12 < 2.0
```
More requirement see ``requirements.txt``.

## Usage

We provide our service via REST APIs. Service can be requested through `http POST` method.
 
### Example
Service request (on gbox1):
```
curl -F image=@'data/text_small.jpg' http://localhost:5000/ocr 
```

Response:
```json
{"result": 
[{"bbox": {"x0": 16, "y0": 75, "x1": 1200, "y1": 75, "x2": 1200, "y2": 106, "x3": 16, "y3": 106}, 
  "text": "保证，并出具包含审计意见的审计报告。含理保证是高水平的保证，但并不能保证按照审", 
   "id": "c99355fa-df22-49b4-b3d9-92acc593ec94"}, 
 {"bbox": {"x0": 16, "y0": 230, "x1": 336, "y1": 230, "x2": 336, "y2": 260, "x3": 16, "y3": 260}, 
  "text": "通常认为错报是重大的。", "id": "d12a0633-4416-4eb6-a347-797eae20e7ff"}, 
 {"bbox": {"x0": 16, "y0": 125, "x1": 1200, "y1": 125, "x2": 1200, "y2": 160, "x3": 16, "y3": 160}, 
  "text": "计准则执行的审计在某一重大错报存在时总能发现。错报可能由于舞弊或错误导致，如果", 
   "id": "6b219b90-c39e-453e-8595-498788c8af10"}, 
 {"bbox": {"x0": 16, "y0": 178, "x1": 1200, "y1": 178, "x2": 1200, "y2": 212, "x3": 16, "y3": 212}, 
  "text": "合理预期错报单独或汇总起来可能影响财务报表使用者依据财务报表作出的经济决策，则", 
   "id": "a842391c-5390-4332-ae54-5e666b64fe03"}, 
 {"bbox": {"x0": 64, "y0": 23, "x1": 1200, "y1": 23, "x2": 1200, "y2": 57, "x3": 64, "y3": 57}, 
  "text": "我们的目标是对财务报表整体是否不存在由于舞弊或错误导致的重大错报获取合理", 
  "id": "51392031-1bf8-4024-b2a1-09633d9e2a81"}], 
  "resized_im": "/tmp/bf558511-0826-4300-a8bc-44cb7b57e564.jpg"}
```
`resized_im` gives the location of the resized image on the host machine, for debugging purpose.

## Service Deployment
In case you would like to deploy your own service, please do the following,
```shell
cd ocrservice
./devel.sh start
```
you can config your deployment there.

## Model Zoos
We will release pretrained model zoos for different types of doucment and languages 
soon ...

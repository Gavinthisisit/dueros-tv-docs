## 通用识图接口文档
#### 整体逻辑
1. 用户发query：帮我看看这是什么花
2. 服务端返回ImageRecognizer.StartRecognize的directive指令, 包含type参数：flower
```
    {
       "header": {
            "namespace":  "ImageRecognizer",
            "name":  "StartRecognize",
            "message_id": "message_id-1344"
        },
        "payload": {
            "type":"flower"，
            "direction":"",
            "offset":""
        }
    }
```
  a.用户发query：右边第一个女人是谁
  b.服务端返回ImageRecognizer.StartRecognize的directive指令, 包含type参数：female
```
    {
       "header": {
            "namespace":  "ImageRecognizer",
            "name":  "StartRecognize",
            "message_id": "message_id-1344"
        },
        "payload": {
            "type":"female"，
            "direction":"left",
            "ffset":"1"
        }
    }
```
3. 客户端拍照（或者截图、让用户选图片）后发起ImageRecognizer.Recognize 的event, 包含type参数：female，方向参数：position,位置参数：offsite,url参数：图片地址，message_id是步骤2中的message_id
```
    {
        "device_event":{
            "header": {
                "namespace":  "ImageRecognizer",
                "name":  "Recognize",
                "message_id": "message_id-1344"
            },
            "payload": {
                "type":"female", //可选
                "direction"："left"，
                "offset":"1"
                "url":"http://f10.baidu.com/it/u=2640880481,1295783988&fm=72"
            }
        }
    }
```
ps: 注意发起的请求是个event, 需要放在device_event里面

4. 度秘收到结果后返回resource数据
```
    "resource":{
        "face":{
            "resultData":[
                {
                    "location":{
                        "left":99,
                        "top":0,
                        "width":187,
                        "height":184
                    },
                    "name":"周杰伦",
                    "thumb":"http://10.94.243.177:8222/zhou.jpg",
                    "baikeUrl":"http://baike.baidu.com/item/%E5%91%A8%E6%9D%B0%E4%BC%A6/129156",
                    "description":"周杰伦（JayChou），1979年1月18日出生于台湾省新北市，中国台湾流行乐男歌手、音乐人、演员..."，
                    "flag":1         //标记该人物是否为所找人物，{1:是query所找人物，2:不是query 所找人物，-1:识别出人脸，但未识别出该人物是谁}
                },
                {
                        "name": "",
                        "thumb": "http://10.94.243.177:8222/image_recognize/192.jpg",
                        "baike_image": "",
                        "baikeUrl": "",
                        "description": "",
                        "detail": "",
                        "location": {
                            "left": 677,
                            "top": 123,
                            "width": 140,
                            "height": 137
                        },
                        "flag": -1
                 }
            ],
            "resultNum":2
        }

```
#### type可能的类型有如下四种
人脸识别:face/female/male
花卉识别: flower
logo识别: logo
汽车识别: car

#### direction可能的类型有如下五种：
前后左右中：front/behind/left/right/middle（默认值为left）

#### offset取值范围是（1~10）：
默认值为1


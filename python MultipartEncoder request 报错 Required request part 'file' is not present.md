# python MultipartEncoder request 报错 Required request part 'file' is not present



今天在处理一个python 做post请求的小工具，是要post一个文件,由于'Content-Type':'multipart/form-data',代码类似这个样子

```python
multipart_encoder = MultipartEncoder(
  fields={
    "file":
      (i,file,)
  },
  # boundary = '-----------------------------' + "WebKitFormBoundary" + str(random.randint(1e28, 1e29 - 1))
  # boundary='------' + "WebKitFormBoundaryWebKitFormBoundaryNSwAkoutBZPHbZB1"
)
  headers = {
    'Admin-Authorization': '76a22d00e8a742729e586d26c17c761a',
    'Accept': 'application/json, text/plain, */*',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.88 Safari/537.36',
    'Content-Type': 'multipart_encoder.content_type',
    'cookie':"_ga=GA1.1.1286920307.1646305970; JSESSIONID=node099cl7few1efbi3b9wsxm2m4615.node0"
  }
  response = requests.request("POST", url, headers=headers,data=multipart_encoder)

```

在python中提交请求总是返回Required request part 'file' is not present,但是可以看到file名字是对的，并且许多修改springboot文件配置的方法感觉也不对（如https://blog.csdn.net/u013231332/article/details/105624361），因为在postman中上传文件又是正确的。

![image-20220415014125580](http://typora-pc.oss-cn-hangzhou.aliyuncs.com/img/image-20220415014125580.png)

这样看来应该是python代码的问题了，后来看了网上的代码是需要将请求头中的'Content-Type'设置为multipart_encoder.content_type,大致原因应该是每次的Content-Type。比如下面的代码：

```py
from requests_toolbelt import MultipartEncoder
import requests

m = MultipartEncoder(
    fields={'field0': 'value', 'field1': 'value',
            'field2': ('filename', open('file.py', 'rb'), 'text/plain')}
    )

r = requests.post('http://httpbin.org/post', data=m,
                  headers={'Content-Type': m.content_type})
```
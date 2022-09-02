## 1、requests转CURL
```bash
pip install curlify
```

```python
import curlify
import requests

rep = requests.get("https://www.baidu.com")
ret1 = curlify.to_curl(rep.request)
print(ret1)# 这种形式，需要输出成一个二进制文件，后面需要再加上 --output (文件名)
"""
curl -X GET -H 'Accept: */*' -H 'Accept-Encoding: gzip, deflate' -H 'Connection: keep-alive' -H 'User-Agent: python-requests/2.23.0' https://www.baidu.com/
"""

ret2 = curlify.to_curl(rep.request,compressed=True)
print(ret2)
"""
curl -X GET -H 'Accept: */*' -H 'Accept-Encoding: gzip, deflate' -H 'Connection: keep-alive' -H 'User-Agent: python-requests/2.23.0' --compressed https://www.baidu.com/
"""
```



## 2、CURL转requests

```bash
npm install --save curlconverter
```

//curl_test.js

```js
var curlcon = require("curlconverter");

ret = curlcon.toPython("curl --request POST --url https://open.workec.com/auth/accesstoken --header 'cache-control: no-cache' --header 'content-type: application/json' --data '{ 'appId': appId, 'appSecret': 'appSecret'}'")

console.log(ret)
```

```bash
node curl_test.js
```




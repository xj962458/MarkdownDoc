## 1、安装
picgo-core
```bash
npm install picgo -g
picgo install super-prefix  # 时间戳重命名工具
```

## 2、使用
```bash
picgo u
```

## 3、腾讯云COS配置文件
```text
Secretld：AKIDQr89uQuMn3iANL8YmkV5WhcIjupZ5TYN
SecretKey：jEG2by95lCPM3by7H0VmncCXcR4TdOt9
APPID：1301172852
存储空间名：xjfyt-markdown-1301172852
存储区域：ap-shanghai
指定存储路径：markdown_img/
自定义域名：http://doc.xjfyt.top
```

### 4、picgo-core配置文件
config.json
```json
{
  "picBed": {
    "current": "tcyun",
    "uploader": "tcyun",
    "picgoPlugins": {},
    "tcyun": {
      "secretId": "AKIDQr89uQuMn3iANL8YmkV5WhcIjupZ5TYN",
      "secretKey": "jEG2by95lCPM3by7H0VmncCXcR4TdOt9",
      "bucket": "xjfyt-markdown-1301172852",
      "appId": "1301172852",
      "area": "ap-shanghai",
      "path": "markdown_img/",
      "customUrl": "http://doc.xjfyt.top",
      "version": "v5",
      "options": ""
    }
  },
  "picgoPlugins": {
    "picgo-plugin-super-prefix": true
  },
  "picgo-plugin-super-prefix": {
    "fileFormat": "YYYYMMDDHHmmss"
  }
}
```
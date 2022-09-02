## 1、最基础
```python
# 第一步: 访问这个链接
import requests
r = requests.get("https://autoupdate.termius.com/windows/Termius.exe")
# 第二步: 获取返回的文件内容，并写到本地
with open(r"./termius.exe", "wb") as f:
    f.write(r.content)
```

## 2、带进度条
```python
import requests
from tqdm import tqdm

def download(url: str, fname: str):
    # 用流stream的方式获取url的数据
    resp = requests.get(url, stream=True)
    # 拿到文件的长度，并把total初始化为0
    total = int(resp.headers.get('content-length', 0))
    # 打开当前目录的fname文件(名字你来传入)
    # 初始化tqdm，传入总数，文件名等数据，接着就是写入，更新等操作了
    with open(fname, 'wb') as file, tqdm(desc=fname, total=total, unit='iB', unit_scale=True, unit_divisor=1024,) as bar:
        for data in resp.iter_content(chunk_size=1024):
            size = file.write(data)
            bar.update(size)


if __name__ == "__main__":
    # 下载文件，并传入文件名
    url = "https://autoupdate.termius.com/windows/Termius.exe"
    download(url, "haha.exe")

```
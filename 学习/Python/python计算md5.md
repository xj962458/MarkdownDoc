## 1、计算字符串md5
```python
import hashlib

if __name__ == '__main__':
    content = "hello"
    md5hash = hashlib.md5(content)
    md5 = md5hash.hexdigest()
    print(md5)
```


## 2、计算文件md5
```python
import hashlib

if __name__ == '__main__':
    file_name = "./1.py"
    with open(file_name, 'rb') as fp:
        data = fp.read()
    file_md5= hashlib.md5(data).hexdigest()
    print(file_md5)
```

## 3、计算大文件的md5值
```python
import hashlib

def get_file_md5(fname):
    m = hashlib.md5()   #创建md5对象
    with open(fname,'rb') as fobj:
        while True:
            data = fobj.read(4096)
            if not data:
                break
            m.update(data)  #更新md5对象
    return m.hexdigest()    #返回md5对象

if __name__ == '__main__':
    file_name = "mongodb_us.zip"
    file_md5 = get_file_md5(file_name)
    print(file_md5)
```
# npy/npz文件的使用



* npy/npz其实就是numpy数组保存到文件中。

  ```python
  import numpy as np
  
  a=np.random.random(100)
  np.save("./random_data",a) # 将保存为random_data.npy文件
  np.savez("./random_data",a) # 将保存为random_data.npz文件
  
  ## numpy还可以将数据保存至文本文件
  np.savetxt("./random_data.txt",a) # 保存为random_data.txt文件
  np.loadtxt("./random_data.txt") # 读取保存的random_data.txt文件
  ```

  

* npz和npy文件都可以直接使用numpy读写。

```python
import numpy as np

data = np.load('data.npz') # 加载npy/npz文件
data.files  # 查看npy/npz文件的可用属性，单纯的numpy数组无此属性
# ['vocab', 'embedding']

data['vocab'] # 访问vocab数据
data['embedding'] # 访问embedding数据
```



* 训练好的词向量常以此种方式存储

  ```python
  import numpy as np
  a=np.load("./w2v.baidu_encyclopedia.target.word-word.dim300.npz")
  
  word="张三" # 要获取词向量的词
  
  print(a['embedding'][(np.where(a['vocab']==word))]) #打印出对应的词向量
  ```

  


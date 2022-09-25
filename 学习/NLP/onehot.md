- 文本张量表示的方法:
  - one-hot编码
  - Word2vec
  - Word Embedding



onehot编码

```python
# 导入用于对象保存与加载的joblib
import joblib
# 导入keras中的词汇映射器Tokenizer
from keras.preprocessing.text import Tokenizer
# 假定vocab为语料集所有不同词汇集合
vocab = {"周杰伦", "陈奕迅", "王力宏", "李宗盛", "吴亦凡", "鹿晗"}
# 实例化一个词汇映射器对象
t = Tokenizer(num_words=None, char_level=False)
# 使用映射器拟合现有文本数据
t.fit_on_texts(vocab)

for token in vocab:
    zero_list = [0]*len(vocab)
    # 使用映射器转化现有文本数据, 每个词汇对应从1开始的自然数
    # 返回样式如: [[2]], 取出其中的数字需要使用[0][0]
    token_index = t.texts_to_sequences([token])[0][0] - 1
    zero_list[token_index] = 1
    print(token, "的one-hot编码为:", zero_list)

# 使用joblib工具保存映射器, 以便之后使用
tokenizer_path = "./Tokenizer"
joblib.dump(t, tokenizer_path)
```

```bash
鹿晗 的one-hot编码为: [1, 0, 0, 0, 0, 0]
王力宏 的one-hot编码为: [0, 1, 0, 0, 0, 0]
李宗盛 的one-hot编码为: [0, 0, 1, 0, 0, 0]
陈奕迅 的one-hot编码为: [0, 0, 0, 1, 0, 0]
周杰伦 的one-hot编码为: [0, 0, 0, 0, 1, 0]
吴亦凡 的one-hot编码为: [0, 0, 0, 0, 0, 1]

# 同时在当前目录生成Tokenizer文件, 以便之后使用
```



onehot编码使用

```python
# 导入用于对象保存与加载的joblib
# from sklearn.externals import joblib
# 加载之前保存的Tokenizer, 实例化一个t对象
t = joblib.load(tokenizer_path)

# 编码token为"李宗盛"
token = "李宗盛"
# 使用t获得token_index
token_index = t.texts_to_sequences([token])[0][0] - 1
# 初始化一个zero_list
zero_list = [0]*len(vocab)
# 令zero_List的对应索引为1
zero_list[token_index] = 1
print(token, "的one-hot编码为:", zero_list) 
```


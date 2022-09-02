**gensim**是一个Python的自然语言处理库，能够将文档根据TF-IDF，LDA，LSI等模型转换成向量模式，此外，gensim还实现了word2vec，能够将单词转换为词向量。
**三大核心概念：文集（语料）–>向量–>模型**

1. 将原始的文档处理后生成语料库
```python3
from gensim import corpora
import jieba
documents = ['工业互联网平台的核心技术是什么',
            '工业现场生产过程优化场景有哪些']
def word_cut(doc):
    seg = [jieba.lcut(w) for w in doc]
    return seg

texts= word_cut(documents)

##为语料库中出现的所有单词分配了一个唯一的整数id
dictionary = corpora.Dictionary(texts)
dictionary.token2id
from gensim import corpora
import jieba
documents = ['工业互联网平台的核心技术是什么',
            '工业现场生产过程优化场景有哪些']
def word_cut(doc):
    seg = [jieba.lcut(w) for w in doc]
    return seg

texts= word_cut(documents)

##为语料库中出现的所有单词分配了一个唯一的整数id
dictionary = corpora.Dictionary(texts)
dictionary.token2id
```

```text
{'互联网': 0,
 '什么': 1,
 '优化': 7,
 '哪些': 8,
 '场景': 9,
 '工业': 2,
 '平台': 3,
 '是': 4,
 '有': 10,
 '核心技术': 5,
 '现场': 11,
 '生产': 12,
 '的': 6,
 '过程': 13}
```


2. 把文档表示成向量




# 1. corpora 和 dictionary
**corpora是gensim中的一个基本概念，是文档集的表现形式**，也是后续进一步处理的基础。从本质上来说，corpora其实是一种格式或者说约定，其实就是一个二维矩阵

```python3
from gensim import corpora
from collections import defaultdict
documents = ["Human machine interface for lab abc computer applications",
             "A survey of user opinion of computer system response time",
             "The EPS user interface management system",
             "System and human system engineering testing of EPS",
             "Relation of user perceived response time to error measurement",
             "The generation of random binary unordered trees",
             "The intersection graph of paths in trees",
             "Graph minors IV Widths of trees and well quasi ordering",
             "Graph minors A survey"]

# 去掉停用词
stoplist = set('for a of the and to in'.split())
texts = [[word for word in document.lower().split() if word not in stoplist]
         for document in documents]

# 去掉只出现一次的单词
frequency = defaultdict(int)
for text in texts:
    for token in text:
        frequency[token] += 1
texts = [[token for token in text if frequency[token] > 1]
         for text in texts]

dictionary = corpora.Dictionary(texts)   # 生成词典

# 将文档存入字典，字典有很多功能，比如
# diction.token2id 存放的是单词-id key-value对
# diction.dfs 存放的是单词的出现频率
dictionary.save('/tmp/deerwester.dict')  # store the dictionary, for future reference
corpus = [dictionary.doc2bow(text) for text in texts]
corpora.MmCorpus.serialize('/tmp/deerwester.mm', corpus)  # store to disk, for later use
```


#  word2vec模型的保存和加载

## 一、介绍

**gensim**是一个Python的自然语言处理库，能够将文档根据TF-IDF，LDA，LSI等模型转换成向量模式，此外，gensim还实现了word2vec，能够将单词转换为词向量。

Gensim是一个用于从文档中自动提取语义主题的Python库，足够智能，堪比无痛人流。
Gensim可以处理原生，非结构化的数值化文本(纯文本)。Gensim里面的算法，比如Latent Semantic Analysis(潜在语义分析LSA)，Latent Dirichlet Allocation，Random Projections，通过在语料库的训练下检验词的统计共生模式(statistical co-occurrence patterns)来发现文档的语义结构。这些算法是非监督的，也就是说你只需要一个语料库的文档集。
当得到这些统计模式后，任何文本都能够用语义表示(semantic representation)来简洁的表达，并得到一个局部的相似度与其他文本区分开来。

官方网站：https://radimrehurek.com/gensim/index.html.

安装：

```bash
pip install gensim
```



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



**corpora 和 dictionary**

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



##  二、加载预训练词向量

* 预训练词向量集合：[Embedding/Chinese-Word-Vectors: 100+ Chinese Word Vectors 上百种预训练中文词向量 (github.com)](https://github.com/Embedding/Chinese-Word-Vectors)

* GloVe预训练词向量：[GloVe: Global Vectors for Word Representation (stanford.edu)](https://nlp.stanford.edu/projects/glove/)
* 腾讯AI Lab预训练词向量：[腾讯AI Lab，自然语言处理(NLP)研究 (tencent.com)](https://ai.tencent.com/ailab/nlp/zh/)
* 谷歌新闻预训练词向量：[mmihaltz/word2vec-GoogleNews-vectors: word2vec Google News model (github.com)](https://github.com/mmihaltz/word2vec-GoogleNews-vectors)

使用`gensim`加载预训练词向量需要保证文本的最开头一行保存**整个词表的大小和维度**，`GloVe`词向量中不包含，若要加载，需要自行添加。



### 1、加载词向量

```python
from gensim.models.keyedvectors import KeyedVectors

word_vectors=KeyedVectors.load_word2vec_format(word2vec_path=filepath) # 根据问价加载词向量
# word2vec_path：词向量文档所在的地址，比如上述腾讯词向量下载后的本地地址。

# binary：如果为真，表示数据是否为二进制word2vec格式。默认为False。谷歌新闻词向量为bin文件，此处应为True
```



```python
# 加载词向量文件
wv_from_text = gensim.models.KeyedVectors.load_word2vec_format(vec_path, binary=False)

# 如果每次都用上面的方法加载，速度非常慢，可以将词向量文件保存成bin文件，以后就加载bin文件，速度会变快
wv_from_text.init_sims(replace=True)
wv_from_text.save(vec_path.replace(".txt", ".bin")) # 转化为bin文件
# 之后可以用下面的方式加载词向量
wv_from_text = gensim.models.KeyedVectors.load(embed_path, mmap='r')
```



### 2、获取所有词、向量并构建idx2word和word2idx

```python
# 获取词表
#vocab = wv_from_text.vocab # 已弃用
word2id = wv_from_text.key_to_index # 获取word to id,字典
vocab = wv_from_text.index_to_key  # 获取词表，列表

# 获取所有向量
word_embedding = wv_from_text.vectors

# 将向量和词保存下来
word_embed_save_path = "emebed"
word_save_path = "word.ckpt"
np.save(word_embed_save_path, word_embedding) # 使用numpy保存为npy文件
np.savez(word_embed_save_path, word_embedding) # 使用numpy保存为npz文件
np.savetxt(word_embed_save_path, word_embedding) # 使用numpy保存为文本文件
pd.to_pickle(vocab, word_save_path)   # 词biao

# 加载保存的向量和词
weight_numpy = np.load(file="emebed.npy")
vocab = pd.read_pickle(word_save_path)
word2idx = {word: idx for idx, word in enumerate(vocab)}
idx2word = {idx: word for idx, word in enumerate(vocab)}
pd.to_pickle(word2idx, "word2idx.ckpt")
pd.to_pickle(idx2word, "idx2word.ckpt")

# 加载向量用户pytorch模型
embedding =torch.nn.Embedding.from_pretrained(torch.FloatTensor(weight_numpy))

```



### 3、其他方法

```python
wv_from_text.most_similat(word,topn) # 找出word的相近词
# word为单词
# topn指定显示多少个

wv_from_text.similarity(word1,word2) # 比较word1和word2余弦相似度

# 获取某个词的词向量,两种方法
wv_from_text[word]
wv_from_text.get(word)
```




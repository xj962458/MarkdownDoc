## 一、认识
```python
from paddlenlp.embeddings import TokenEmbedding
```

`TokenEmbedding`是`paddlenlp`框架中一个可以加载预训练词向量模型，并可以将词转化为词向量的一个API



* 调用`TokenEmbedding`

```python
from paddlenlp.embeddings import TokenEmbedding
```



**`TokenEmbedding()`参数**

- `embedding_name`
  将模型名称以参数形式传入TokenEmbedding，加载对应的模型。默认为`w2v.baidu_encyclopedia.target.word-word.dim300`的词向量。

- `unknown_token`
  未知token的表示，默认为[UNK]。

- `unknown_token_vector`
  未知token的向量表示，默认生成和embedding维数一致，数值均值为0的正态分布向量。

- `extended_vocab_path`
  扩展词汇列表文件路径，词表格式为一行一个词。如引入扩展词汇列表，trainable=True。

- `trainable`
  Embedding层是否可被训练。True表示Embedding可以更新参数，False为不可更新。默认为True。

  
```bash
# 初始化TokenEmbedding，预训练embedding未下载时会自动下载并加载数据
token_embedding = TokenEmbedding(embedding_name="w2v.baidu_encyclopedia.target.word-word.dim300")

# 查看token_embedding详情
print(token_embedding)
```

```bash
[2021-11-10 21:42:13,213] [    INFO] - Loading token embedding...
W1110 21:42:18.557029  1415 device_context.cc:447] Please NOTE: device: 0, GPU Compute Capability: 7.0, Driver API Version: 10.1, Runtime API Version: 10.1
W1110 21:42:18.562464  1415 device_context.cc:465] device: 0, cuDNN Version: 7.6.
[2021-11-10 21:42:23,903] [    INFO] - Finish loading embedding vector.
[2021-11-10 21:42:23,906] [    INFO] - Token Embedding info:             
Unknown index: 635963             
Unknown token: [UNK]             
Padding index: 635964             
Padding token: [PAD]             
Shape :[635965, 300]


Object   type: TokenEmbedding(635965, 300, padding_idx=635964, sparse=False)             
Unknown index: 635963             
Unknown token: [UNK]             
Padding index: 635964             
Padding token: [PAD]             
Parameter containing:
Tensor(shape=[635965, 300], dtype=float32, place=CUDAPlace(0), stop_gradient=False,
       [[-0.24200200,  0.13931701,  0.07378800, ...,  0.14103900,
          0.05592300, -0.08004800],
        [-0.08671700,  0.07770800,  0.09515300, ...,  0.11196400,
          0.03082200, -0.12893000],
        [-0.11436500,  0.12201900,  0.02833000, ...,  0.11068700,
          0.03607300, -0.13763499],
        ...,
        [ 0.02628800, -0.00008300, -0.00393500, ...,  0.00654000,
          0.00024600, -0.00662600],
        [-0.01989385, -0.02005955,  0.01555019, ...,  0.00248810,
         -0.02033536, -0.01693229],
        [ 0.        ,  0.        ,  0.        , ...,  0.        ,
          0.        ,  0.        ]])

```



## 二、使用

* 直接调用，将词转化为词向量

```python
test_token_embedding = token_embedding.search("中国")
print(test_token_embedding)
```

* 用于判断两个词之间的距离

  ```python
  score1 = token_embedding.cosine_sim("女孩", "女人")
  ```

  

* **在神经网络中组建word-embedding层，即将词转化为词向量**

  * 但在神经网络中，传入的不是一个词，而是一个Tensor

  * 调用时不止传入预训练词向量名称：`embedding_name`

  * 还要传入扩展词表`extended_vocab_path`

  * 未知词标签表示：`unknown_token`，此处`token`可理解为标签

  * 在数据流向中使用，接受的x是一个batch，一个batch中包含若干条句子（向量表示），字符用id代替

    ```python
    class BiGRUWithCRF(nn.Layer):
        def __init__(self, emb_size, hidden_size, label_num):
            super(BiGRUWithCRF, self).__init__()
            self.word_emb = TokenEmbedding(embedding_name="w2v.baidu_encyclopedia.target.word-word.dim300",extended_vocab_path='./data/words.char', unknown_token='OOV')
            self.gru = nn.GRU(emb_size, hidden_size, num_layers=2,
                              direction='bidirectional')  # 定义GRU层
            self.fc = nn.Linear(hidden_size * 2, label_num + 2)  # BOS EOS
            self.crf = LinearChainCrf(label_num)  # CRF层
            self.decoder = ViterbiDecoder(self.crf.transitions)  # 解码
    
        def forward(self, x, lens):  # 定义数据流向
            embs = self.word_emb(x)  # 将输入的id转化为词向量
            output, _ = self.gru(embs)  # 经过GRU层
            output = self.fc(output)
            _, pred = self.decoder(output, lens)
            return output, lens, pred  # return 句向量,句子长度,预测的标签值
    ```



## 三、paddlenlp.seq2vec


情感分析是自然语言处理领域一个老生常谈的任务。句子情感分析目的是为了判别说者的情感倾向，比如在某些话题上给出的的态度明确的观点，或者反映的情绪状态等。情感分析有着广泛应用，比如电商评论分析、舆情分析等。

<p align="center">
<img src="https://ai-studio-static-online.cdn.bcebos.com/febb8a1478e34258953e56611ddc76cd20b412fec89845b0a4a2e6b9f8aae774" hspace='10'/> <br />
</p>


paddlenlp.seq2vec

句子情感分析的关键技术是如何将文本表示成一个**携带语义的文本向量**。随着深度学习技术的快速发展，目前常用的文本表示技术有LSTM，GRU，RNN等方法。
PaddleNLP提供了一系列的文本表示技术，集成在`seq2vec`模块中。

[`paddlenlp.seq2vec`](https://github.com/PaddlePaddle/models/tree/develop/PaddleNLP/paddlenlp/seq2vec) 模块的作用是将输入的序列文本，表示成一个语义向量。

<center><img src="https://ai-studio-static-online.cdn.bcebos.com/bbf00931c7534ab48a5e7dff5fbc2ba3ff8d459940434628ad21e9195da5d4c6" width="700" height="350" ></center>
​																							图1：paddlenlp.seq2vec示意图



## 四、**`seq2vec`模块**

**`seq2vec`模块**

* 输入：文本序列的Embedding Tensor，shape：(batch_size, num_token, emb_dim)
* 输出：文本语义表征Enocded Texts Tensor，shape：(batch_sie,encoding_size)
* 提供了`BoWEncoder`，`CNNEncoder`，`GRUEncoder`，`LSTMEncoder`，`RNNEncoder`等模型
	- `BoWEncoder` 是将输入序列Embedding Tensor在num_token维度上叠加，得到文本语义表征Enocded Texts Tensor。     
    
    - `CNNEncoder` 是将输入序列Embedding Tensor进行卷积操作，在对卷积结果进行max_pooling，得到文本语义表征Enocded Texts Tensor。   
    
    - `GRUEncoder` 是对输入序列Embedding Tensor进行GRU运算，在运算结果上进行pooling或者取最后一个step的隐表示，得到文本语义表征Enocded Texts Tensor。     
    
    - `LSTMEncoder` 是对输入序列Embedding Tensor进行LSTM运算，在运算结果上进行pooling或者取最后一个step的隐表示，得到文本语义表征Enocded Texts Tensor。   
    
    - `RNNEncoder` 是对输入序列Embedding Tensor进行RNN运算，在运算结果上进行pooling或者取最后一个step的隐表示，得到文本语义表征Enocded Texts Tensor。
    
  
* `seq2vec`提供了许多语义表征方法，那么这些方法有什么特点呢？
	1. `BoWEncoder`采用Bag of Word Embedding方法，其特点是简单。但其缺点是没有考虑文本的语境，所以对文本语义的表征不足以表意。
    2. `CNNEncoder`采用卷积操作，提取局部特征，其特点是可以共享权重。但其缺点同样只考虑了局部语义，上下文信息没有充分利用。

  <center>
    <img src="https://ai-studio-static-online.cdn.bcebos.com/2b2498edd83e49d3b017c4a14e1be68506349249b8a24cdaa214755fb51eadcd" width="400" height="150" >
  </center>
  <center>
    图2：卷积示意图
  </center>
  </br>

	3. `RNNEnocder`采用RNN方法，在计算下一个token语义信息时，利用上一个token语义信息作为其输入。但其缺点容易产生梯度消失和梯度爆炸。

    <p align="center">
    <img src="http://colah.github.io/posts/2015-09-NN-Types-FP/img/RNN-general.png" width = "40%" height = "20%"  hspace='10'/> 
    </p>
    <center>
      图3：RNN示意图
    </center>
    </br>

	4. `LSTMEnocder`采用LSTM方法，LSTM是RNN的一种变种。为了学到长期依赖关系，LSTM 中引入了门控机制来控制信息的累计速度，包括有选择地加入新的信息，并有选择地遗忘之前累计的信息。

  <p align="center">
    <img src="https://ai-studio-static-online.cdn.bcebos.com/a5af1d93c69f422d963e094397a2f6ce978c30a26ab6480ab70d688dd1929de0" width = "50%" height = "30%"  hspace='10'/> 
  </center>
  <center>
    图4：LSTM示意图
  </center>
  </br>

	5. `GRUEncoder`采用GRU方法，GRU也是RNN的一种变种。一个LSTM单元有四个输入 ，因而参数是RNN的四倍，带来的结果是训练速度慢。GRU对LSTM进行了简化，在不影响效果的前提下加快了训练速度。

<p align="center">
<img src="https://ai-studio-static-online.cdn.bcebos.com/fc848bc2cb494b40ae42af892b756f5888770320a1fa42348cec10d3df64ee2f" width = "40%" height = "25%"  hspace='10'/> 
  <br />
</p><br><center>图5：GRU示意图</center></br>

​    
关于CNN、LSTM、GRU、RNN等更多信息参考：
* Understanding LSTM Networks: [https://colah.github.io/posts/2015-08-Understanding-LSTMs/](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)
* Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling:[https://arxiv.org/abs/1412.3555](https://arxiv.org/abs/1412.3555)
* A Critical Review of Recurrent Neural Networks
for Sequence Learning: [https://arxiv.org/pdf/1506.00019](https://arxiv.org/pdf/1506.00019)
* A Convolutional Neural Network for Modelling Sentences: [https://arxiv.org/abs/1404.2188](https://arxiv.org/abs/1404.2188)



## 五、nn.Embedding

```python
torch.nn.Embedding(num_embeddings, embedding_dim,padding_idx)
paddle.nn.Embedding(num_embeddings,embedding_dim,padding_idx)

# 两者类似，功能也近乎一样，甚至参数也一样
# num_embeddings:词表大小
# embedding_dim:词向量维度
# padding_idx：填充的数，默认为0
```

在RNN模型的训练过程中，需要用到词嵌入，而torch.nn.Embedding就提供了这样的功能。我们只需要初始化torch.nn.Embedding(n,m)，n是单词数，m就是词向量的维度。**一开始embedding是随机的，在训练的时候会自动更新。**

自然语言中使用批处理时候, 每个句子的长度并不一定是等长的, 这时候就需要对较短的句子进行padding, 填充的数据一般是0, 这个时候, 在进行词嵌入的时候就会进行相应的处理, nn.embedding会将填充的映射为0。其中padding_idx就是这个参数, 这里以3 为例, 也就是说补长句子的时候是以3padding的

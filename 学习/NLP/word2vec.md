# word2vec理解
* word2vec最终训练得到的是一个矩阵，即参数矩阵，该矩阵中存储着所有的词向量信息
* word2vec需要借助one-hot编码实现
* word2vec有两种模式：CBOW和skip-gram
     * CBOW：由两端预测中间，即输入为两端，输出为中间
     * skip-gram：由中间预测两端，即输入为中间，输出为中间
* word2vec和word embedding不是一个东西，word2vec是word embedding中的一种



# 训练wor2vec过程
1. 找出语料中的所有词，按出现频次排序；
2. 根据找出的词对其进行one-hot编码；
3. 选取word2vec的模式
4. 设置窗口大小
5. 训练


# CBOW过程
![](http://doc.xjfyt.top/markdown_img/%E6%89%AB%E6%8F%8F%E5%85%A8%E8%83%BD%E7%8E%8B%202022-08-12%2010.00.jpg)
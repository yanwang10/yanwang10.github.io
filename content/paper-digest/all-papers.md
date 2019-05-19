---
title: "论文短评"
date: 2019-05-18T16:28:17-07:00
draft: true
categories:
  - 我的
tags:
  - 论文 科研
summary: "不时看些论文，记录一些观点和启发，就算是狗熊掰棒子，也会留下些痕迹不是。"
---

有点类似 Paperweekly 的论文推介，不过当然是更侧重我个人的关注点。论文的组织结构比较随性。可能有些描述和认识不够准确（希望不至于错的离谱），请谨慎参考。

### BERT 一家亲

#### BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding

抛开具体技术不谈，BERT 跟当年的 word2vec 有很多相似之处。两者都针对语言表示学习提出了各自的方案，用巧妙地结构或者建模方式得到了通用性比较强的模型，然后在各个主要 NLP 任务上刷一波 SOTA。

在我看来，BERT 的成功可能主要归功于以下几点。

##### Word piece

这个切词方式不是 BERT 的首创，2012 年 Google 发的 Japanese and Korean voice search 一文中就详细介绍了这个方法，主要动机是解决日文词表太大的问题（其实中文分词之后也有这个问题），后来 Google 在做神经网络机器翻译的时候，也用这个手法来解决德分词的困难 (Neural Machine Translation of Rare Words with Subword Units)。这个手法的好处有以下几点：

* 对于英文或者类似英文的语言，模型习得的 embeddings 介于 character-level 和 word-level 之间，既保留了类似 character-level embedding 的平滑，对 OOV 有一定的处理能力，又保证高频的词能够得到单独的 embedding，缩短作为序列的上下文的长度，容易看到更多上下文信息。

* 词表的大小容易控制，而且词表选择的过程考虑了整个语料的信息，应该比只看词频的选择更加科学。

* 在一些语言中间有部分 word piece 是通用的，比如繁体中文和日文中的汉字。这样一来如果同时处理多种语言，词表的总大小会小于各个语言单独词表的叠加，而且模型可能能学到一些跨语言的信息（同样的 word piece 出现在不同语言中）。

#### Transformer 结构

就一个字，大力出奇迹。Self-attention 就像一种新的全连接层，直接对整个序列进行处理，特征随便抽，再加上巧妙的 position embedding，序列中各个元素的相对位置也可以被表示出来。虽然这个结构早就在颇具争议的文章 Attention Is All You Need 中提出，不过能一次怼上这么多层、这么多 attention head 也是没谁了，参数多就是可以为所欲为。估计之后会出一波论文来专门研究 BERT 模型的压缩或者近似。训练费时费力也就罢了，如果 inference 的时候也这么大计算量，层与层之间还不能并行，对于计算成本和延迟都有很大的负担，应用于产品可能会有不少麻烦。

Transformer 结构还有一个好处，就是可以直接预留一个元素（文章中用 [CLS] 表示）作为整个序列的表示结果，比传统的基于 RNN 的网络更方便。据说在一些任务里，直接用 Bi-LSTM 首尾的隐态作为整个序列的表示的话，效果并不好，还需要再学一个带 attention 的 pooling 模块，把各个位置的隐态结合起来。

#### 预训练任务

这一点终于算是 BERT 的创新了。用 masked language modeling 以及 next sentence prediction 两个任务联合预训练，在用未标注文本训练词向量的时候同时考虑前后文的信息，学出整个序列的表示以及各个位置的表示，同时兼容 sequence labeling 和 sequence classification 的多种任务，特别是因为可以找到两个句子之间的交互，也可以用于一些需要 two-tower 模型的任务（比如 QA 和 IR 的一些具体任务）。比起把两个序列分别做 embedding 再接另一个网络评估相似度（或者直接余弦相似度），把两个句子拼在一起学习 attention 显然在模型结构和训练过程上更简单（也许还更高效）。

当然，要想用 BERT，还是有很多细节问题要解决的，比如用了 word piece 之后处理 sequence labeling 的任务，一个词（或者命名实体的名字）被切成的几个 piece 得到不同的 label 怎么办？如果用 BERT 直接替代 two-tower 模型，那么每对输入都要单独计算一遍，QA 系统的计算量会不会爆炸？等等等等。
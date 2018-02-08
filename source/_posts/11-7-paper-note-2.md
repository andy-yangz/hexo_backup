---
title: 论文笔记：Understanding Hidden Memories of Recurrent Neural Networks
date: 2017-11-07 16:03:40
categories: Paper Notes
description:
tags:
- RNN
- NLP
---

#### 一、文章有何贡献？

主要贡献有三。

1. 设计了RNNVis系统，这是一个用于理解、比较、和诊断自然语言处理任务中RNN的视觉分析系统。
2. 提出一个用期望反应(expected response)来链接RNN中的隐藏状态和文本信息的新技术。
3. 提出了一个图像为基础的序列可视化设计，来分析句子级别的RNN的行为。

#### 二、本文研究问题有何价值？

现在关于神经网络的很多东西我们还并不是很理解，所以都把它当成是一个黑匣子。当然也有过一些对神经网络中捕捉的表达的探索，但主要集中在计算机视觉方面。

主要是因为序列语言中的规则难以视觉化，而且对于其中RNN的机制也很多地方没有理解，这篇论文有望让我们进一步理解RNN黑箱子里面的一些行为，然后对RNN结构进行提高。

#### 三、研究问题有什么挑战？

1. ​

#### 四、本文解决思路？

1. 对于第一个问题，就如问题，把生成器的StackLSTM替换成NMT的解码器，然后让NMT的解码器作为生成器来和RNNG的其他两个部件，Stack和Action部件进行互动。如每次只有当，Action部件生成Shift指令的时候，才让NMT的解码器预测下一个单词，然后推入Stack中去。

   ![RNNG 模型](http://upload-images.jianshu.io/upload_images/4787675-e6faea7b84f4c40b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   ​

   关于这个部分具体可以参考RNNG的原论文，重点读Generator Transition那一节。[Dyer et al., Recurrent Neural Network Grammars](https://arxiv.org/abs/1602.07776)

2. 对于第二个问题，就是找个比较好的parser，对已有的平行语料库进行标注，然后再训练，也没有太多新颖的地方，却特意提到distillation。

#### 五、讨论

虽然这篇论文的idea并不是让人惊喜，而且也没有给出模型的图片，还得跑去找RNNG的论文来看。

其中一些具体的实现细节也没提到， 比如是直接把NMT的解码器当做RNNG的生成器，然后进行Joint Training。还是说将Action和Stack的一些信息也作为输入信号输入了NMT的解码器呢，使得NMT的解码器可以进一步的利用获得的语法知识。就如论文 [Wu, Shuangzhi, et al. Sequence-to-Dependency Neural Machine Translation 2017](http://www.aclweb.org/anthology/P17-1065)里面一样。

如果没有提到就是没有利用的话，那么这个思路不妨是个继续探索的方向。


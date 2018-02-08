---
title: 论文笔记:Learning to Parse and Translate Improves Neural Machine Translation
date: 2017-11-07 15:06:16
categories: Paper Notes
description: Using dependency tree
tags:
- Neural Machine Translation
- Linguistic Structure
---

#### 一、文章有何贡献？

提出了一个叫做**NMT (Neural Machine Translation) + RNNG (Recurrent Neural Network Grammars)**的混合模型，这个模型如字面意思将注意力机制的NMT和RNNG进行了结合，可以同时进行dependency parsing和翻译。 

#### 二、本文研究问题有何价值？

也是将语法知识利用进NMT模型的一篇论文之一，与基线相比提高了一些性能。

#### 三、研究问题有什么挑战？

大概有两条挑战。实际上感觉挑战并不是很大，只是将之前的RNNG的生成器部分替换成了NMT的解码器。

1. 如何将原来的RNNG模型中的生成器的StackLSTM替换成NMT的解码器。
2. 没有标注好parsing的平行语料库。

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


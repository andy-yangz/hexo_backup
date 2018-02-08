---
title: 论文笔记：Improved Neural Machine Translation with a Syntax-Aware Encoder and Decoder
date: 2017-11-06 17:55:44
categories: Paper Notes
description:
tags:
- Neural Machine Translation
- Linguistic Structure
---

#### 1.文章有何贡献？

一、提出了bidirectional tree encoder，可以同时学会译出语言的序列表达和树状表达特征。之后，解码器利用这些信息进行解码。

二、提出了tree-coverage model, 使得注意力机制更有效地利用了译出语言的句法结构。

#### 2.本文研究的问题有何价值？

一，在encoder端，对*Eriguchi et al. (2016)*的树状解码器进行了强化，改成了双方向的，不仅有bottom-up encoder，还有up-down encoder。与基线NMT模型相比，性能有了很大的提升。

二、在decoder端，利用*Tu et al. (2016)*的coverage模型，将译出端的句法结构整合进注意力机制中去。这种处理，使得性能得到更大的提升。

#### 3.研究问题有什么挑战？

一、如何充分编码译出端的句法信息？较之之前已有的树状编码器(tree encoder).

![Tree Encoder](http://upload-images.jianshu.io/upload_images/4787675-ac021456b88eaeed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

二、直接将树状编码器的各个节点，输入注意力机制后，发现会过度地集中于父节点，而忽略了子节点。导致的结果是，对某些部分的句子进行了重复翻译。如何解决这个问题。

![Over attention to parent nodes](http://upload-images.jianshu.io/upload_images/4787675-165ce74906fae653.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4.本文的解决思路？

对于第一个问题，提出**bidirectional tree encoder**。

当下面的叶节点按照序列顺序进行完了双向LSTM后，拼接特征，输入上一级的父节点，然后以此类推，到达最后的根节点。这是，原来的树状编码器的思路，也就是bottom-up。这样我们每个节点，获得了一个向上的特征向量。

![Bi-directional Tree Encoder](http://upload-images.jianshu.io/upload_images/4787675-76377f6fdc6bdb0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而本文更进一步，将bottom-up的结果输入根节点，然后再从上到下，到达各个子节点。这也就是top-down。这样每个节点又获得了一个向下的特征向量。

之后将向上和向下的拼接，就是我们需要的双向特征了。

对于第二个问题，提出了**tree-coverage model**。

其实所谓的coverage就是，在计算当前时序的attention时，考虑之前时序的attention。

![Attention balanced](http://upload-images.jianshu.io/upload_images/4787675-1ed9e9f543969536.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

具体细节公式参考论文。 

[Improved Neural Machine Translation with a Syntax-Aware Encoder and Decoder](https://arxiv.org/pdf/1707.05436.pdf)
---
title: 论文笔记：Sequence-to-Dependency Neural Machine Translation
categories: Paper Notes
description:
tags:
- Neural Machine Translation
- Linguistic Structure
---

1. #### 文章有什么贡献？ 

  提出了一种新的 Sequence-to-Dependency Neural Machine Translation (SD-NMT)的方法，来将目标语言句法知识利用进了NMT系统，相比起没有使用句法知识的基准NMT系统，性能得到了相对的提高。

2. #### 研究的问题有何价值？

  目前的NMT系统主要是直接用线性RNN来进行Seq2Seq，但是这样的系统对于捕捉不明显的长距离词的依存还是有很大难度的。因此在解码的时候，将句法知识考虑进解码器中后，可以提高翻译结果语法的正确性，并且也可以利用局部依存信息来生成之后的词语。

3. #### 研究问题有什么挑战？

  一，如何利用RNN来构建句法结构；
  二，如何在一个神经网络中，有效地同时进行词语生成，还有句法结构的构建；
  三，如何有效地利用目标语言的句法背景，来帮助词语的生成。

4. #### 本文的解决思路？

![](http://upload-images.jianshu.io/upload_images/4787675-594358846a37bbbd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一，利用了两个RNN网络，Action RNN 和 Word RNN，分别进行词语生成和句法结构的构建。Action RNN 利用了transition-based dependency parsing (基于转换的依存句法分析) 中的 arc-standard shift-reduce algorithm 算法，来生成构建所需依存结构的动作。而同时因为两个 RNN 生成的的序列长度不一致，所以 Word RNN 利用了些技巧，使得它能够参考 Action RNN 的结果输出词语，或者保持不变以和 Action RNN 的时序保持一致。

![](http://upload-images.jianshu.io/upload_images/4787675-d49d4d5947999b68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


二，通过定义生成依存结构时的栈顶部两个词语，最左和最右修饰语的一元和二元语言特征，生成相对当前词汇的局部依存背景。之后将这个背景与 Word RNN 的输出结合起来，帮组生成新的词汇。
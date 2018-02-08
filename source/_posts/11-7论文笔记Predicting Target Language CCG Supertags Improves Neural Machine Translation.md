---
title: 论文笔记:Predicting Target Language CCG Supertags Improves Neural Machine Translation
date: 2017-11-07 22:14:14
categories: Paper Notes
description:
tags:
- NMT
- Linguistic structure 
---

#### 一、文章有什么贡献？

1. 主要共享是提出了一个新的思路，以CCG (Combinatory Categorial Grammar) Supertag的形式将句法信息引入了，NMT（神经机器翻译）的解码器端，对NMT的性能有了一定提高。
2. 用两种方式将CCG Supertag任务引入解码器，一种是直接插入输出的序列，一种是利用了多任务学习，对多任务学习的研究也有一些贡献。
3. 展示了不光是解码器，当同时在编码器端输入语言学信息的时候，性能得到进一步提高。
4. 对其中更多细节，如句子种类还有句子长度也进行了详细的分析。进一步理解，引入语言学信息后对NMT系统的影响。

#### 二、本文研究问题有什么价值？

首先引入CCG Supertag来对NMT的解码器加入语法学信息，而且证明了这种情况下直接插入输出序列比多任务学习的性能要好。当然主要还是证明了，语言学对NMT系统的影响。

#### 三、研究问题有什么挑战？

大概就是如何将CCG supertag的语法信息引入编码器端吧。

之后很多都是对系统的详细分析了。

#### 四、本文解决思路？

本文提出了两个解决思路。

1. 一个是interleaving，也就是将CCG supertag直接相间插入目标语言的序列中去，也就是将输出序列长度增加一倍，一个单词一个相应的tag。如这样 $y^{'}=y_1^{tag},y_1^{word},...,y_T^{tag},y_T^{word}$ .

   然后就把这个当做是原来的目标语言序列，进行解码预测。

   ![interleaving](http://upload-images.jianshu.io/upload_images/4787675-9a98cd36e56004d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 还有一种思路是利用多任务学习(Multi-task Learning)，两个解码器分别用来翻译和输出CCG supertag，这两个解码器共享一个编码器。

   ![multitasking](http://upload-images.jianshu.io/upload_images/4787675-0f2176669ea14a6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   结果是第一个方案更好一些。
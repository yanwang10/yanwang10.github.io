---
author: Yan Wang
date: 2017-06-13 21:21:21 -0700
layout: post
title: 陈年老 Paper
categories: [论文]
tags:
-  论文
-  技术
-  科研
---

本帖列举其他有趣或者经典的文章，看到了就更新一下。

## Frustratingly Easy Domain Adaptation (arxiv 2009)

兴趣点：Domain Adaptation

介绍了一种非常简单的特征变换来做 domain adaptation，简单有效而且原理容易理解。

## Skip-Thought Vectors (NIPS 2015)

兴趣点：generic distributed sentence encoder

## Breaking Cycles in Noisy Hierarchies (WebSci 2017)

兴趣点：Hierarchy

这篇文章提出了一个好的问题，把带噪音的图转换为可以表示 hierarchy 的DAG。方法似乎都是已有的。文章把已有的方法归为三类：（1）在 BFS/DFS 加入简单的删除回边 (back edge) 的启发策略，（2）基于 minimum-feedback arc set (MFAS) 问题的 NP-hard 近似优化，以及（3）根据具体问题设计的特殊算法。但当然这三类方法各有各的问题。

本文的方法是，把这个问题变为一个给节点打分排序的问题，节点得分高意味着在层次中处于更高的位置，或者表示了更普遍 (more general) 的概念。对于给节点打分这个子问题，作者应用了两种已有方法：

`TrueSkill`，把 hierarchy 看成是多个玩家之间的比赛，两个节点之间的有向边表示一个节点“赢”了另一个节点，那么用给玩家排序的算法自然也可以给节点排序。这个想法很新颖，很贴合实际，唯一区别是，实际比赛场景中两个玩家可以多次对战以得到更好地估计，而一般的图中没有重边，可能算法给出的估计方差会大。

`Social Agony`，假设社交网络中的人们会互相推荐，一般来说上游的人推荐下游的人，要去掉噪音回边来形成 DAG，给边赋一定的权重，然后求一个总权重最大的 DAG 子图。如果所有边权重一样就退化为 MFAS 问题。

然后基于前两种基础方法，设计了一系列启发删边策略，用 voting 的方法聚合一下做出最终删边的决策。

实验部分没太看，反正也没什么太多的分析，跑一堆数据也没意思，还是去看 TrueSkill 。

## TrueSkill: A Bayesian Skill Rating System (NIPS 2007)

兴趣点：TrueSkill

这个算法是 Elo 算法的升级版，用于评估玩家的竞技水平，也是贝叶斯派的方法，可以考虑多人组队比赛的情况（不仅限于 1v1 的比赛），而且声称在 Xbox 上商用了。

略复杂，暴露了对贝叶斯的不了解……

## Opening the Black Box of Deep Neural Networks via Information

兴趣点：整个标题都是兴趣点


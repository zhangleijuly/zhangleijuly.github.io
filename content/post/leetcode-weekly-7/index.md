---
title: "LeetCode每日一题周总结(七)"
date: 2021-12-17
slug: leetcode-weekly-7
image: "img/LeetCode.jpeg"
math: true
draft: true
categories:
    - Code
tags:
    - LeetCode
---

## Consecutive Characters

太简单了，略。

## Range Sum of BST

也略。

## Insertion Sort List

链表版的插入排序，收获的经验是解决链表问题创建一个`dummy`节点有时候非常好用。

## Minimum Height Trees

[Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)题目大意：已知n个结点和n-1条边，以任何一个结点作为树根可以得到一棵树，求构成的树的高度最低的结点。

这道题看起来是一道树的题目，但是实际上它是一道图的题目。把这棵树看作一个图，那么构成树的高度最低就意味着这个结点最接近图的“中心”。从这种角度理解这道题的解法应该是从外层一层一层剥离结点，直到剩下1个或者2个结点（如果剩下2个结点，就无法继续剥离了；如果剩下3个结点，因为树中无环，那么一定还能剥离2个结点得到1个结点）。

首先需要求最外层的结点，也就是叶结点，叶结点只和一个结点相邻。遍历所有边，记录每个结点的相邻关系。然后遍历所有结点，找到所有只和一个结点相邻的结点，存入队列。

用一个循环来层层剥离，每次执行循环体剥离一层叶结点，记录未被剥离的结点数作为循环终止条件。首先把未剥离的结点数减少当前叶结点数。接下来剥离这层叶结点，遍历当前叶节点队列，把叶节点从它的相邻结点的相邻关系里删除。如果叶节点的相邻结点成为了新的叶节点，就把它保存到一个临时队列。遍历完所有当前叶节点后，用临时队列替换当前叶节点队列。

最后叶结点队列中剩余的1个或者2个结点即为所求。

## Maximal Square

[Maximal Square](https://leetcode.com/problems/maximal-square/)题目大意：已知一个由0和1构成的m×n大小的矩阵，找到由1构成的最大的正方形，返回它的面积。

![Maximal Square](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)


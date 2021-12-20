---
title: "LeetCode每日一题周总结(七)"
date: 2021-12-17
slug: leetcode-weekly-7
image: "img/LeetCode.jpeg"
math: true
draft: false
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

之前做过一道[Maximal Rectangle](/p/leetcode-weekly-5/#maximal-rectangle)，这道题稍微简单点。用`dp[i][j]`表示右下角坐标为`(i,j)`的正方形的边长，那么当`matrix[i][j]==0`时，`dp[i][j]=0`；当`matrix[i][j]==1`时，`dp[i][j]=min(dp[i-1][j-1],dp[i][j-1],dp[i-1][j])+1`。找到最大的边长就能找到答案。

## Numbers At Most N Given Digit Set

[Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)题目大意：已知一个数字集合和一个整数n，数字集合中包含1~9的几个数字，求用这些数字构成的不大于n的整数有多少个。

这道题其实是一道排列组合题，需要用代码实现数学计算的过程，需要注意一些细节。

比n小的数包括位数比n少的数，这些数可以随意组合，比较容易计算。主要需要考虑和n位数相等的数，需要从高位到低位遍历，如果这一位的数在数字集合里没有，那么这一位我们只能选比它小的数，后面的每一位都可以随意选择，跳出循环；如果这一位在数字集合里有，那么要分为两种情况，一种是选择比它小的数，后面每一位可以随意选择，另一种是选择这个数，继续遍历。需要注意的是，如果遍历到最低位每一个数都在集合中出现过，那么还需要在结果加1，也就是这个数本身。

## Decode String

[Decode String](https://leetcode.com/problems/decode-string/)题目大意：解码字符串，字符串编码形如`k[encoded_string]`，表示方括号中的字符串出现k次，需要注意方括号中的字符串可能也需要解码。

递归处理即可，从头到尾读取字符串，没有遇到数字就一直读取，遇到数字就解析数字，获取重复次数，然后截取'['和']'之间的字符串，递归调用该函数得到解码结果，把解码结果按照次数重复，然后继续读取字符串解码直到结束。

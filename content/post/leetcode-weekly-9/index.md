---
title: "LeetCode每日一题周总结(九)"
date: 2022-01-04
slug: leetcode-weekly-9
image: "img/LeetCode.jpeg"
math: true
draft: false
categories:
    - Code
tags:
    - LeetCode
---

## Number Complement

[Number Complement](https://leetcode.com/problems/number-complement/)题目大意：求一个整数的补数。

找到恰好比这个整数大的2的幂，减去这个整数再减1就是。需要注意整型的表示范围，可以用无符号整型或者长整型。

## Middle of the Linked List

[Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)题目大意：求链表的中点。

![Middle of the Linked List 1](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

![Middle of the Linked List 2](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

快慢指针入门题目，定义快指针和慢指针，快指针每次走两步，慢指针每次走一步，快指针走到链表尾部的时候慢指针就在链表的中点。

## Populating Next Right Pointers in Each Node

[Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)题目大意：为**完美二叉树**的结点添加指针指向相邻结点。

![Populating Next Right Pointers in Each Node](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

维护队列q1存储当前层的结点和队列q2存储下一层的结点。将根节点加入q1。重复以下循环直到q1为空：如果q1非空，从q1中弹出一个结点，将该结点的相邻结点指针指向q1中的下一个结点，如果没有下一个结点就指向空；如果该结点的左右子树非空，就把该结点的左右子树加入q2。如果q1为空，交换q1和q2。

## Smallest Integer Divisible by K

[Smallest Integer Divisible by K](https://leetcode.com/problems/smallest-integer-divisible-by-k/)题目大意：已知整数k，求最少由多少个1组成的整数可以整除k，如果没有这样的整数，返回-1。

首先判断k的因数是否包含2和5，如果包含可以直接返回-1。

维护整数长度和整数除以k的余数，并且用hash保存余数，如果同一个余数出现2次就说明不存在这样的整数。

如果余数不为0并且余数没有在hash中出现过就执行以下循环：把余数加入hash，计算新余数并把整数长度加1。

循环结束时判断余数，如果余数为0就返回长度，否则返回-1。

实际上返回-1的k好像只有因数包含2和5的k。

## Maximum Difference Between Node and Ancestor

[Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)题目大意：求一棵二叉树上两个有祖先关系的结点的值之差的最大值。

![Maximum Difference Between Node and Ancestor](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree.jpg)

把返回值保存在全局变量中，初始化为0。从根结点遍历整棵树，把根结点到当前结点路径上的最大值和最小值传给子结点，每到一个结点就用这个结点的值更新最大值和最小值，如果最大值和最小值的差大于返回值，就更新返回值。

## Burst Balloons

[Burst Balloons](https://leetcode.com/problems/burst-balloons/)题目大意：有`n`个气球，分别编号`0`到`n-1`，每个气球上都画着一个数字，用数组`nums`表示。如果打爆气球`i`，就能够得到`nums[i-1]*nums[i]*nums[i+1]`个硬币，如果`i-1`或者`i+1`超出数组边界，就乘以`1`代替。求打爆全部气球后最多能获得多少硬币。

这周最难的一道题，使用的方法是动态规划。用`dp[i][j]`表示打爆编号`i`到编号`j`的气球最多能获得的硬币，状态转移公式如下：

$$
dp[i][j] = max(dp[i][k-1]+dp[k+1][j]+nums[i-1]*nums[k]*nums[j+1]) \newline
(i<=k<=j)
$$
其中`k`表示最后一个打爆的气球的编号。从状态转移公式可以看出，需要按照区间长度由短到长计算`dp`，以保证更新时用到的`dp`值都已经计算过。

## Pairs of Songs With Total Durations Divisible by 60

[Pairs of Songs With Total Durations Divisible by 60](https://leetcode.com/problems/pairs-of-songs-with-total-durations-divisible-by-60/)题目大意：已知若干歌曲和它们的时长，求取出两首总长度为整数分钟的歌一共有多少种取法。

用每首歌的长度分别余60，统计每种余数的歌各有多少。计算取法包含两种情况，一种是余数为1~29的歌要和余数为31~59的歌组合，另一种是余数为0的歌或者余数为30的歌两两组合。分别计算相加即可。

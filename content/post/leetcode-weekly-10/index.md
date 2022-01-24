---
title: "LeetCode每日一题周总结(十)"
date: 2022-01-11
slug: leetcode-weekly-10
image: "img/LeetCode.jpeg"
math: true
categories:
    - Code
tags:
    - LeetCode
---

## Find the Town Judge

略。

## Complement of Base 10 Integer

与[Number Complement](https://zhangleijuly.me/p/leetcode-weekly-9/#number-complement)类似。

## Palindrome Partitioning

[Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)题目大意：已知一个字符串，求把这个字符串分解成回文字符串的所有方案。

使用深度优先搜索和回溯遍历字符串的所有子串，维护当前遍历的子串和字符串的回文分解方案，只有当子串是回文字符串时才能添加到分解方案，如果遍历到字符串末尾时恰好所有子串都是回文字符串，就把当前分解方案加入答案中。每次把子串添加到分解方案都要进行回溯，即从分解方案中弹出当前子串。

## Car Pooling

[Car Pooling](https://leetcode.com/problems/car-pooling/)题目大意：已知一辆汽车的准乘人数和一个数组`trips`，`trips`的每一项包含三个值：`numPassengers`，`from`和`to`，表示有`numPassengers`个乘客在`from`上车并在`to`下车。求汽车能否在不超载的情况下把所有乘客送到目的地。

这是一道模拟题，初始汽车乘客数为0，`numPassengers`个乘客在`from`上车并在`to`下车可以分解为在`from`乘客数增加`numPassengers`和在`to`乘客数减少`numPassengers`两个事件。遍历`trips`得到所有事件，按照事件发生的位置从小到大排序，依次计算在每个位置汽车是否超载即可。

## Linked List Random Node

[Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/)题目大意：编写一个函数随机获取链表中的一个结点。

题目不难，可以先计算链表长度，在长度范围内生成一个随机数，返回这个序号的结点就可以了。

我更加好奇的是LeetCode是怎么判断这道题的对错的。

## Cherry Pickup II

[Cherry Pickup II](https://leetcode.com/problems/cherry-pickup-ii/)题目大意：如图的矩形表示草莓田，格子中的数字表示草莓数量，有两个机器人分别从矩形的左上角和右上角出发采摘草莓，它们遵循以下原则行动：

- 机器人从第i行第j列可以移动到第i+1行第j-1列、第i+1行第j列或第i+1行第j+1列。
- 机器人经过一个格子时会采摘全部草莓，这个格子变为空。
- 两个机器人到达同一个格子时，只有一个机器人可以采摘到草莓。
- 机器人不能走出矩形边界。
- 机器人应当抵达矩形的底部。

求机器人最多能够采摘多少草莓。

![Cherry Pickup II](https://assets.leetcode.com/uploads/2020/04/29/sample_1_1802.png)

使用动态规划，根据题意两个机器人总是在同一行，所以用`dp[r][c1][c2]`表示第一个机器人从第r行第c1列出发，第二个机器人从第r行第c2列出发最多能采摘多少草莓。状态转移公式为：
$$
\begin{aligned}
dp[r][c1][c2]&=机器人在当前位置能够采摘的草莓数 \newline
&+max(dp[r+1][c1-1][c2-1],\dots,dp[r+1][c1+1][c2+1])
\end{aligned}
$$

## Robot Bounded In Circle

[Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle/)题目大意：在无穷大的平面上，上北下南左西右东，机器人站在原点并且面朝北方，它能够接收三种指令：

- 'G'：机器人将向前移动一个单位。
- 'L'：机器人将向左旋转90度。
- 'R'：机器人将向右旋转90度。

已知一个指令序列，判断机器人能否在执行若干次该指令序列后回到原点。

这道题是一道模拟题，机器人只有在一种情况下无法回到原点，即机器人执行完该指令序列后离开原点并且仍然面朝北方。其他任意情况执行4次指令序列后机器人都能回到原点。因此只需要对机器人执行完一遍指令序列后的位置和朝向进行判断。
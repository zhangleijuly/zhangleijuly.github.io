---
title: "LeetCode每日一题周总结(八)"
date: 2021-12-24
slug: leetcode-weekly-8
image: "img/LeetCode.jpeg"
math: true
draft: false
categories:
    - Code
tags:
    - LeetCode
---

## Minimum Absolute Difference

略。

## Power of Two

[Power of Two](https://leetcode.com/problems/power-of-two/)题目大意：判断一个整数是不是2的幂。

题目很简单，可以用递归或者循环一路判断加除2。但是其他的方法也值得说道说道，一种方法是按位判断，2的幂的二进制表示只包含一个1；还有一种方法是在可表达范围找到一个最大的2的幂，判断能不能用这个整数整除。

## Reorder List

[Reorder List](https://leetcode.com/problems/reorder-list/)题目大意：把链表$L_0 → L_1 → … → L_{n-1} → L_n$重排成$L_0 → L_n → L_1 → L_{n-1} → L_2 → L_{n-2} → …$

![Reorder List](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

链表只能单向遍历，借助栈这种后进先出的数据结构可以比较简单地实现这种重排。

## Course Schedule II

[Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)题目大意：已知一些课程和先修课程要求——指在学习课程A之前必须先学习课程B，求学完全部课程的顺序，如果没有一种合理的顺序就返回空。

Course Schedule是一系列题，因为最近时间紧张我没有做完这个系列的全部题，这里只记录这道题的解法，整个系列的以后补上。

这道题可以用图的思路去解答，把所有课程看作是有向图中的结点，如果学习课程A之前必须先学习B，那么就在图上添加一条B指向A的边。在初始状态下只有没有入度的课程可以学习，表示这些课程没有先修课程。维护所有课程的入度，将所有入度为0的课程都加入一个队列中，表示它们可以学习。依次学习队列中的课程，把课程添加到解中，并把该课程指向的课程入度减1，如果指向的课程入度为0，就添加到学习队列。直到学习队列为空，说明已经没有可以学习的课程了，此时判断解中是否包含全部课程，如果是就返回解，如果不是就说明还有其他课程无法学习，返回空。

## Merge Intervals

虽然是一道中等难度的题，但是不难，略。

## Basic Calculator II

[Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)题目大意：已知一个由数字和四则运算符号组成的表达式字符串，计算这个表达式的值。

计算表达式的值没有太多可以说的点，通常的做法是用两个栈，一个保存运算数，一个保存运算符，每次计算的时候弹出两个数和一个符号，算出结果之后再压栈。这道题主要需要注意的地方就是运算优先级，在一个没有括号的表达式里乘除法的优先级高于加减法，所以遇到乘除号可以直接计算。这样就保证运算符栈内只有加减法，加减法需要按顺序计算，做法是每当运算符栈内有两个运算符，就计算栈底的运算符。这时候弹出3个运算数，2个运算符，计算完成后压入2个运算数和1个运算符。

## K Closest Points to Origin

[K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)题目大意：已知若干二维平面点的坐标，求距离原点最近的k个点的坐标。

感觉考察点应该是结构体的排序？中等难度但是做起来很简单，排序或者用优先队列都可以。


---
title: "LeetCode每日一题周总结(一)"
date: 2021-11-08
slug: leetcode-weekly-1
image: "img/LeetCode.jpeg"
math: true
categories:
    - Code
tags:
    - LeetCode
---
## Surrounded Regions

![Surrounded Regions](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

 [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)题目大意：矩形棋盘上有"O"和"X"两种棋子，将所有四面被"X"包围的"O"的区域都替换为"X"(棋盘边缘不算被"X"包围)。

一道做过的题，常规思路是遍历棋盘上所有"O"的区域，用**深度优先搜索**确定当前区域的范围，并判断是否满足替换条件，如果满足替换条件就进行替换，不满足替换条件就标记当前区域为已访问。这种思路能够解答该题，但是比较麻烦，需要维护额外的数据结构储存当前区域的范围，并且直到搜索遍整个当前区域后才能够确定是否需要替换。

另一种比较巧妙的思路是反向思考，题目的含义告诉我们，不需要替换的"O"的区域都是临近棋盘边缘的，所以可以先找到这样的区域。沿着棋盘边缘寻找包含"O"的区域，同样用**深度优先搜索**确定这些区域的范围，这些区域就是所有不需要替换的"O"，对这些"O"做标记，把其他的"O"替换为"X"即可。

## Unique Paths III

前置题目为[Unique Paths](https://leetcode.com/problems/unique-paths/)和[Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)。

![Unique Paths](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

[Unique Paths](https://leetcode.com/problems/unique-paths/)题目大意：机器人从矩形网格左上角走到右下角，每次只能向右或者向下走一步，求不同的走法有多少种。

使用递推法即可。假设矩阵大小为m×n，左上角坐标为(0, 0)，右下角坐标为(m-1, n-1)。从(0, 0)到(i, j)的不同走法记作result\[i][j]，有递推公式result\[i][j] = result\[i-1][j] + result\[i][j-1]，系统初态为result\[0][0] = 1。

![Unique Paths II](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

[Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)与Unique Paths题干类似，只是在网格中增加了障碍物，整体解题思路不变。

![Unique Paths III](https://assets.leetcode.com/uploads/2021/08/02/lc-unique1.jpg)

[Unique Paths III](https://leetcode.com/problems/unique-paths-iii)题干相比以上两道发生了比较大变化，起点和终点不再固定。网格上的点分为四类：起点、终点、空格和障碍物。机器人从起点出发，可以任意向四个方向移动到终点，必须通过并且仅通过每个空格一次，求不同的走法有多少种。

可以用**深度优先搜索**解决，因为网格上的点只有四种，每个空格必须通过并且只能通过一次，所以每种可行的走法通过的格子数是一定的。首先扫描整个网格确定起点、终点和空格数，从起点开始搜索，初始步数为0，如果遇到障碍物或者已经踩过的空格就返回；遇到没走过的空格就标记已走过，步数加1，然后继续搜索；遇到终点就判断步数是否等于空格数，等于就返回1，否则返回0。将所有返回值相加就是最终结果。

## Sum Root to Leaf Numbers

![Sum Root to Leaf Numbers](https://assets.leetcode.com/uploads/2021/02/19/num2tree.jpg)

[Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)题目大意：一棵二叉树每个结点都是0~9的整数，从根结点到叶子结点的路径能够表示一个整数，例如上图的路径4->9->5可以表示495。求一棵这样的二叉树根结点到每一个叶子结点路径表示的整数之和。

**深度优先搜索**即可，如果根结点为空直接返回0。用cur记录根结点到当前结点路径表示的整数，初始为0。对每一个非空结点root，调用函数时首先更新cur为cur*10+root->val。如果左右子树都为空，说明当前结点是叶子结点，给最终结果加上cur；否则递归调用函数，将cur传给非空子树。

## Sum of Left Leaves

![Sum of Left Leaves](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

[Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)题目大意：求二叉树所有左叶子结点值的和。

从根结点开始递归遍历每一个结点，增加一个标记位，进入左子树时传true，进入右子树时传false。到达叶子结点时如果标记位是true就在最终结果加上当前结点的值。

## Arranging Coins

![Arranging Coins](https://assets.leetcode.com/uploads/2021/04/09/arrangecoins2-grid.jpg)

[Arranging Coins](https://leetcode.com/problems/arranging-coins/)题目大意：n枚硬币按照1、2、3……逐层摆放，求摆满的层数。

略。

## Single Number III

前置题目为[Single Number](https://leetcode.com/problems/single-number/)和[Single Number II](https://leetcode.com/problems/single-number-ii/)。

[Single Number](https://leetcode.com/problems/single-number/)题目大意：非空数组中除了一个整数外其他整数都出现偶数次，找到只出现一次的整数。

这里利用了异或运算的性质：a⊕b⊕a=b。把整个数组异或起来结果就是要找的整数。

[Single Number II](https://leetcode.com/problems/single-number-ii/)题目大意：非空数组中除了一个整数外其他整数都出现3次，找到只出现一次的整数。

常规思路是把每个整数都表示为二进制，对每一位分别求和并模3，余数所构成的二进制整数就是所求的数。进阶解法是把每一位求和模3的状态表示为0、1和2，那么下一个数这一位为0时，该状态保持不变；下一个数这一位为1时，状态将按照0->1->2->0迁移，表示为二进制就是00->01->10->00。分别用B<sub>0</sub>和B<sub>1</sub>表示二进制状态的低位和高位，B表示输入的这一位的值，根据状态迁移可以推导出以下关系：

$$
B_0 = (B_0 \oplus B) \And B_1 \newline
B_1 = (B_1 \oplus B) \And B_0
$$

应用到这道题中，就是用两个整数high和low分别维护二进制整数每一位的高低位状态表示，对数组中的每一个整数分别计算high和low，最终维护低位状态表示的整数low即为答案。

[Single Number III](https://leetcode.com/problems/single-number-iii/)题目大意：非空数组中除了两个整数外其他整数都出现偶数次，找到这两个整数。

假设这两个整数分别为a和b，利用异或运算的性质，把整个数组异或起来结果就是a⊕b。a和b是不同整数，所以a⊕b不为0，我们可以在a⊕b的二进制表示中找到值为1的某一位，a和b的这一位一定一个为0一个为1。根据这一位的不同把数组分为两个数组，那么a和b一定分别在这两个数组中，并且除了a和b以外这两个数组中的其他整数都是成对出现的，接下来按照Single Number的解法就可以得到a和b。

## Multiply Strings

[Multiply Strings](https://leetcode.com/problems/multiply-strings/)题目大意：已知两个表示整数的字符串，求这两个整数的乘积，也用字符串表示。

字符串表示的大整数乘法，略。
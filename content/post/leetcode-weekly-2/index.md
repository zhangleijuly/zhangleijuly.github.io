---
title: "LeetCode每日一题周总结(二)"
date: 2021-11-14
slug: leetcode-weekly-2
image: "img/LeetCode.jpeg"
math: true
draft: false
categories:
    - Code
tags:
    - LeetCode
---

## Unique Binary Search Trees

![Unique Binary Search Trees](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

[Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)题目大意：n个结点的二叉搜索树其结点的值分别为1~n，求这样的二叉搜索树有多少种不同的结构。

刷软考题时遇到过相同问题，可以把该问题转化为规模更小的问题来求解。假设结点数为n时该问题的解为`fun(n)`。对于n个结点的二叉搜索树，选择1个结点作为根节点，可以选择1~n，假设我们选择m作为根节点，那么根据二叉搜索树的性质，左子树将包含1~m-1这m-1个结点，右子树将包含m+1~n这n-m个结点，以m作为根结点的二叉搜索树的结构就有`fun(m-1)*fun(m-n)`种。根节点的选择有n种，可以得到以下公式：
$$
fun(n) = \sum_{i=1}^n fun(i-1)*fun(n-i)
$$
为了避免重复运算，我们用数组记录结果，初始化`fun[0]=1`，然后从小到大依次求解`fun[1]`直到`fun[n]`即可。

## Number of Valid Words for Each Puzzle

[Number of Valid Words for Each Puzzle](https://leetcode.com/problems/number-of-valid-words-for-each-puzzle/)题目大意：输入包括`word`和`puzzle`两种字符串，对于一个`puzzle`，满足以下条件的`word`是合法的：

- `word`包含`puzzle`的首字母
- `word`中的每一个字母都在`puzzle`中出现

求每个`puzzle`分别有多少个合法的`word`。

这道题题意比较简单，数据规模不大的情况下可以直接求解。但是LeetCode给出的数据规模是$1 \leq words.length \leq 10^5$和$1 \leq puzzle.length \leq 10^4$，直接求解一定会超时。

题目的约束中，`word`和`puzzle`只包含英文小写字母。用1位二进制表示某个字母在字符串中是否出现，1表示出现，0表示未出现，只需要26位二进制整数就能表示一个字符串包含的字母集合。遍历`word`数组，可以求出每一个单词所包含的字母集合，因为不同的单词可能有相同的字母集合，用一个`hashMap`保存每一个字母集合分别构成了多少个单词。

处理完`word`之后，对每个`puzzle`也计算出表示它所包含字母集合的整数，因为后面用到[**掩码**](https://zh.wikipedia.org/wiki/%E6%8E%A9%E7%A0%81)操作，就用`mask`表示这个整数。然后需要找到符合条件的`mask`集合的子集`subMask`。

首先`subMask`一定小于等于`mask`，但是并非所有小于等于`mask`的整数都是`mask`的子集，这时就需要用到[**掩码**](https://zh.wikipedia.org/wiki/%E6%8E%A9%E7%A0%81)操作，对任意`subMask`，`subMask&mask`一定是`mask`的子集。用下面的循环就可以遍历所有`mask`的子集，掩码操作也是对这个循环的剪枝，大大减少了循环的时间代价。

```c++
int subMask = mask;
while(subMask > 0)
{
    //do something
    subMask = (--subMask) & mask;
}
```

当然，不要忘记在`subMask`中必须包含`puzzle`的首字母。对符合条件的`subMask`，在`hashMap`中查找有多少由该字母集合构成的单词，计入结果中即可。

## Best Time to Buy and Sell Stock II

这周最有意思的有一道题，兄弟姐妹也真的多，同系列一共有6道题，包括[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)、[Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)、[Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)、[Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)、[Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)和[Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)。

我做了前三道，但是思路不连贯在第四道卡住了，在网上找第四道的题解大多是用局部最优和全局最优数组的动态规划解法，看得一知半解。直到在B站找到下面的视频，小姐姐把这六道题串连在一起，解题思路统一清晰，关键是容易理解，强烈推荐。

{{< bilibili BV1nv411P7bk >}}

下面主要介绍这系列题的题意，我解前三道题的方法和补充视频里动态规划数组降维的证明。

### 题目大意

题目的背景是已知一个数组表示每一天的股票价格`prices`，可以选择某一天买入一支股票，并在之后的另一天卖掉获得盈利，同时最多只能持有一支股票。每道题都是在这个背景上增加不同的条件，求能够获得的最高盈利是多少。

[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)的条件是只能买入一次卖出一次，如果不能盈利就返回0。

[Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)的条件是可以任意买入和卖出，只要满足最多持有一支股票的限制即可。

[Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)的条件是最多只能够买入和卖出2次，[Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)更进一步，最多只能买入和卖出`k`次。

[Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)的条件是可以买入和卖出任意多次，但是卖出股票后必须等待一天才能够再次买入。

[Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)的条件是可以买入和卖出任意多次，但是卖出股票时必须支付交易费`fee`。

### 我解前三道题的思路

Best Time to Buy and Sell Stock的条件是只能买入和卖出一次，所以使用贪心法。维护一个买入价格`buy`和盈利`profit`，分别初始化为`prices[0]`和0。从第一天开始遍历股票价格，如果这一天的股票价格比买入价格低，那么在这一天买入可以获得更高的盈利，更新`buy=prices[i]`；如果这一天的股票价格比买入价格高，那么可以在这一天卖出，更新盈利`profit=max(profit, prices[i]-buy)`。

Best Time to Buy and Sell Stock III可以在第一题的基础上求解。题目的条件是最多可以买入和卖出2次，那么可以分为3种情况：

1. 不进行交易，盈利为0；
2. 买入和卖出1次，第一题中已经求出最高盈利；
3. 买入和卖出2次，假设第一次卖出发生在第`i`天，那么我们可以分别对第一天到第`i`天和第`i+1`天到最后一天使用第一题的算法分别计算最高盈利，然后相加即可。

只需要考虑第三种情况，我们需要分别计算第一天到第`i`天交易一次的最高盈利和第`i`天到最后一天交易一次的最高盈利。对于前者，我们用数组`fromBegin[i]`储存第一天到第`i`天交易一次的最高盈利，仍然使用第一题的算法，只需要每一天都用`profit`更新`fromBegin[i]`即可。我们用数组`toEnd[i]`储存第`i`天到最后一天交易一次的最高盈利。把第一题的算法反过来，维护一个卖出价格`sell`和盈利`profit`，分别初始化为`prices[prices.size()-1]`和0。从最后一天倒序遍历股票价格，如果这一天的股票价格比卖出价格高，那么在这一天卖出可以获得更高的盈利，更新`sell=prices[i]`；如果这一天的股票价格比卖出价格低，那么可以在这一天买入，更新盈利`profit=max(profix,sell-prices[i])`。每一天都用`profit`更新`toEnd[i]`。

初始化`profit`为`fromBegin[prices.size()-1]`，也就是只交易一次能获得的最高盈利。然后遍历所有`i`，更新`profit=max(profit,fromBegin[i]+toEnd[i+1])`。最后`profit`即为最高盈利。

最后回到本题Best Time to Buy and Sell Stock II，题目不限制买入和卖出次数，但是最多只能持有一支股票。如果画出股票的价格曲线，那么要实现最大化盈利只需要在曲线的波谷买入股票，在下一个波峰卖出，然后在下一个波谷再买入，不断重复。可以用算法模拟这一过程，分别维护两个变量`top`和`bottom`表示价格曲线中的波峰和波谷。在一笔交易中需要先买入后卖出，所以先寻找波谷，再寻找波峰。从第一天遍历股票价格，直到当天的价格比后一天的价格低，那么这一天的价格就是波谷，接下来继续遍历，直到当天的价格比后一天的价格高，那么这一天的价格就是波峰，找到一对波谷与波峰后在盈利中加上`top-bottom`，循环该过程直到遍历整个数组。

### 视频里数组降维的证明

数组降维出现在视频对第二道题的讲解中(3分40秒到8分40秒)。用动态规划来解决这道题，每一天有两种状态，即有股票和没有股票。每一天的状态由前一天的状态转移而来，用`dp[i][1]`和`dp[i][0]`表示第`i`天有股票和没有股票的盈利状态，状态转移公式如下：

```c++
dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i]);
dp[i][1] = max(dp[i-1][1],dp[i-1][0]-prices[i]);
```

因为状态转移只跟前一天的状态有关系，用`withShare`和`noShare`分别表示有股票和没有股票的盈利状态，状态转移公式变成：

```c++
noShare = max(noShare, withShare+prices[i]);
withShare = max(withShare, noShare-prices[i]);
```

评论区有人问`noShare`更新完已经是第`i`天的状态了，用第`i`天的`noShare`更新第`i`天的`withShare`不对啊。其实我第一次看到这里的时候也产生了相同的疑问，动态规划的状态转移公式比较好理解，后一天的状态由前一天的状态来决定，但数组降维之后后更新的变量一定需要用先更新的变量去更新，这和后一天的状态由前一天的状态来决定不矛盾吗。严格计算之后就会发现并不矛盾。

首先观察数组降维前的状态转移公式，这两个`max`看似进行了两次比较，但实际上比较的是相同的东西。不等式`dp[i-1][1]>dp[i-1][0]-prices[i]`，把`prices[i]`移项后可得`dp[i-1][1]+prices[i]>dp[i-1][0]`。也就是说，`dp[i-1][1]+prices[i]`和`dp[i-1][0]`的大小关系唯一地决定了状态转移的结果。

下面分别讨论`dp[i-1][1]+prices[i]`和`dp[i-1][0]`的不同大小关系对状态转移的影响。

1. `dp[i-1][1]+prices[i]`和`dp[i-1][0]`相等。二者相等那么`max`函数在两个相等的值中任取其一，选择`dp[i][0]=dp[i-1][0]`和`dp[i][1]=dp[i-1][1]`。对应数组降维后是`noShare=noShare`和`withShare=withShare`，`withShare`没有用`noShare`更新，所以没有影响。
2. `dp[i-1][1]+prices[i]`比`dp[i-1][0]`大。可以得到`dp[i][0] = dp[i-1][1]+prices[i]`和`dp[i][1] = dp[i-1][i]`。对应数组降维后是`noShare=withShare+prices[i]`和`withShare=withShare`，`withShare`没有用`noShare`更新，所以没有影响。
3. `dp[i-1][1]+prices[i]`比`dp[i-1][0]`小。可以得到`dp[i][0] = dp[i-1][0]`和`dp[i][1]=dp[i-1][0]-prices[i]`。对应数组降维后是`noShare=noShare`和`withShare=noShare-prices[i]`，`withShare`用`noShare`更新，但是这时`noShare`的值还是前一天的`noShare`的值，所以仍然没有影响。

综上所述，无论何种情况，数组降维后两种状态都能够正确更新。同样在Best Time to Buy and Sell Stock with Transaction Fee一题中，状态转移公式中虽然加上了交易费，但是两种状态还是能够正确更新。

## Minimum Value to Get Positive Step by Step Sum

[Minimum Value to Get Positive Step by Step Sum](https://leetcode.com/problems/minimum-value-to-get-positive-step-by-step-sum/)题目大意：已知整型数组`nums`，用一个正整数`startValue`逐一累加数组中的数，为保证累加和全程不小于1，`startValue`至少要是多少。

略。

## Remove Linked List Elements

[Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)题目大意：已知链表的头指针，删除链表中所有值等于给定值的结点，返回新的头指针。

略。

## Daily Temperatures

[Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)题目大意：已知一个数组`temperatures`表示每日温度，求每一天需要过多少天才能迎来更高温度，如果未来没有更高的温度，输出0。

题意很简单，但是数据规模很大，$1 \leq temperatures.length \leq 10^5$，如果对每一天都扫描一遍数组肯定会超时。

解这类**找后面第一个比自身小或者大的位置**的题目，可以考虑使用单调栈，单调栈要求其中的元素保持单调性。

维护一个栈`stack`，栈内元素保存数组下标，保持的单调性为：越靠近栈顶的数组下标，对应的温度越低。

倒序遍历数组`temperatures`，遍历到第`i`个元素时，如果栈非空就弹栈直到栈为空或者栈顶元素对应的温度比`temperatures[i]`大。如果栈为空则记答案为0，如果栈不为空那么记答案为`stack.top()-i`。最后把`i`入栈。

## Iterator for Combination

[Iterator for Combination](https://leetcode.com/problems/iterator-for-combination/)题目大意：设计一个`CombinationIterator`类，构造时传入一个由不同英文小写字母构成的有序字符串`characters`和组合长度`combinationLength`。该类有两个方法：`next()`按字典序返回下一个长度为`combinationLength`的字母组合；`hasNext()`返回是否存在下一个字母组合。

根据题意，在构造`CombinationIterator`对象时，需要生成由`characters`中字符构成的所有长度为`combinationLength`的字母组合，并按照字典序保存在一个数组中。初始化数组下标`index`为0，调用`next()`方法时返回当前下标的字母组合，然后把`index`加1；调用`hasNext()`方法时返回`index`是否小于数组大小的布尔值。

## 一些碎碎念

上一周算是我行动力最强的一周了，花了几天时间把博客从[Hexo](https://hexo.io/)迁移到[Hugo](https://gohugo.io/)，开始参与[LeetCode](https://leetcode.com/)每日打卡领勋章活动，并记录总结。

刷算法题我是一直不太擅长的，虽然我曾经也在博客写过很多的题解，但是现在我想换一种方式。我希望能够在尽量不贴代码的情况下用文字把解题思路描述清楚，今后自己再看的时候能够想起算法是如何实现的。现阶段这些内容可能并不适合其他读者看，毕竟在算法上文字比起代码在表达上是乏力的。幸运的是互联网上有大量分享解题思路的文章，我也仅仅是其中普通的一员罢了。

工作之后才发现现在互联网上充斥着所谓“内容农场”的网站，每每搜索一些问题的解决办法出现在首页的都是一些机器人抓取的机翻的出处不明的文章，虽然有时候能在其中找到work的方法，但是给人的观感真的很差。也正是这些内容让我意识到重要的并非问题和答案，而是找到答案的过程。这些总结对我而言的意义正在于此，在今后遇到类似问题的时候能够举一反三，有记录可复盘。

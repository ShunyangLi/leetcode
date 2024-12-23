
# Best Time to Buy and Sell Stock

[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Say you have an array for which the *i*th element is the price of a given stock on day *i*.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

**Example 1:**

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

**Example 2:**

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

这个题的思路稍微简单一点，可以使用动态规划来找到最小的买入价格，然后我们就可以根绝在买入价格最低的日期之后，找到卖出价格最高的点，这样收益就是最大的。

```java
public int maxProfit(int[] prices) {
    if (prices.length < 2) return 0;
    int max = 0, stock = prices[0];

    for (int price : prices) {
        int receive = price - stock;
      	// 找到最大收入
        if (receive >= 0) max = Math.max(max, price-stock);
				// 找到最新的收入
        stock = Math.min(stock, price);
    }

    return max;
}
```
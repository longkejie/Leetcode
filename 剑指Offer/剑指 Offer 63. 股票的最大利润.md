# [剑指 Offer 63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

示例 1:

输入: [7,1,5,3,6,4]

输出: 5

解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。

注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。

示例 2:

输入: [7,6,4,3,1]

输出: 0

解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

限制：

0 <= 数组长度 <= 10^5

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路

### 方法1：记录+循环

对于每一天我们交易掉该股票能赚取的前等于该天的价格减去改天以前包含改天中价格最低的那天的价钱（表明我们是那天买的），每次计算完都维护一个整体最大值$ans$,遍历完数组返回答案即可。



**时间复杂度**：$O(n)$，遍历一遍数组即可

**空间复杂度**：$O(1)$，只需一个额外的变量保存局部最小值，然后就不再需要额外的空间来保存中间值

#### 代码

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ans = 0, mmin = INT32_MAX;
        if (prices.empty()) {
            return ans;
        }
        for (auto &i : prices) {
            mmin = min(i, mmin);
            ans = max(ans, i - mmin);
        }
        return ans;
    }
};
```
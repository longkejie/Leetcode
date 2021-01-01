# [剑指 Offer 64. 求1+2+…+n1](https://leetcode-cn.com/problems/qiu-12n-lcof/)

求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。



示例 1：

输入: n = 3

输出: 6

示例 2：

输入: n = 9

输出: 45

限制：

1 <= n <= 10000

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/qiu-12n-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 思路

### 方法1：短路原则+递归

因为递归都是需要条件判断来确定条件边界的，但是这里不能用if，所以我们可以使用一个短路原则。

设置一个遍历b，当n>0时，b为真，否则b为假。

使用如下表达式

$b\&\&(sum = sumNums(n - 1) + n)$

含义就是如果b为假，后面的表达式将不会执行，否则执行后面的表达式（递归）

#### 代码

```cpp
class Solution {
public:
    int sumNums(int n) {
       int sum = 0;
       int b = n > 0;
       b && (sum = sumNums(n - 1) + n);
       return sum;
    }
};
```


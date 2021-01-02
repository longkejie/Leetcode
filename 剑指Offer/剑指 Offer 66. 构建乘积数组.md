# [剑指 Offer 66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中 B 中的元素 B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。

示例:

输入: [1,2,3,4,5]

输出: [120,60,40,30,24]

提示：

所有元素乘积之和不会溢出 32 位整数

a.length <= 100000

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 思路

### 方法1：动态规划+循环

大佬的解法如下：

本题的难点在于 不能使用除法 ，即需要只用乘法 生成数组 B。根据题目对 B[i]的定义，可列表格，如下图所示。

根据表格的主对角线（全为 1 ），可将表格分为 上三角 和 下三角 两部分。分别迭代计算下三角和上三角两部分的乘积，即可不使用除法 就获得结果。

<img src="https://pic.leetcode-cn.com/6056c7a5009cb7a4674aab28505e598c502a7f7c60c45b9f19a8a64f31304745-Picture1.png" alt="Picture1.png" style="zoom: 50%;" />

算法流程：
**初始化**：数组 B ，其中B[0]=1 ；辅助变量tmp=1 ；
计算 B[i]的 下三角各元素的乘积，直接乘入B[i] ；
计算 B[i]的 上三角 各元素的乘积，记为 tmp，并乘入B[i] ；
返回 B 。
复杂度分析：
**时间复杂度** $O(N)$ ： 其中 N 为数组长度，两轮遍历数组 a，使用O(N) 时间。
**空间复杂度** $O(1)$ ： 变量 tmp 使用常数大小额外空间（数组 b作为返回值，不计入复杂度考虑）。

作者：jyd
链接：https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/solution/mian-shi-ti-66-gou-jian-cheng-ji-shu-zu-biao-ge-fe/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#### 代码

```cpp
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        if (a.empty()) {
            return {};
        }
        vector<int> ans;
        ans.push_back(1);
        for (int i = 0; i < a.size() - 1; ++i) {
            ans.push_back(a[i] * ans[i]);
        }
        int temp = 1;
        for (int i = a.size() - 1; i >= 0; --i) {
            ans[i] *= temp;
            temp *= a[i]; 
        }
        return ans;
    }
};
```


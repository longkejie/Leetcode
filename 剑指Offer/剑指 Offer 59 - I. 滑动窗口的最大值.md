# [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3

输出: [3,3,5,5,6,7] 

解释: 

  滑动窗口的位置                					最大值

```cpp
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

提示：

你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路

### 方法1：暴力

对于暴力，我们每次都遍历一遍窗口中的值，找到最大的即可。

**时间复杂度**：O(nk)，每次遍历k个元素找最大值，遍历n-k+1次。

**空间复杂度**：O(1)，不需要额外的空间

#### 代码

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        if (nums.empty()) {
            return ans;
        }
        for (int i = 0; i < nums.size() - k+1; ++i) {
            ans.push_back(*max_element(nums.begin() + i,nums.begin() + i + k));
        }
        return ans;
    }
};
```



### 方法2：双端队列+单调队列

**借鉴了题解大佬的题解，详细链接看下面**

窗口对应的数据结构为 双端队列 ，本题使用 单调队列 即可解决以上问题。遍历数组时，每轮保证单调队列 deque ：

deque内 仅包含窗口内的元素 ⇒ 每轮窗口滑动移除了元素 nums[i - 1] ，需将 deque内的对应元素一起删除。

deque 内的元素 非严格递减 ⇒ 每轮窗口滑动添加了元素 nums[j + 1] ，需将 deque 内所有 < nums[j + 1] 的元素删除。

算法流程：

1.**初始化**： 双端队列 deque ，结果列表 ans ，数组长度 n ；

2.**滑动窗口**： 左边界范围 i∈[1−k,n+1−k] ，右边界范围 j∈[0,n−1] ；

​	a.若 i > 0且 队首元素 deque[0] == 被删除元素 nums[i - 1],则队首元素出队；

​	b.删除 deque内所有 < nums[j]的元素，以保持 deque 递减；

​	c.将nums[j] 添加至 deque尾部；

​	d.若已形成窗口（即 i≥0 ）：将窗口最大值（即队首元素 deque[0] ）添加至列表 ans 。

3.**返回值**： 返回结果列表 ans 。

作者：jyd

链接：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/mian-shi-ti-59-i-hua-dong-chuang-kou-de-zui-da-1-6/

来源：力扣（LeetCode）

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

#### 代码

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        if (nums.empty()) {
            return ans;
        }
        deque<int > deq;
        for (int i = 0; i < k; ++i) {
            while (deq.size() && deq.back() < nums[i]) {
                deq.pop_back();
            }
            deq.push_back(nums[i]);
        }
        ans.push_back(deq.front());
        for (int i = k; i < nums.size(); ++i) {
            if (nums[i - k] == deq.front()) {
                deq.pop_front();
            }
            while (deq.size() && deq.back() < nums[i]) {
                deq.pop_back();
            }
            deq.push_back(nums[i]);
            ans.push_back(deq.front());
        }
        return ans;
    }
};
```


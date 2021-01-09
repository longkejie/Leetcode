# [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。 

示例 1:

输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]

输出: 2

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



本题写出来的话较为简单，但是其中的方法值得学习



## 思路

### 方法1：哈希表（常规方法）

我们利用哈希表记录每个数在数组中出现的次数，当有某个数的次数大于数组长度的一半时，返回该数即可



**时间复杂度**：$O(n)$，遍历一遍数组

**空间复杂度**：$O(n)$，需要保存数组中每个数字，记录个数

#### 代码

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> mmap;
        for (auto &i : nums) {
            mmap[i]++;
            if (mmap[i] > nums.size() / 2) return i;
        }
        return 0;
    }
};
```



### 方法2：排序（值得学习）

我们将数组排序，然后中间那个值肯定就是我们的答案。



**时间复杂度**：$O(nlogn)$，排序所需的时间复杂度

**空间复杂度**：$O(logn)$，归并排序以及快排都需要递归，这也是需要消耗内存空间的。

#### 代码

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};
```



### 方法3：摩尔计数（最优方法）

摩尔投票法：

设输入数组 nums 的众数为 x ，数组长度为 n 。

推论一： 若记众数的票数为+1 ，非众数的票数为−1 ，则一定有所有数字的票数和>0 。

推论二： 若数组的前 a个数字的票数和=0 ，则数组剩余(n−a)个数字的 票数和一定仍>0 ，即后(n−a) 个数字的众仍为 x 。

根据以上推论，假设数组首个元素 $n_1$为众数，遍历并统计票数。当发生票数和=0 时，剩余数组的众数一定不变 ，这是由于：

当 $n_1=x$ ： 抵消的所有数字，有一半是众数 x 。

当$ n_1 \neq x$： 抵消的所有数字，少于或等于一半是众数 x 。

利用此特性，每轮假设发生票数和=0 都可以缩小剩余数组区间 。当遍历完成时，最后一轮假设的数字即为众数。

算法流程:

- 初始化： 票数统计votes = 0 ， 众数 x；
- 循环： 遍历数组nums 中的每个数字 num ；
- 当 票数 votes 等于 0 ，则假设当前数字 num 是众数；
- 当 num = x 时，票数 votes 自增 1 ；当 num != x 时，票数 votes 自减 1 ；
- 返回值： 返回 x 即可；



**时间复杂度**：$O(n)$，遍历一遍数组

**空间复杂度**：$O(1)$，不需要额外的空间

#### 代码

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int votes = 0, num;
        for (auto &i : nums) {
            if (votes == 0) {
                num = i;
                votes++;
            }else {
                if (i == num) {
                    votes++;
                }else {
                    votes--;
                }
            }
        }
        return num;
    }
};
```


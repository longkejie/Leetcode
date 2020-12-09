# [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

参考以下这颗二叉搜索树：

![image-20201209164243671](https://gitee.com/long_kejie/image/raw/master/image-20201209164243671.png)

示例 1：

输入: [1,6,3,2,5]

输出: false

示例 2：

输入: [1,3,2,6,5]

输出: true

提示：

数组长度 <= 1000

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 思路

### 方法1：递归

对于该题我们可以利用递归来对这棵树，以及这棵树的左子树和右子树来进行判断它是不是一颗二叉搜索树，从而得到总体的一颗树是不是一颗二叉搜索树。

首先，我们需要明白对于二叉搜索树中的每个结点，它的左子树一定小于它的结点值，它的右子树一定大于它的结点值。

然后对于后续遍历，它先遍历左子树，然后再遍历右子树，最后再遍历我们的根结点。

所以对于题目给我们的一个数组（后续遍历），我们可以遍历一遍数组，找到所有比数组末尾元素小的值，放入左子树的数组集合中，反之放到右子树数组集合中。当我们遍历到某个元素的值小于末尾元素，但是它前面出现过比末尾元素大的值，这时它一定不满足二叉搜索树的性质，直接返回**false**,结束递归。

那我们递归到什么时候就可以结束掉我们的递归，并返回**true**呢？

当我们递归到树的结点个数小于等于2时，这时这颗子树一定满足二叉搜索树的性质，返回**true**即可。

接下来就是代码演示



#### 代码

```cpp
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        int n = postorder.size() - 1;
        if (n <= 1) {
            return true;
        }
        vector<int> temp1;
        vector<int> temp2;
        int flag = 0;
        for (auto &i: postorder) {
            if (i < postorder[n] && !flag) {
                temp1.push_back(i);
            }else if (i > postorder[n] || flag) {
                flag = 1;
                if (i < postorder[n]) {
                    return false;
                }else if (i > postorder[n]){
                    temp2.push_back(i);
                }
            }
        }
        return verifyPostorder(temp1) && verifyPostorder(temp2);
        return true;
    }
};
```


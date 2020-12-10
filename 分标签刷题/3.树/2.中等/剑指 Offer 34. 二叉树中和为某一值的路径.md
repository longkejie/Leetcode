# [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。



**示例:**
给定如下二叉树，以及目标和 sum = 22，



![image-20201210133840490](https://gitee.com/long_kejie/image/raw/master/image-20201210133840490.png)

返回:

```c++
[
   [5,4,11,2],
   [5,8,4,5]
]
```

**提示：**

1. 节点总数 <= 10000



## 思路

### 方法1：递归+回溯

先序遍历： 按照 “根、左、右” 的顺序，遍历树的所有节点。
路径记录： 在先序遍历中，记录从根节点到当前节点的路径。当路径为 ① 根节点到叶节点形成的路径 且 ② 各节点值的和等于目标值 sum 时，将此路径加入结果列表。

递推参数： 当前节点 root ，当前目标值 sum。
终止条件： 若节点 root 为空，则直接返回。
递推工作：
路径更新： 将当前节点值 root->val 加入路径 path ；
目标值更新： sum= sum - root.val（即目标值 sum 减至 0 ）；
路径记录： 当 ① root 为叶节点 且 ② 路径和等于目标值 ，则将此路径 path 加入 ans 。
先序遍历： 递归左 / 右子节点。
路径恢复： 向上回溯前，需要将当前节点从路径 path 中删除，即执行 path.pop_back() 。

代码如下，递归回溯还得多练。

#### 代码

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int > temp;
    vector<vector<int> > ans;
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if (root == nullptr) {
            return {};
        }
        path(root, sum);
        return ans;
    }
    void path(TreeNode* root, int sum ) {
        if (root == nullptr) {
            temp.push_back(1);
            return ;
        }
        if (root -> val == sum && root -> left == nullptr && root -> right == nullptr) {
            temp.push_back(root -> val);
            ans.push_back(temp);
            return ;
        }
        temp.push_back(root -> val);
        path(root->left,sum - root->val);
        temp.pop_back();
        path(root->right,sum - root->val);
        temp.pop_back();
        return;
    }
};
```


# [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/binarytree.png)

 

示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1

输出: 3

解释: 节点 5 和节点 1 的最近公共祖先是节点 3。

示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4

输出: 5

解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。

说明:

所有节点的值都是唯一的。

p、q 为不同节点且均存在于给定的二叉树中。

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路

### 方法1:递归+队列

本题我们使用递归去寻找p，以及q，找到后利用回溯将它们的路径保存到两个队列中（从终点开始往根结点存，终点在队首，这样就可保证找到的第一个为最近公共祖先），然后在队列中找第一个相同的结点，那么这个结点就是我们的最近公共祖先。

值得注意的是，当我们的队列长度不同时，队列长度小的不需出队，当队列长度一样时，两个队列的元素要一起出队。

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        queue<TreeNode* > que;
        queue<TreeNode* > que1;
        if (!root) return NULL;
        find_node(root, p,que);
        find_node(root, q,que1);
        while (que.front() != que1.front()) {
            if (que.size() > que1.size()) {
                que.pop();
            }else if (que.size() < que1.size()) {
                que1.pop();
            }else {
                que.pop();
                que1.pop();
            }
        }
        return que.front();
    }
    TreeNode *find_node(TreeNode *root, TreeNode* q, queue<TreeNode *> &que) {
        if (root == NULL) return NULL;
        if (root != q) {
            if (find_node(root -> left, q, que)) {
                que.push(root);
                return root;
            }
            if (find_node(root -> right, q, que)) {
                que.push(root);
                return root;
            }
            return NULL;
        }
        que.push(root);
        return root;
    }
};
```


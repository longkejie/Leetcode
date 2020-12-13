# [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

为了让您更好地理解问题，以下面的二叉搜索树为例：

<img src="https://assets.leetcode.com/uploads/2018/10/12/bstdlloriginalbst.png" alt="img" style="zoom:67%;" />

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。

<img src="https://assets.leetcode.com/uploads/2018/10/12/bstdllreturndll.png" alt="img" style="zoom:67%;" />

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。	

## 思路

### 方法1：递归分治

我们先找到二叉搜索树的最小结点以及最大结点，作为我们链表的头结点和尾结点。

然后从根结点开始递归，对于每个结点，我们依次判断它的左节点是否为空，右节点是否为空。

左节点不为空的话，我们找到它左子树中最大的那个结点，然后将这个最大的结点的后继指向该结点，该结点的前驱指向这个最大的结点。

对于右节点不为空的情况，我们找右子树中最小的结点，然后将最小结点的前驱指向该结点，该结点的后继指向这个最小的结点。

然后再次递归左子树和右子树，值得注意的是，我们需要在递归左子树和右子树后，才能改变左子树和右子树的相关指向。

**时间复杂度**O(nlon),对于每个结点我们需要找它的前驱和后继，每次需要O(logn)

#### 代码

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if (root == NULL) {
            return root;
        }
        Node *head = findleft(root);
        Node *tail = findright(root);
        tran(root);
        head -> left = tail;
        tail -> right = head;
        return head;
    }
    Node *findleft(Node *root) {
        return root -> left == NULL ? root : findleft(root -> left);
    }
    Node *findright(Node *root) {
        return root -> right == NULL ? root : findright(root -> right);
    }
    void tran(Node *root) {
        if (root == NULL) return;
        if (root -> left != NULL) {
            Node *node = root->left;
            Node *mmax = findright(node);
            root -> left = findright(node);
            tran(node);
            mmax -> right = root;
        }
        if (root -> right != NULL) {
            Node *node = root -> right;
            Node *mmin = findleft(node);
            root -> right = mmin;
            tran(node);
            mmin -> left = root;
        }
        return ;
    }
};
```



### 方法2：后续遍历+队列

题解里有这里就不详细说了。
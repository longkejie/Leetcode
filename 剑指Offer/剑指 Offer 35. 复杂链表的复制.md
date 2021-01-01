# [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

 

示例 1：

![img](https://gitee.com/long_kejie/image/raw/master/e1.png)

输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]

输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

示例 2：

![img](https://gitee.com/long_kejie/image/raw/master/e2.png)

输入：head = [[1,1],[2,1]]

输出：[[1,1],[2,1]]

示例 3：

![img](https://gitee.com/long_kejie/image/raw/master/e3.png)

输入：head = [[3,null],[3,0],[3,null]]

输出：[[3,null],[3,0],[3,null]]

示例 4：

输入：head = []

输出：[]

解释：给定的链表为空（空指针），因此返回 null。

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 思路

### 方法1：哈希+扫描



本题是一个关于链表复制的题，我们需要对原有的链表进行深拷贝，将链表拷贝到新的地址。

本题的难点在于，**random**指针究竟是什么？它指向的是链表中的第几个节点。

对于上述难点，我们可以使用一个哈希表来映射原链表的每个结点和目标链表的结点，一一对应。

我们首先将原有链表中除了random变量，全部拷贝到新的链表中，并利用哈希表在新旧两个结点之间建立映射关系。

然后我们使用两个指针，一个从原链表头部开始遍历，一个从新链表头部开始遍历。

当我们遍历到原链表中的**random**指针不为NULL时，我们就利用哈希表，映射出原链表中的**random**对应的是新链表中的哪个节点，然后将新链表中遍历到的那个指针指向该映射出来的结点即可。



**时间复杂度**：O(n)

**空间复杂度**：O(n)//遍历两边即可

解释的可能不是很清楚，看代码就清晰很多了。

#### 代码

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    int cnt = 0;
    Node* copyRandomList(Node* head) {
        if (head == nullptr) {
            return NULL;
        }
        unordered_map<Node*, Node *> mmap;
        Node *node = head -> next;
        Node *head1 = new Node(head->val);
        Node *temp = head1;
        mmap[head] = head1;
        while (node != NULL) {
            Node *node1 = new Node (node -> val);
            mmap[node] = node1;
            temp -> next = node1;
            temp = temp -> next;
            node = node -> next;
        }
        temp -> next = NULL;
        temp = head1;
        while (head) {
            if (head -> random != NULL) {
                temp -> random = mmap[head->random];
            }
            temp = temp -> next;
            head = head -> next;
        }
        return head1;
    }
};
```


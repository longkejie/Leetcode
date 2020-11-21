# Leetcode每日一题

## 前言

有一个督促自己的作用吧，从今天开始**(2020-11-20)**起每天最少也要把每日一题做了

### 2020-11-20

#### [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

对链表进行插入排序。



插入排序算法：

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。


示例 1：

输入: 4->2->1->3
输出: 1->2->3->4

示例 2：

输入: -1->5->3->4->0
输出: -1->0->3->4->5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/insertion-sort-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



##### 思路

1.首先我们需要了解插入排序的基本思想，它分为两个集合，一个有序集合和一个无序集合。每次我们需要从无序集合中选取一个元素插入到有序集合的相应位置，直到无序集合为空为止。

2.了解了插入排序后，我们就可以来讨论如何进行链表的插入排序呢。链表是各种地址链接在一起的集合，在进行插入和删除时，我们应该尤其小心，不然则会导致内存溢出，指针乱指的情况。

3.本题我们首先假设有序集合开始只有一个元素，那就是我们的头结点，为了方便在头部进行插入，我们可以设置一个哑结点，它只是一个虚拟的结点，它指向我们真实的头结点，我们返回的时候，只要返回这个哑结点的next结点即可。

4.我们需要保存的结点有有序集合的尾部结点，这样插入的时候才能方便我们遍历整个有序(集合)链表来寻找相应的插入位置，然后还需要保存无序(集合)链表的第一个结点，和第二个结点，因为当我们将无序(集合)链表的第一个结点插入到有序链表时，我们该怎么获得无序链表的第二个结点呢？这样将会进行不下去，所以应该存储第一个结点和第二个结点。

5.下面我们就可以在有序链表中遍历查找适合无序链表第一个结点插入的位置了，插入后，我们将无序链表第一个结点，指向无序链表的第二个结点，然后重新执行上述插入操作，将无序链表第二个结点也插入进来，直到无序链表没有结点可以插入。

6.值得注意的时，当我们要插入进来的无序链表结点时，如果大于我们有序链表的尾部结点（tail)，在插入后，我们应该修改我们的tail结点为这个插入进来的结点。

基于上述几点，我们就可以来实现我们的代码了。

###### 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if (head == nullptr) {
            return head;
        }
        ListNode* HEAD = new ListNode(0);
        HEAD -> next = head;
        ListNode* current_node = head -> next;
        ListNode* tail = head;
        head -> next = NULL;
        while (current_node != NULL) {
            ListNode* temp = current_node -> next;
            head = HEAD;
            if (current_node -> val > tail->val) {
                head = tail;
            }
            while (head != tail && head -> next -> val  <= current_node -> val) {
                head = head -> next;
            }
            current_node -> next = head -> next;
            head -> next = current_node;
            if (head == tail) tail = current_node;
            current_node = temp;
        }
        return HEAD -> next;
    }
};
```



### 2020-11-21

#### [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

进阶：

你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

示例 1：

![img](https://gitee.com/long_kejie/image/raw/master/sort_list_1.jpg)

输入：head = [4,2,1,3]
输出：[1,2,3,4]

示例 2：

![img](https://gitee.com/long_kejie/image/raw/master/sort_list_2.jpg)

输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]

示例 3：

输入：head = []
输出：[]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sort-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

##### 思路

本题和昨日的[147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)感觉是差不多的，都是对链表进行排序，我们将昨天那个代码，一贴，一提交肯定也是可以过的，结果如下：

##### 方法1：插入排序

![image-20201121115902749](https://gitee.com/long_kejie/image/raw/master/image-20201121115902749.png)

但是这个时间是不是有点太长了？

因为插入排序的时间复杂度为**O(n^2)**,题目要求我们在**O(nlogn)** 时间内完成，所以我们肯定是不能再利用插入排序来做了。

###### 代码

代码和昨天的一样就不贴了。

##### 方法2：利用multimap

我们利用STL内的关联容器multimap来进行排序，它内部是红黑树实现的，排序时间复杂度为**O(nlogn)**（每次插入的时间复杂度为O(logn）插入n次）,空间复杂度为**O(n)**（存n个结点）。

我们定义key值为每个结点的val值，根据这个值的大小来排序，然后依次将每个结点插入即可。

插入完成后，我们遍历我们的整个map，依次取出，然后就是一个有序的链表了。

###### 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (head == nullptr) return head;
        multimap<int,ListNode*> mmap;
        ListNode* HEAD = new ListNode(0);
        while (head != NULL) {
            mmap.insert(multimap<int, ListNode*>::value_type(head -> val, head));
            head = head -> next;
        }
        ListNode* tail = HEAD;
        for (auto i : mmap) {
            tail -> next = i.second;
            tail = tail -> next;
        }
        tail->next = NULL;
        return HEAD -> next;
    }
};
```

結果如下：

![image-20201121123814036](https://gitee.com/long_kejie/image/raw/master/image-20201121123814036.png)

比插入排序快一点，但是好像也没好到哪里去。
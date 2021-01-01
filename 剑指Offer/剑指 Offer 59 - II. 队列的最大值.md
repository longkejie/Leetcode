# [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

若队列为空，pop_front 和 max_value 需要返回 -1



示例 1：

输入: 

["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]

[[[[[],[1],[2],[],[],[]]

输出: [null,null,null,2,1,2]



示例 2：

输入: 

["MaxQueue","pop_front","max_value"]

[[[],[],[]]

输出: [null,-1,-1]

限制：

1 <= push_back,pop_front,max_value的总操作数 <= 10000

1 <= value <= 10^5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 思路

### 方法1:维护单调递减的双端队列

我们使用两个队列来完成这题，对于第一个队列和正常队列一样插入数据，弹出数据

本题我们的难点是，如何维护每次队列中的最大值，想了很久，我决定使用一个双端队列，用其维护队列中的最大值。

方法如下

- 当队列为空时，直接将该值压入两个队列。
- 当队列不为空时，我们就需要循环判断该值和双端队列队尾元素值的大小关系，如果双端队列的队尾元素小于插入进来的值的话，我们就应该一直出队，因为这个新插入来的值，一定比前面插入来的值后$pop$,所以它可以将所有小于它的值都去除掉。然后将其插入双端队列即可
- 我们的双端队列是单调递减的，所以双端队列的头部维护着我们队列中的最大值
- 当我们pop时，需要判断第一个队列中被pop掉的值（头部）是否和我们双端队列中头部的值相等，如果相等的话，需要将双端队列中头部的值也pop掉。以维护队列中的最大值。

上述就是我们本题的难点，其他就不细说了。



**时间复杂度**：$O(1)$（插入，删除，求最大值）
删除操作与求最大值操作显然只需要 O(1)的时间。
而插入操作虽然看起来有循环，做一个插入操作时最多可能会有 n次出队操作。但要注意，由于每个数字只会出队一次，因此对于所有的 n个数字的插入过程，对应的所有出队操作也不会大于 n次。因此将出队的时间均摊到每个插入操作上，时间复杂度为 $O(1)$。

**空间复杂度**：$O(n)$，需要用队列存储所有插入的元素。

#### 代码

```cpp
class MaxQueue {
public:
    queue<int> que;
    deque <int> deq;
    MaxQueue() {
    }
    
    int max_value() {
        if (que.empty()) return -1;
        return deq.front();
    }
    
    void push_back(int value) {
        if (que.empty()) {
            que.push(value);
            deq.push_back(value);
        }else {
            while (deq.size() && value > deq.back()) {
                deq.pop_back();
            }
            deq.push_back(value);
            que.push(value);
        }
    }
    int pop_front() {
        if (que.empty()) return -1;
        int temp = que.front();
        if (que.front() == deq.front()) {
            deq.pop_front();
        }
        que.pop();
        return temp;
    }
};
/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
```


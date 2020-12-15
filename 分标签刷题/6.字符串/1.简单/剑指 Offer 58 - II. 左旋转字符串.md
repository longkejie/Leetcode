# [剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

示例 1：

输入: s = "abcdefg", k = 2

输出: "cdefgab"

示例 2：

输入: s = "lrloseumgh", k = 6

输出: "umghlrlose"

限制：

1 <= k < s.length <= 10000

来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



## 思路

### 方法1：双指针+扫描

我们利用一个左指针和一个右指针来对字符串进行扫描，当n大于0时，将左指针的字符加到目标字符串，左指针加一，n减一，当n等于0时，我们将右指针的字符加到目标字符串（注意的是前面是加到目标字符串后面，这里是加到目标字符串前面），右指针减一。当左指针大于右指针时，退出循环，返回目标字符串即可。

**时间复杂度**：O(k),k为字符串长度

**空间复杂度**：O(1),不需要额外的空间

#### 代码

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        string ans = "";
        if (n == 0) {
            return s;
        }
        int l = 0, r = s.size() - 1;
        while (l <= r) {
            if (n) {
                ans += s[l++];
                n--;
            }else {
                ans = s[r--] + ans;
            }
        }
        return ans;
    }
};
```

### 方法2：拼接+截取（学废了）

我们在原字符串后再加一个一样的字符串，然后从这个有两倍长度的字符串的第n个位置开始截取原字符串长度，返回即可。

一行代码即可完成

#### 代码

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        return (s + s).substr(n, s.size());
    }
};
```


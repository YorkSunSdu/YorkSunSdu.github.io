---
layout: post
title: 哈希表
date: 2022-05-17
tags: 算法
---

博主对leetcode中的有关**哈希表**的题目作了总结。

题目来源[leetcode](https://leetcode-cn.com/)，点击题目即可直达leetcode对应题目。

## [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

### 题目

给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

**注意**：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。


示例 1:
> 输入: s = "anagram", t = "nagaram"<br />
> 输出: true<br />

示例 2:
> 输入: s = "rat", t = "car"<br />
> 输出: false

提示:<br />
1 <= s.length, t.length <= 5 * 10^4<br />
s 和 t 仅包含小写字母

进阶: 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？



### 解法

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int table[26] = {0};
        if(s.size() != t.size())
            return false;
        int n = s.size();
        for(int i = 0; i < n; i++)
        {
            table[s[i] - 'a']++;
            table[t[i] - 'a']--;
        }
        for(int i = 0; i < 26; i++)
        {
            if(table[i] != 0)
                return false;
        }
        return true;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(26)



## [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

### 题目

给定两个数组 nums1 和 nums2 ，返回 它们的交集 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。


示例 1：
> 输入：nums1 = [1,2,2,1], nums2 = [2,2]<br />
> 输出：[2]

示例 2：
> 输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]<br />
> 输出：[9,4]<br />
> 解释：[4,9] 也是可通过的

提示：<br />
1 <= nums1.length, nums2.length <= 1000<br />
0 <= nums1[i], nums2[i] <= 1000



### 解法

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
		vector<int> vec;
        // 注意下面这个申请哈希集合空间的方法
		unordered_set<int> set(nums1.begin(),nums1.end());

		for (int i = 0; i < nums2.size(); i++) {
			if (set.count(nums2[i])) {
				vec.push_back(nums2[i]);
				set.erase(nums2[i]);
			}
		}
		return vec;
    }
};
```

时间复杂度：O(m + n)

空间复杂度：O(m + n)



## [202. 快乐数](https://leetcode-cn.com/problems/happy-number/)

### 题目

编写一个算法来判断一个数 n 是不是快乐数。

**「快乐数」** 定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。
如果这个过程 **结果为 1**，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false 。


示例 1：
> 输入：n = 19<br />
> 输出：true<br />
> 解释：<br />
> 1^2 + 9^2 = 82<br />
> 8^2 + 2^2 = 68<br />
> 6^2 + 8^2 = 100<br />
> 1^2 + 0^2 + 0^2 = 1

示例 2：
> 输入：n = 2<br />
> 输出：false

提示：<br />
1 <= n <= 2^31 - 1



### 解法一

哈希表。

```c++
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> set;
        while(1)
        {
            int sum = 0, num = n;
            // while循环中计算各位数字的平方和
            while(num)
            {
                sum += (num % 10) * (num % 10);
                num /= 10;
            }
            if(sum == 1)
                return true;
            if(set.count(sum))
                return false;
            else
                set.insert(sum);
            n = sum;
        }
    }
};
```

时间复杂度：O(log n)

空间复杂度：O(log n)



### 解法二

快慢指针。

注：只要判断循环就可以用快慢指针。

```c++
class Solution {
public:
    // 此方法计算各位数字的平方和
    int bitSquareSum(int n) {
        int sum = 0;
        while(n > 0)
        {
            int bit = n % 10;
            sum += bit * bit;
            n = n / 10;
        }
        return sum;
    }
    
    bool isHappy(int n) {
        int slow = n, fast = n;
        do{
            // 慢指针一次移动一次
            slow = bitSquareSum(slow);
            // 快指针一次移动两次
            fast = bitSquareSum(fast);
            fast = bitSquareSum(fast);
        }while(slow != fast);
        
        // 当slow == fast时跳出while循环，此时slow == fast有两种情况
        // 1. slow == fast == 1，说明是快乐数
        // 2. slow和fast在循环中相遇，说明不是快乐数，slow == fast != 1
        return slow == 1;
    }
};
```

时间复杂度：O(log n)

空间复杂度：O(1)

解法来自[linux-man](https://leetcode-cn.com/problems/happy-number/solution/shi-yong-kuai-man-zhi-zhen-si-xiang-zhao-chu-xun-h/)。



## 总结

+ unordered_set的插入：set.insert(num)
+ unordered_set的元素个数计数：set.count(num)
+ unordered_set的寻找元素：set.find(num)
+ 判断num是否在set中存在时，下面两个if语句等价：
  + if(set.find(num)  !=  set.end())
  + if(set.count(num))

<div style="color: red; font-size:24px">商业转载请联系博主获得授权，非商业转载请注明出处！</div>

分享结束，大家辛苦了。散会！


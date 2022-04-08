---
layout: post
title: 滑动窗口
date: 2022-04-08
tags: 算法
---

博主对leetcode中可以使用**滑动窗口**解题的有关数组的题目作了总结。

题目来源[leetcode](https://leetcode-cn.com/)，点击题目即可直达leetcode对应题目。

## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

### 题目

给定一个字符串 s ，请你找出其中不含有重复字符的 **最长子串** 的长度。

示例 1:
> 输入: s = "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:
> 输入: s = "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:
> 输入: s = "pwwkew"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
>      请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

提示：
0 <= s.length <= 5 * 10^4
s 由英文字母、数字、符号和空格组成

### 解法

```c++
class Solution {
    // 复制粘贴的
    // 首先学习了散列表（哈希表）
    // unordered_set是无序集，用find()函数返回元素位置，用count()函数查找元素个数，用erase()函数删除元素
public:
    int lengthOfLongestSubstring(string s) {
        // 选择窗口类型
        unordered_set<char> occ;
        int n = s.size();
        // 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        int rk = -1, ans = 0;
        // 枚举左指针的位置，初始值隐性地表示为 -1
        for (int i = 0; i < n; ++i) {
            if (i != 0) {
                // 左指针向右移动一格，即移除左指针指向的字符
                occ.erase(s[i - 1]);
            }
            while (rk + 1 < n && !occ.count(s[rk + 1])) {
                // 不断地移动右指针，直到遇到重复的字符或者遍历完毕整个字符串
                occ.insert(s[rk + 1]);
                ++rk;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = max(ans, rk - i + 1);
        }
        return ans;
    }
};
```

时间复杂度：O(n)，其中 n 是字符串的长度。左指针和右指针分别会遍历整个字符串一次。

空间复杂度：O(∣Σ∣)，其中Σ 表示字符集（即字符串中可以出现的字符），∣Σ∣ 表示字符集的大小。在本题中没有明确说明字符集，因此可以默认为所有 ASCII 码在 [0,128) 内的字符，即∣Σ∣=128。我们需要用到哈希集合来存储出现过的字符，而字符最多有∣Σ∣ 个，因此空间复杂度为O(∣Σ∣)。



## [209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

### 题目

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 **连续子数组** [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

示例 1：

> 输入：target = 7, nums = [2,3,1,2,4,3]
> 输出：2
> 解释：子数组 [4,3] 是该条件下的长度最小的子数组。

示例 2：

> 输入：target = 4, nums = [1,4,4]
> 输出：1

示例 3：

> 输入：target = 11, nums = [1,1,1,1,1,1,1,1]
> 输出：0

提示：
1 <= target <= 10^9
1 <= nums.length <= 10^5
1 <= nums[i] <= 10^5



### 解法

```c++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int ans = INT_MAX;
        int start = 0, end = 0, sum = 0;
        while (end < n) 
        // 右指针依次遍历直到遍历完整个数组
        {
            sum += nums[end]; // 给sum加上右指针指向的元素
            while (sum >= s) { // 如果目前的sum比目标值s大或相等
                ans = min(ans, end - start + 1); // 判断是否更新最小长度ans
                // 将左指针指向的元素在sum中减去；
                sum -= nums[start]; 
                // 右移左指针，继续判断右指针不动的情况下是否还能更短
                start++;
            }
            end++;
        }
        return ans == INT_MAX ? 0 : ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)



## [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

### 题目

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。

示例 1：
> 输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
> 输出：[3,3,5,5,6,7]
> 解释：
>    滑动窗口的位置        最大值
>
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1  [3  -1  -3] 5  3  6  7       3
>  1   3 [-1  -3  5] 3  6  7       5
>  1   3  -1 [-3  5  3] 6  7       5
>  1   3  -1  -3 [5  3  6] 7       6
>  1   3  -1  -3  5 [3  6  7]      7

示例 2：
> 输入：nums = [1], k = 1
> 输出：[1]

提示：
1 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4
1 <= k <= nums.length

### 解法一

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        // 选择窗口类型
        priority_queue<pair<int, int>> q; // 优先队列
        for (int i = 0; i < k; ++i) {
            q.emplace(nums[i], i); // (优先级(当前数), 下标)
        }
        vector<int> ans = {q.top().first}; // ans[1] = 前k个元素最大值
        for (int i = k; i < n; ++i) { // 每层for循环滑动窗口都滑动一次
            q.emplace(nums[i], i); // 向优先级队列添加新增的一个元素
            while (q.top().second <= i - k) { // 最大的元素被移出去
            // 注意上面的语句也不能用if，也不能用==，因为要确保把之前的全部移出
                q.pop();
            }
            ans.push_back(q.top().first); // 将目前窗口中的最大值保存到ans中
        }
        return ans;
    }
};
```

时间复杂度：O(nlog n)，一共有n个元素，将一个元素放入优先队列的复杂度为O(log n)

空间复杂度：O(n)，优先队列使用的空间

### 解法二

对解法一进行优化。

如果j > i，且nums[j] >= nums[i]，那么nums[i]肯定不会出现在ans数组中，因此可以将满足这样条件的i直接删掉。

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        // 选择窗口类型
        deque<int> q;
        // deque是双向的队列，允许高效地插入/删除位于队首/队尾的元素
        // 这里的q只存储下标
        for (int i = 0; i < k; ++i) {
            // 如果此时q非空，且要插入的元素比前面的元素都要大
            while (!q.empty() && nums[i] >= nums[q.back()]) {
                // 把比即将插入的元素小的下标全部删掉
                q.pop_back();
                // 因为把前面的小元素的下标全删了（留下的都比当前大），
                // 所以可以保证q中下标从首到尾对应的元素是递减的
            }
            // 把当前元素的下标插入队尾，可以保证q(存储下标)是从首到尾递增的队列
            q.push_back(i);
        }

        // 把前k个元素中最大的，也就是下标位于q队首的赋给ans[0]
        vector<int> ans = {nums[q.front()]};
        for (int i = k; i < n; ++i) {
            while (!q.empty() && nums[i] >= nums[q.back()]) {
                q.pop_back();
            }
            q.push_back(i);
            while (q.front() <= i - k) {
                // 如果最大值被移出窗口，就把队首的下标删掉，这样次大值就成了最大值，
                // 再用while循环的条件判断这个次大值是否被移出窗口
                q.pop_front();
            }
            ans.push_back(nums[q.front()]);
        }
        return ans;
    }
};
```

时间复杂度：O(n)，每一个下标恰好被放入队列一次，并且最多被弹出队列一次。

空间复杂度：O(k)，队列中不会有超过k + 1个元素。



## [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

跟[567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)几乎一样，略。



## [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

### 题目

给你两个字符串 s1 和 s2 ，写一个函数来判断 s2 是否包含 s1 的排列。如果是，返回 true ；否则，返回 false 。

换句话说，s1 的排列之一是 s2 的 **子串** 。

示例 1：
> 输入：s1 = "ab" s2 = "eidbaooo"
> 输出：true
> 解释：s2 包含 s1 的排列之一 ("ba").

示例 2：
> 输入：s1= "ab" s2 = "eidboaoo"
> 输出：false

提示：
1 <= s1.length, s2.length <= 104
s1 和 s2 仅包含小写字母


### 解法一

```c++
class Solution {
    // s1的长度为n，首先计算s1中每个字母出现的次数，然后遍历s2的连续n位，
    // 如果s2的这n位每个字母的个数和s1相等，则s2的这n位为s1的排列组合
public:
    bool checkInclusion(string s1, string s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        // 选择窗口类型
        vector<int> cnt1(26), cnt2(26);
        for (int i = 0; i < n; ++i) { // 计算s1和s2的前n位每个字母各出现几次
            ++cnt1[s1[i] - 'a'];
            ++cnt2[s2[i] - 'a'];
        }
        if (cnt1 == cnt2) { // s1和s2的前n位每个字母出现的次数相等
            return true;
        }
        // 注意vector类型的cnt1和cnt2可以直接用==符号判断是否相同
        for (int i = n; i < m; ++i) {
            // 下面两行将s2滑动一位
            ++cnt2[s2[i] - 'a'];
            --cnt2[s2[i - n] - 'a'];
            if (cnt1 == cnt2) {
                return true;
            }
        }
        return false;
    }
};
```

时间复杂度：O(m + ( n - m ) * ∣Σ∣)，需要O(m)来统计字符串p中每种字母的数量；需要O(m)来初始化滑动窗口；需要判断n−m+1个滑动窗口中每种字母的数量是否与字符串p中每种字母的数量相同，每次判断需要Σ。Σ 是字符集，这道题中的字符集是小写字母，∣Σ∣=26。

空间复杂度：O(∣Σ∣)

### 解法二

对解法一进行优化。可读性变差预警！

```c++
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        // 选择窗口类型
        vector<int> cnt(26);
        // 将26个字母放在一个容器中，对两个字符串的前n项遍历时在这一个容器中进行操作
        for (int i = 0; i < n; ++i) {
            --cnt[s1[i] - 'a'];
            ++cnt[s2[i] - 'a'];
        }
        // diff为s1与s2的数目不同的字符个数
        int diff = 0;
        for (int c: cnt) {
            if (c != 0) {
                ++diff;
            }
        }
        // 将cnt遍历一遍，如果diff为0，说明s1和s2前n个字符的字符数相同，直接返回true
        if (diff == 0) {
            return true;
        }
        // s1和s2前n个字符的字符数不同，将n位的窗口在s2上进行滑动
        for (int i = n; i < m; ++i) {
            // x为s2滑入的字符，y为s2滑出的字符
            int x = s2[i] - 'a', y = s2[i - n] - 'a';
            // 前面的字符不同，这一位字符相同没产生变化，总字符肯定还是不同；
            // 也不需要对cnt进行操作。因此跳出此次循环。
            if (x == y) {
                continue;
            }
            // cnt[x] == 0代表这次滑动之前的n位中，s1和s2的这个字符数相同，
            // 再加了一个，那这个字符数肯定不同了
            if (cnt[x] == 0) {
                ++diff;
            }
            // 对s2增加字符，对应的是cnt++
            ++cnt[x];
            // 增加了这一个字符后，这个字符在s1和s2的这n位相同了，那么diff--
            if (cnt[x] == 0) {
                --diff;
            }
            if (cnt[y] == 0) {
                ++diff;
            }
            // 对s2减少字符，对应的是cnt--
            --cnt[y];
            if (cnt[y] == 0) {
                --diff;
            }
            // 判断一下滑动这次后，s1与s2的这n位窗口是否满足排列
            if (diff == 0) {
                return true;
            }
        }
        return false;
    }
};
```

时间复杂度：O(n+m+|Σ|)

空间复杂度：O(∣Σ∣)



## [904. 水果成篮](https://leetcode-cn.com/problems/fruit-into-baskets/)

### 题目

你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 fruits 表示，其中 fruits[i] 是第 i 棵树上的水果 **种类** 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

+ 你只有 **两个** 篮子，并且每个篮子只能装 **单一类型** 的水果。每个篮子能够装的水果总量没有限制。

+ 你可以选择任意一棵树开始采摘，你必须从 **每棵** 树（包括开始采摘的树）上 **恰好摘一个水果** 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。

+ 一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。

给你一个整数数组 fruits ，返回你可以收集的水果的 **最大** 数目。

示例 1：
> 输入：fruits = [1,2,1]
> 输出：3
> 解释：可以采摘全部 3 棵树。

示例 2：
> 输入：fruits = [0,1,2,2]
> 输出：3
> 解释：可以采摘 [1,2,2] 这三棵树。
> 如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。

示例 3：
> 输入：fruits = [1,2,3,2,2]
> 输出：4
> 解释：可以采摘 [2,3,2,2] 这四棵树。
> 如果从第一棵树开始采摘，则只能采摘 [1,2] 这两棵树。

示例 4：
> 输入：fruits = [3,3,3,1,2,1,1,2,3,3,4]
> 输出：5
> 解释：可以采摘 [1,2,1,1,2] 这五棵树。

提示：
1 <= fruits.length <= 10^5
0 <= fruits[i] < fruits.length



### 解法

```c++
class Solution {
public:
    int totalFruit(vector<int>& tree) {
        // 选择窗口类型
        unordered_map<int, int> window;
        int res = 0;
        // 采用双指针，分别指向窗口的首尾
        for (int left = 0, right = 0; right < tree.size(); right++) {
            window[tree[right]]++;
            // 树的种类超过两种，一直删除左指针指向的树种，直到树的种类为两种
            while (window.size() > 2) {
                window[tree[left]]--;
                if (window[tree[left]] == 0) {
                    window.erase(tree[left]);
                }
                left++;
            }
            res = max(res, right - left + 1);
        }
        return res;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)



## 总结

+ 滑动窗口未必用双指针。可能使用双指针，分别指向窗口的首尾，例如[209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)；也可能使用一个指针，即for循环中的i，指向窗口的尾端，例如[239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)。
+ 滑动窗口题目中，对窗口容器的选择很重要，要根据需求，在vector, unordered_set, unordered_map, deque, priority_queue等类型中选择合适的窗口类型。

<div style="color: red; font-size:24px">商业转载请联系博主获得授权，非商业转载请注明出处！</div>

分享结束，大家辛苦了。散会！


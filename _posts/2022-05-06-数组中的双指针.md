---
layout: post
title: 数组中的双指针
date: 2022-05-06
tags: 算法
---

博主对leetcode中可以使用**双指针**解题的有关数组的题目作了总结。

题目来源[leetcode](https://leetcode-cn.com/)，点击题目链接即可直达leetcode对应题目。



## 27.移除元素

### 题目

题目链接：[27.移除元素](https://leetcode-cn.com/problems/remove-element/)

给你一个数组 nums 和一个值 val，你需要**原地**移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

> // nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝<br />
> int len = removeElement(nums, val);
>
> // 在函数里修改输入数组对于调用者是可见的。<br />
> // 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。<br />
> for (int i = 0; i < len; i++) {<br />
>     print(nums[i]);<br />
> }

示例 1：

> 输入：nums = [3,2,2,3], val = 3<br />
> 输出：2, nums = [2,2]<br />
> 解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。

示例 2：
> 输入：nums = [0,1,2,2,3,0,4,2], val = 2<br />
> 输出：5, nums = [0,1,4,0,3]<br />
> 解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。

提示：<br />
0 <= nums.length <= 100<br />
0 <= nums[i] <= 50<br />
0 <= val <= 100

### 解法

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int l = 0, r = 0, len = nums.size();
        while(r < len)
        {
            if(nums[r] != val)
            {
                nums[l] = nums[r];
                l++;
            }
            r++;   
        }
        return l;
    }
};
```

如果右指针指向的元素不等于val，它一定是输出数组的一个元素，我们就将右指针指向的元素复制到左指针位置，然后将左右指针同时右移；

如果右指针指向的元素等于val，它不能在输出数组里，此时左指针不动，右指针右移一位。

时间复杂度：O(n)，最多遍历数组两遍

空间复杂度：O(1)

### 解法优化

如果要移除的元素恰好在数组的开头，例如序列 [1,2,3,4,5]，当val 为 1 时，我们需要把每一个元素都左移一位。注意到题目中说：「元素的顺序可以改变」。实际上我们可以直接将最后一个元素 5 移动到序列开头，取代元素 1，得到序列 [5,2,3,4]，同样满足题目要求。这个优化在序列中 val 元素的数量较少时非常有效。

实现方面，我们依然使用双指针，两个指针初始时分别位于数组的首尾，向中间移动遍历该序列。这样的方法两个指针在最坏的情况下合起来只遍历了数组一次。与上一解法不同的是，**这个解法避免了需要保留的元素的重复赋值操作**。

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0, right = nums.size();
        while (left < right) {
            if (nums[left] == val) {
                nums[left] = nums[right - 1];
                right--;
            } else {
                left++;
            }
        }
        return left;
    }
};
```

时间复杂度：O(n)，最多遍历数组一遍

空间复杂度：O(1)



## 26. 删除有序数组中的重复项

### 题目

题目链接：[26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

给你一个 **升序排列** 的数组 nums ，请你 **原地** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。

由于在某些语言中不能改变数组的长度，所以必须将结果放在数组nums的第一部分。更规范地说，如果在删除重复项之后有 k 个元素，那么 nums 的前 k 个元素应该保存最终结果。

将最终结果插入 nums 的前 k 个位置后返回 k 。

不要使用额外的空间，你必须在 **原地** **修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

**判题标准:**

系统会用下面的代码来测试你的题解:

> int[] nums = [...]; // 输入数组<br />
> int[] expectedNums = [...]; // 长度正确的期望答案
>
> int k = removeDuplicates(nums); // 调用
>
> assert k == expectedNums.length;<br />
> for (int i = 0; i < k; i++) {<br />
>     assert nums[i] == expectedNums[i];<br />
> }

如果所有断言都通过，那么您的题解将被 **通过**。

示例 1：
> 输入：nums = [1,1,2]<br />
> 输出：2, nums = [1,2,_]<br />
> 解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。

示例 2：
> 输入：nums = [0,0,1,1,1,2,2,3,3,4]<br />
> 输出：5, nums = [0,1,2,3,4]<br />
> 解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。

提示：<br />
0 <= nums.length <= 3 * 10^4<br />
-10^4 <= nums[i] <= 10^4<br />
nums 已按 **升序** 排列

### 解法

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) {
            return 0;
        }
        int fast = 1, slow = 1;
        while (fast < n) {
            if (nums[fast] != nums[fast - 1]) {
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
};
```

因为数组是升序排列的，所以nums[fast]可以放心大胆地跟nums[fast - 1]比较。

时间复杂度：O(n)

空间复杂度：O(1)

## 167. 两数之和 II - 输入有序数组

### 题目

题目链接：[167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

给你一个下标从 **1** 开始的整数数组 numbers ，该数组已按 **非递减顺序排列**  ，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果设这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。

以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

示例 1：
> 输入：numbers = [2,7,11,15], target = 9<br />
> 输出：[1,2]<br />
> 解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

示例 2：
> 输入：numbers = [2,3,4], target = 6<br />
> 输出：[1,3]<br />
> 解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。

示例 3：
> 输入：numbers = [-1,0], target = -1<br />
> 输出：[1,2]<br />
> 解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

提示：<br />
2 <= numbers.length <= 3 * 10^4<br />
-1000 <= numbers[i] <= 1000<br />
numbers 按 **非递减顺序** 排列<br />
-1000 <= target <= 1000<br />
**仅存在一个有效答案**

### 解法

```c++
class Solution { 
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int low = 0, high = numbers.size() - 1;
        while (low < high) {
            int sum = numbers[low] + numbers[high];
            if (sum == target) {
                return {low + 1, high + 1};
            } else if (sum < target) {
                ++low;
            } else {
                --high;
            }
        }
        return {-1, -1};
    }
};
```

双指针法不会把可能的解过滤掉。但是题目确保仅存在一个有效答案，因此使用双指针一定可以找到答案。

时间复杂度：O(n)

空间复杂度：O(1)

### 其他解法

#### 双指针

略。

时间复杂度：O(nlog n)

空间复杂度：O(1)



## 283. 移动零

### 题目

题目链接：[283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

示例 1:
> 输入: nums = [0,1,0,3,12]<br />
> 输出: [1,3,12,0,0]

示例 2:
> 输入: nums = [0]<br />
> 输出: [0]

提示:<br />

1 <= nums.length <= 10^4<br />
-2^31 <= nums[i] <= 2^31 - 1

**进阶：**你能尽量减少完成的操作次数吗？

### 解法

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();
        int l = 0, r = 0;
        while(r < n) //注意while这层双指针算法，直接把左边的零覆盖为右边的值
        {
            if(nums[r] != 0)
            {
                nums[l] = nums[r];
                l++;
            }
            r++;
        }
        for(int i = l; i < n; i++) //把剩下的位补0
        {
            nums[i] = 0;
        }
    }
};
```

与[27.移除元素](https://leetcode-cn.com/problems/remove-element/)几乎完全相同。

时间复杂度：O(n)

空间复杂度：O(1)



## [557. 反转字符串中的单词 III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

### 题目

题目链接：[557. 反转字符串中的单词 III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

给定一个字符串 s ，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

示例 1：
> 输入：s = "Let's take LeetCode contest"<br />
> 输出："s'teL ekat edoCteeL tsetnoc"

示例 2:
> 输入： s = "God Ding"<br />
> 输出："doG gniD"

提示：<br />
1 <= s.length <= 5 * 104<br />
s 包含可打印的 **ASCII** 字符。<br />
s 不包含任何开头或结尾空格。<br />
s 里 **至少** 有一个词。<br />
s 中的所有单词都用一个空格隔开。

### 解法

```c++
class Solution {
public:
    string reverseWords(string s) {
        int length = s.length();
        int i = 0;
        while (i < length) {
            int start = i;
            while (i < length && s[i] != ' ') {
                i++;
            }

            int left = start, right = i - 1;
            while (left < right) {
                swap(s[left], s[right]);
                left++;
                right--;
            }
            while (i < length && s[i] == ' ') {
                i++;
            }
        }
        return s;
    }
};
```

时间复杂度：O(N)

空间复杂度：O(1)



## 844. 比较含退格的字符串

### 题目

题目链接：[844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)

给定 s 和 t 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 true 。# 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

示例 1：
> 输入：s = "ab#c", t = "ad#c"<br />
> 输出：true<br />
> 解释：s 和 t 都会变成 "ac"。

示例 2：
> 输入：s = "ab##", t = "c#d#"<br />
> 输出：true<br />
> 解释：s 和 t 都会变成 ""。

示例 3：
> 输入：s = "a#c", t = "b"<br />
> 输出：false<br />
> 解释：s 会变成 "c"，但 t 仍然是 "b"。

提示：<br />
1 <= s.length, t.length <= 200<br />
s 和 t 只含有小写字母以及字符 '#'

进阶：
你可以用 O(n) 的时间复杂度和 O(1) 的空间复杂度解决该问题吗？

### 解法

一个字符是否会被删掉，只取决于该字符后面的退格符，而与该字符前面的退格符无关。因此当我们逆序地遍历字符串，就可以立即确定当前字符是否会被删掉。

具体地，我们定义 skip 表示当前待删除的字符的数量。每次我们遍历到一个字符：

+ 若该字符为退格符，则我们需要多删除一个普通字符，我们让 skip 加 1；
+ 若该字符为普通字符：
  + 若 skip 为 0，则说明当前字符不需要删去；
  + 若 skip 不为 0，则说明当前字符需要删去，我们让 skip 减 1。

这样，我们定义两个指针，分别指向两字符串的末尾。每次我们让两指针逆序地遍历两字符串，直到两字符串能够各自确定一个字符，然后将这两个字符进行比较。重复这一过程直到找到的两个字符不相等，或遍历完字符串为止。

```c++
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        // 初始化双指针，分别指向两个字符串的末尾，朝头部开始遍历
        int i = S.length() - 1, j = T.length() - 1;
        // 定义skipS/skipT为S/T应该跳过的数目
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                // 遇到一个退格，需要跳过的字符+1
                if (S[i] == '#') {
                    skipS++, i--;
                }
                // 如果skipS/T > 0，说明需要跳过当前字符
                else if (skipS > 0) {
                    skipS--, i--;
                } else {
                    break;
                }
            }
            while (j >= 0) {
                if (T[j] == '#') {
                    skipT++, j--;
                } else if (skipT > 0) {
                    skipT--, j--;
                } else {
                    break;
                }
            }
            // 如果两个字符串都没有遍历完
            if (i >= 0 && j >= 0) {
                // 当前字符不相同
                if (S[i] != T[j]) {
                    return false;
                }
            } else {
                // 其中一个字符串遍历完，另一个没有遍历完，即长度不同
                if (i >= 0 || j >= 0) {
                    return false;
                }
            }
            i--, j--;
        }
        return true;
    }
};
```

时间复杂度：O(M + N)，M N分别为两个字符串的长度。

空间复杂度：O(1)

### 其它解法

#### 重构字符串

我们用栈处理遍历过程，每次我们遍历到一个字符：

+ 如果它是退格符，那么我们将栈顶弹出；
+ 如果它是普通字符，那么我们将其压入栈中。

```c++
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        return build(S) == build(T);
    }

    string build(string str) {
        string ret;
        for (char ch : str) {
            if (ch != '#') {
                ret.push_back(ch);
            } else if (!ret.empty()) {
                ret.pop_back();
            }
        }
        return ret;
    }
};
```

时间复杂度：O(M + N)，M, N分别为两个字符串的长度。

空间复杂度：O(M + N)



## 977. 有序数组的平方

### 题目

题目链接：[977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

示例 1：
> 输入：nums = [-4,-1,0,3,10]<br />
> 输出：[0,1,9,16,100]<br />
> 解释：平方后，数组变为 [16,1,0,9,100]<br />
> 排序后，数组变为 [0,1,9,16,100]

示例 2：
> 输入：nums = [-7,-3,2,3,11]<br />
> 输出：[4,9,9,49,121]

提示：<br />
1 <= nums.length <= 10^4<br />
-10^4 <= nums[i] <= 10^4<br />
nums 已按 非递减顺序 排序

进阶：
请你设计时间复杂度为 O(n) 的算法解决本问题

### 解法

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int l = nums.size() - 1;
        vector<int> nums2(nums.size());
        for(int i = 0, j = l; i <= j;) 
        //由于两边的绝对值往中间递减，
        //所以用双指针从两边开始
        //i=j也要考虑，此时两个指针相遇，归到else的情况
        {
            if(nums[i] * nums[i] < nums[j] * nums[j])
            {
                nums2[l] = nums[j] * nums[j];
                l--;
                j--;
            }
            else
            {
                nums2[l] = nums[i] * nums[i];
                l--;
                i++;
            }
        }
        return nums2;
    }
};
```

用到双指针i, j。i指向数组左端，j指向数组右端，分别向中间靠近。

时间复杂度：O(n)

空间复杂度：O(1)

## 总结

据博主不完全总结，可用双指针解决的数组题目可以分为以下三种：

1. 两个指向开头、相同步伐（相同速率）、一前一后的指针；
2. 两个指向末尾、相同步伐（相同速率）、一前一后的指针；
3. 一个指向开头、一个指向末尾、相同步伐（相同速率）的指针。

注：在链表相关的题目中用到双指针，还会用到其他类型的双指针。比如两个指向开头、不同步伐（不同速率）、一前一后的指针。比如slow->next; fast->next->next。这里不展开讨论。

<div style="color: red; font-size:24px">商业转载请联系博主获得授权，非商业转载请注明出处！</div>

分享结束，大家辛苦了。散会！


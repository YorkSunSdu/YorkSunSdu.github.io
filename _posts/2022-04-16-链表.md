---
layout: post
title: 链表
date: 2022-04-16
tags: 算法
---

博主对leetcode中的有关**链表**的题目作了总结。

题目来源[leetcode](https://leetcode-cn.com/)，点击题目链接即可直达leetcode对应题目。

## 203. 移除链表元素

### 题目

题目链接：[203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 **新的头节点** 。

示例 1：
> 输入：head = [1,2,6,3,4,5,6], val = 6<br />
> 输出：[1,2,3,4,5]

示例 2：
> 输入：head = [], val = 1<br />
> 输出：[]

示例 3：
> 输入：head = [7,7,7,7], val = 7<br />
> 输出：[]

提示：<br />
列表中的节点数目在范围 [0, 104] 内<br />
1 <= Node.val <= 50<br />
0 <= val <= 50



### 解法一

迭代：

```c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0); // 设置一个虚拟头结点
        dummyHead->next = head; // 将虚拟头结点指向head，这样方面后面做删除操作
        // 以上两行也可以换成 ListNode* dummyHead = new ListNode(0, head);
        ListNode* cur = dummyHead;
        while (cur->next != NULL) {
            if(cur->next->val == val) {
                cur->next = cur->next->next;
            } else {
                cur = cur->next;
            }
        }
        return dummyHead->next;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)



### 解法二

递归：

```c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (head == nullptr) {
            return head;
        }
        head->next = removeElements(head->next, val);
        return head->val == val ? head->next : head;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)



## 707. 设计链表

### 题目

题目链接：[707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)

设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

+ get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
+ addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
+ addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
+ addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
+ deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。


示例：

> MyLinkedList linkedList = new MyLinkedList();<br />
> linkedList.addAtHead(1);<br />
> linkedList.addAtTail(3);<br />
> linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3<br />
> linkedList.get(1);            //返回2<br />
> linkedList.deleteAtIndex(1);  //现在链表是1-> 3<br />
> linkedList.get(1);            //返回3<br />


提示：<br />

所有val值都在 [1, 1000] 之内。<br />
操作次数将在  [1, 1000] 之内。<br />
请不要使用内置的 LinkedList 库。



### 解法

```c++
class MyLinkedList {
public:
    // 定义链表节点结构体
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
    };
    // 别忘了上面这个分号

    // 初始化链表
    MyLinkedList() {
        _dummyHead = new LinkedNode(0); // 这里定义的头结点 是一个虚拟头结点，而不是真正的链表头结点
        _size = 0;
    }

    // 获取到第index个节点数值，如果index是非法数值直接返回-1， 注意index是从0开始的，第0个节点就是头结点
    int get(int index) {
        if (index >= _size || index < 0) {
            return -1;
        }
        LinkedNode* cur = _dummyHead->next;
        while(index--){ // 如果--index 就会陷入死循环
            cur = cur->next;
        }
        return cur->val;
    }

    // 在链表最前面插入一个节点，插入完成后，新插入的节点为链表的新的头结点
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode(val); // 注意新节点的创建方式
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        _size++;
    }

    // 在链表最后面添加一个节点
    void addAtTail(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        // 注意下面这行不是LinkedNode* cur = _dummyHead->next 因为链表有可能为空
        LinkedNode* cur = _dummyHead;
        while(cur->next != nullptr){
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }

    // 在第index个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果index大于链表的长度，则返回空
    void addAtIndex(int index, int val) {
        // 注意这个方法是在index对应的ji'ei'd'n之前插入节点
        if (index > _size) {
            return;
        }
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }

    // 删除第index个节点，如果index 大于等于链表的长度，直接return，注意index是从0开始的
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) {
            return;
        }
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur ->next;
        }
        cur->next = cur->next->next;
        _size--;
    }

    // 打印链表
    void printLinkedList() {
        LinkedNode* cur = _dummyHead;
        while (cur->next != nullptr) {
            cout << cur->next->val << " ";
            cur = cur->next;
        }
        cout << endl;
    }
private:
    int _size;
    LinkedNode* _dummyHead;

};
```

时间复杂度：O(1) / O(index) / O(n)

空间复杂度：O(1)



## 206. 反转链表

### 题目

题目链接：[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。


示例 1：
> 输入：head = [1,2,3,4,5]<br />
> 输出：[5,4,3,2,1]

示例 2：
> 输入：head = [1,2]<br />
> 输出：[2,1]

示例 3：
> 输入：head = []<br />
> 输出：[]

提示：<br />
链表中节点的数目范围是 [0, 5000]<br />
-5000 <= Node.val <= 5000

进阶：链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？



### 解法

```c++
class Solution {
    // 以下为迭代方法，用到双指针
public:
    ListNode* reverseList123(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while(curr)
        {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    // 以下为递归方法

    ListNode* reverseList(ListNode* head)
    {
        // 下面的if条件不仅处理了空链表、单节点链表，
        // 当递归到链表末尾的时候，还把尾节点返回给了newHead
        if(head == nullptr || head->next == nullptr)
            return head;
        ListNode* newHead = reverseList(head->next); // newhead指向最后一个节点
        // 除了尾节点，所有节点都执行下面两行
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1) / O(n)



## 24. 两两交换链表中的节点

### 题目

题目链接：[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

示例 1：
> 输入：head = [1,2,3,4]<br />
> 输出：[2,1,4,3]

示例 2：
> 输入：head = []<br />
> 输出：[]

示例 3：
> 输入：head = [1]<br />
> 输出：[1]

提示：<br />
链表中节点的数目在范围 [0, 100] 内<br />
0 <= Node.val <= 100



### 解法一

迭代。

![](/images/LinkedListimgs/swapEveryTwoNodes.png)

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0); // 设置一个虚拟头结点
        dummyHead->next = head; // 将虚拟头结点指向head，这样方面后面做删除操作
        ListNode* cur = dummyHead;
        while(cur->next != nullptr && cur->next->next != nullptr) {
            ListNode* tmp = cur->next; // 记录临时节点
            ListNode* tmp1 = cur->next->next->next; // 记录临时节点

            cur->next = cur->next->next;    // 步骤一
            cur->next->next = tmp;          // 步骤二
            cur->next->next->next = tmp1;   // 步骤三

            cur = cur->next->next; // cur移动两位，准备下一轮交换
        }
        return dummyHead->next; // 不要return head！因为head已经被交换成第二个节点了
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

注：解法和图片来自代码随想录。

### 解法二

递归。

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode* newHead = head->next;
        head->next = swapPairs(newHead->next);
        newHead->next = head;
        return newHead;
    }
};
```

注：来自Leetcode官方题解。没看懂。递归可太难读了。

时间复杂度：O(n)

空间复杂度：O(n)



## 19. 删除链表的倒数第 N 个结点

### 题目

题目链接：[19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

示例 1：
> 输入：head = [1,2,3,4,5], n = 2<br />
> 输出：[1,2,3,5]

示例 2：
> 输入：head = [1], n = 1<br />
> 输出：[]

示例 3：
> 输入：head = [1,2], n = 1<br />
> 输出：[1]

提示：<br />
链表中结点的数目为 sz<br />
1 <= sz <= 30<br />
0 <= Node.val <= 100<br />
1 <= n <= sz


进阶：你能尝试使用一趟扫描实现吗？



### 解法一

两次遍历，第一次遍历求链表长度，第二次遍历删除索引节点。

```c++
class Solution {
    // 本题这个方法是依次遍历，然后对删除头节点、尾节点做特殊处理
    // 也可以用栈，先全部入栈，再弹出。弹出的第n个节点即为要删除的节点
    // 也可以用前后双指针，先对前指针遍历n次，然后前后指针以同样速度遍历，
    // 当后指针遍历结束前指针指向的即为要删除的节点
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* node = head;
        if(node->next == nullptr)
        {
            head = nullptr;
            return head;
        }
        int count = 0;
        while(node->next->next != nullptr) // node指向倒数第二个节点，便于删除尾节点
        {
            count++;
            node = node->next;
        }
        count += 2;
        if(count == n) // 删除头节点
        {
            head = head->next;
            return head;
        }
        if(n == 1) // 删除尾节点
        {
            node->next = nullptr;
            return head;
        }
        node = head;
        for(int i = 0; i < count - n - 1; i++)
        {
            node = node->next;
        }
        node->next = node->next->next;
        return head;

    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)



### 解法二

用栈解题。先把节点全部入栈，再弹出。弹出的第n个节点即为要删除的节点。

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        stack<ListNode*> stk; // 注意栈的定义
        ListNode* cur = dummy;
        while (cur) {
            stk.push(cur);
            cur = cur->next;
        }
        for (int i = 0; i < n; ++i) {
            stk.pop();
        }
        // 遍历之后，栈顶节点为要删除的节点之前的节点
        ListNode* prev = stk.top();
        prev->next = prev->next->next;
        ListNode* ans = dummy->next;
        delete dummy;
        return ans;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)



### 解法三

用左右双指针解题。先对右指针右移n次，然后左指针和右指针以同样速度右移，当右指针指向最后一个节点时，左指针指向的下一个节点即为要删除的节点。

```c++
ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0, head);
        ListNode* right = dummyHead;
        for(int i = 0; i < n; i++)
            right = right->next;
        ListNode* left = dummyHead;
        while(right->next != nullptr)
        {
            left = left->next;
            right = right->next;
        }
        left->next = left->next->next;
        ListNode* ans = dummyHead->next;
        delete dummyHead;
        return ans;
    }
```

时间复杂度：O(n)

空间复杂度：O(1)



## 面试题 02.07. 链表相交

### 题目

题目链接：[面试题 02.07. 链表相交](https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/)

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

图示两个链表在节点 c1 开始相交：

<img src="/images/LinkedListimgs/02.07.1.jpg" style="zoom: 50%;" />

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

示例 1：

<img src="/images/LinkedListimgs/02.07.2.jpg" style="zoom:50%;" />

> 输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3<br />
> 输出：Intersected at '8'<br />
> 解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。<br />
> 从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。<br />
> 在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。

示例 2：

<img src="/images/LinkedListimgs/02.07.3.jpg" style="zoom:50%;" />

> 输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1<br />
> 输出：Intersected at '2'<br />
> 解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。<br />
> 从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。<br />
> 在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。

示例 3：

<img src="/images/LinkedListimgs/02.07.4.jpg" style="zoom:50%;" />


> 输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2<br />
> 输出：null<br />
> 解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。<br />
> 由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。<br />
> 这两个链表不相交，因此返回 null 。


提示：

listA 中节点数目为 m
listB 中节点数目为 n
0 <= m, n <= 3 * 10^4
1 <= Node.val <= 10^5
0 <= skipA <= m
0 <= skipB <= n
如果 listA 和 listB 没有交点，intersectVal 为 0
如果 listA 和 listB 有交点，intersectVal == listA[skipA + 1] == listB[skipB + 1]

进阶：你能否设计一个时间复杂度 O(n) 、仅用 O(1) 内存的解决方案？



### 解法一

哈希集合：首先遍历链表headA，并将链表headA 中的每个节点加入哈希集合中。然后遍历链表headB，对于遍历到的每个节点，判断该节点是否在哈希集合中。

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        // 注意判断的是节点相同，而不是值相同
        unordered_set<ListNode *> visited; // 用unordered_set储存headA中的节点
        ListNode *temp = headA;
        while (temp != nullptr) {
            visited.insert(temp);
            temp = temp->next;
        }
        temp = headB;
        while (temp != nullptr) {
            if (visited.count(temp)) {
                return temp;
            }
            temp = temp->next;
        }
        return nullptr;
    }
};
```

时间复杂度：O(m + n)

空间复杂度：O(m)



### 解法二

双指针，另两个指针分别指向两个链表的头节点。求两个链表长度的差值k，然后把长链表的指针右移k次，使两个链表剩余长度相等。然后依次判断每个节点是否相同。

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;
        int lenA = 0, lenB = 0;
        while (curA != NULL) { // 求链表A的长度
            lenA++;
            curA = curA->next;
        }
        while (curB != NULL) { // 求链表B的长度
            lenB++;
            curB = curB->next;
        }
        curA = headA;
        curB = headB;
        // 让curA为最长链表的头，lenA为其长度
        if (lenB > lenA) {
            swap (lenA, lenB);
            swap (curA, curB);
        }
        // 求长度差
        int gap = lenA - lenB;
        // 让curA和curB在同一起点上（末尾位置对齐）
        while (gap--) {
            curA = curA->next;
        }
        // 遍历curA 和 curB，遇到相同则直接返回
        while (curA != NULL) {
            if (curA == curB) {
                return curA;
            }
            curA = curA->next;
            curB = curB->next;
        }
        return NULL;
    }
};
```

时间复杂度：O(m + n)

空间复杂度：O(1)

此方法来自[代码随想录](https://programmercarl.com/)。



### 解法三

<img src="/images/LinkedListimgs/02.07.5.jpg" style="zoom:90%;" />

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == nullptr || headB == nullptr) {
            return nullptr;
        }
        ListNode *pA = headA, *pB = headB;
        while (pA != pB) {
            pA = pA == nullptr ? headB : pA->next;
            pB = pB == nullptr ? headA : pB->next;
        }
        // pA == pB
        return pA;
    }
};
```

时间复杂度：O(m + n)

空间复杂度：O(1)

此方法来自[leetcode官方](https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/solution/lian-biao-xiang-jiao-by-leetcode-solutio-2kne/)。

## 142. 环形链表 II

### 题目

题目链接：[142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 pos 是 -1，则在该链表中没有环。**注意：pos 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。

示例 1：

<img src="/images/LinkedListimgs/142.1.jpg" style="zoom:50%;" />

> 输入：head = [3,2,0,-4], pos = 1<br />
> 输出：返回索引为 1 的链表节点<br />
> 解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：

<img src="/images/LinkedListimgs/142.2.jpg" style="zoom:50%;" />

> 输入：head = [1,2], pos = 0<br />
> 输出：返回索引为 0 的链表节点<br />
> 解释：链表中有一个环，其尾部连接到第一个节点。

示例 3：

<img src="/images/LinkedListimgs/142.3.jpg" style="zoom:50%;" />

> 输入：head = [1], pos = -1<br />
> 输出：返回 null<br />
> 解释：链表中没有环。



提示：<br />
链表中节点的数目范围在范围 [0, 104] 内<br />
-10^5 <= Node.val <= 10^5<br />
pos 的值为 -1 或者链表中的一个有效索引<br />

进阶：你是否可以使用 O(1) 空间解决此题？



### 解法一

哈希表。

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode *> visited;
        while (head != nullptr) {
            if (visited.count(head)) {
                return head;
            }
            visited.insert(head);
            head = head->next;
        }
        return nullptr;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(n)



### 解法二

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *slow = head, *fast = head;
        while (fast != nullptr) {
            slow = slow->next;
            if (fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;
            if (fast == slow) {
                ListNode *ptr = head;
                while (ptr != slow) {
                    ptr = ptr->next;
                    slow = slow->next;
                }
                return ptr;
            }
        }
        return nullptr;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)

该解法来自[leetcode](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/huan-xing-lian-biao-ii-by-leetcode-solution/)。



## 总结

+ 要熟练掌握链表的类定义。
+ 做链表题要画图解决。
+ 注意dummyHead虚拟头节点的应用。

<div style="color: red; font-size:24px">商业转载请联系博主获得授权，非商业转载请注明出处！</div>

分享结束，大家辛苦了。散会！


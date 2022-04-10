---
layout: post
title: 链表
date: 2022-04-09
tags: 算法
---

博主对leetcode中的有关**链表**的题目作了总结。

题目来源[leetcode](https://leetcode-cn.com/)，点击题目即可直达leetcode对应题目。

## [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

### 题目

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



## [707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)

### 题目

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



## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

### 题目

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



## [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

### 题目

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



### 总结

+ 要熟练掌握链表的类定义。
+ 做链表题要画图解决。
+ 注意dummyHead虚拟头节点的应用。

<div style="color: red; font-size:24px">商业转载请联系博主获得授权，非商业转载请注明出处！</div>

分享结束，大家辛苦了。散会！


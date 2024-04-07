---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第四天 				# 标题 
subtitle:   链表[LeetCode24两两交换链表中的结点、LeetCode19删除链表的倒数第N个节点、LeetCode160链表相交、LeetCode142环形链表II] #副标题
date:       2024-04-06 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签

---



[TOC]

# 第一题

[LeetCode24两两交换链表中的结点](https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)

## 解法一[双指针]

* 时间复杂度O(n)
* 空间复杂度O(1)
* **学到的点: 在更新完pre以及cur指针时,还需要把链表连起来,如果不加pre->next = cur保证pre在cur的前面,否则会出现环导致错误.**

~~~c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head==nullptr || head->next==nullptr){
            return head;
        }
        ListNode* dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode* pre = dummyHead;
        ListNode* cur = head;
        while (cur && cur->next){
            ListNode* temp = cur->next->next;
            pre->next = cur->next;
            cur->next->next = cur;
            pre = cur;
            cur = temp;
            pre->next = cur; // 需要把链表连起来
        }
        return dummyHead->next;
    }
};
~~~

## 解法二[单指针]

* 时间复杂度O(n)

* 空间复杂度O(1)

* 本解法为代码随想录解法,本质与双指针一样,只不过要保证下一个结点以及下下个结点不同,同时每次移动两位.

~~~c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        
    }
};
~~~

## 总结

* 对于本题画好图应该可以解出来,我主要失败的原因在于使用双指针形成了环,也就是缺少了单指针里面的步骤三.

# 第二题

[LeetCode19删除链表的倒数第N个节点](https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html)

## 解法一[暴力解法]

* 时间复杂度O(2* n)
* 空间复杂度O(1)
* 状态: 本题暴力解法比较简单,十分钟可以搞定.

~~~c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        // 暴力解法
        // 先遍历得到所有的结点数目
        ListNode* cur = head;
        int node_number = 0;
        while (cur) {
            node_number++;
            cur = cur->next;
        }
        // 创建哑结点
        ListNode* dummyHead = new ListNode();
        dummyHead->next = head;
        cur = dummyHead;
        int i = node_number - n;
        while (i--){
            cur = cur->next;
        }
        ListNode* temp = cur->next;
        cur->next = cur->next->next;
        delete temp;
        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
~~~

## 解法二[遍历一次]

* 时间复杂度O(n)
* 空间复杂度O(1)

~~~c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        
    }
};
~~~

## 总结

* 遍历一次确实不好想到

# 第三题

[LeetCode160.相交链表](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html)

## 解法一[反转链表]

* 时间复杂度O(n)
* 空间复杂度O(1)

~~~c++
class Solution {
public:
    ListNode *reverseList(ListNode *head){
        ListNode* pre = nullptr;
        ListNode* cur = head;
        while(cur){
            ListNode* temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        // 先反转链表A,B,使用双指针法
        ListNode* tailA = reverseList(headA);
        printf(tailA->val);
        ListNode* tailB= reverseList(headB);
        ListNode* curA = tailA;
        ListNode* curB = tailB;
        ListNode* pre = new ListNode();
        while (curA && curB){
            if (curA == curB){
                pre = curA;
                curA = curA->next;
                curB = curB->next;
            }
            else{
                headA = reverseList(tailA);
                headB = reverseList(tailB);
                return pre;
            }
        }
        return nullptr;
    }
};
~~~

## 解法二[移动到相同的长度开始]

* 时间复杂度O(m+n)

* 空间复杂度O(1)

~~~c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        // 计算链表长度
        ListNode* curA = headA;
        ListNode* curB = headB;
        int lenA = 0;
        int lenB = 0;
        while(curA){
            curA = curA->next;
            lenA++;
        }
        while(curB){
            curB = curB->next;
            lenB++;
        }
        curA = headA;
        curB = headB;
        if (lenA > lenB){
            int i = lenA - lenB;
            while (i--){
                curA = curA->next;
            }
        }
        else{
            int i = lenB - lenA;
            while (i--){
                curB = curB->next;
            }
        }
        while (curA && curB){
            if (curA != curB){
                curA = curA->next;
                curB = curB->next;
            }
            else{
                return curA;
            }
        }
        return nullptr;
    }
};
~~~

## 总结

* 解法一反转链表目前还有点问题,还有待解决
* 解法二巧妙的利用了如果这有相交的结点,则从末尾开始计算长度会一样,其实与解法一翻转链表有点类似,一个是找不相等的结点,一个是找相等的结点.

# 第四题

[LeetCode142环形链表II](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html)

## 解法一

~~~c++

~~~


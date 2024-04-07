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
* **学到的点: 在更新完pre以及cur指针时,还需要把链表连起来,否则会出现环导致错误.**

~~~c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode* pre = dummyHead;
        ListNode* cur = head;
        while (cur && cur->next){
            ListNode* temp = cur->next->next;
            pre->next = cur->next;
            cur->next->next = cur;
            cur->next = temp;
            pre = cur;
            cur = temp;
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
        // 单指针
        ListNode* dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while (cur->next && cur->next->next){
            // 记录临时结点
            ListNode* temp = cur->next;
            ListNode* temp2 = cur->next->next->next;
            cur->next = cur->next->next;
            cur->next->next = temp;
            temp->next = temp2;
            cur = cur->next->next;
        }
        return dummyHead->next;
    }
};
~~~

## 解法三[奇偶链表]

* 时间复杂度O(n)

* 空间复杂度O(1)

~~~c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        // 生成奇偶链表
        ListNode* odd_dummyHead = new ListNode();
        ListNode* even_dummyHead = new ListNode();
        ListNode* odd_cur = odd_dummyHead;
        ListNode* even_cur = even_dummyHead;
        ListNode* cur = head;
        int i = 0;
        while (cur){
            i += 1;
            if (i % 2 == 1){
                odd_cur->next = cur;
                odd_cur = odd_cur->next;
            }
            else{
                even_cur->next = cur;
                even_cur = even_cur->next;
            }
            cur = cur->next;    
        }
        odd_cur->next = nullptr;
        even_cur->next = nullptr;
        // 依次合并奇偶链表,先偶数再奇数
        odd_cur = odd_dummyHead->next;
        even_cur = even_dummyHead->next;
        // debug
        // while (odd_cur){
        //     std::cout << odd_cur->val << std::endl;
        //     odd_cur = odd_cur->next;
        // }
        // while (even_cur){
        //     std::cout << even_cur->val << std::endl;
        //     even_cur = even_cur->next;
        // }
        ListNode *new_dummyHead = new ListNode();
        ListNode *new_cur = new_dummyHead;
        ListNode* even_tmp = new ListNode();
        ListNode* odd_tmp = new ListNode();
        while (odd_cur && even_cur){
            std::cout << even_cur->val << std::endl;
            std::cout << odd_cur->val << std::endl;
            new_cur->next = even_cur;
            even_tmp = even_cur->next;
            new_cur = even_cur;
            new_cur->next = odd_cur;
            odd_tmp = odd_cur->next;
            new_cur = new_cur->next;
            odd_cur = odd_tmp;
            even_cur = even_tmp;
        }
        if (odd_cur){
            new_cur->next = odd_cur;
        }
        return new_dummyHead->next;
    }
};
~~~

## 总结

* 对于本题画好图应该可以解出来,双指针失败的原因在于使用双指针形成了环,也就是缺少了单指针里面的步骤三.
* 应该保证每次循环开始条件是一样的,只是初始位置不一样.
* 奇偶链表合并是偶然间看到的别人提到了后自己实现的,确实可以,后续或许可以拓展为多个结点翻转,这时用多个链表来合并应该也是个不错的解法.

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

## 解法二[快慢指针]

* 时间复杂度O(n)
* 空间复杂度O(1)

~~~c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        // 双指针
        // 先让快指针先走n步,再慢指针与快指针同时走
        ListNode* dummyHead = new ListNode();
        dummyHead->next = head;
        ListNode* fast = dummyHead;
        ListNode* slow = dummyHead;
        int i = n;
        while (i--){
            fast = fast->next;
        }
        while (fast->next){
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* tmp = slow->next;
        slow->next = slow->next->next;
        delete tmp;
        return dummyHead->next;
    }
};
~~~

## 总结

* 双指针再次发挥作用,下次碰到这种定量的指针差可以用快慢指针

# 第三题

[LeetCode160.相交链表](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html)

## 失败解法一[反转链表]

* 时间复杂度O(n)
* 空间复杂度O(1)

~~~c++
这种解法反转后,新建两个反转链表又没法判断哪个是共同的结点.如果不新建又不允许修改原链表,故则方法失效.
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

* 解法一反转链表方法应该是不可行的.
* 解法二巧妙的利用了如果这有相交的结点,则从末尾开始计算长度会一样,其实与解法一翻转链表有点类似,一个是找不相等的结点,一个是找相等的结点.

# 第四题

[LeetCode142环形链表II](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html)

## 解法一

~~~c++

~~~

# 知识点


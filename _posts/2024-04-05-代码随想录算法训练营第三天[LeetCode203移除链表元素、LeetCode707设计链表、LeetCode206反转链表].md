---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第三天				# 标题 
subtitle:   链表[LeetCode203移除链表元素、LeetCode707设计链表、LeetCode206反转链表] #副标题
date:       2024-04-05 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---



[TOC]

# 第一题

[LeetCode203移除链表元素](https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html)

## 解法一[设置哑结点]

* 时间复杂度: 空间复杂度O(1)

* 空间复杂度: 时间复杂度O(n)

~~~c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while(cur->next){
            if (cur->next->val == val){
                ListNode* tmp = cur->next;
                cur->next = cur->next->next; 
                delete tmp; // 需要手动释放掉被删除的结点空间
            }
            else{
                cur = cur->next;
            } 
        }
        head = dummyHead->next;
        delete dummyHead;// 手动释放掉哑结点
        return head;
    }
};
~~~

## 解法二[不设置哑结点]

* 时间复杂度: 空间复杂度O(1)

* 空间复杂度: 时间复杂度O(n)

~~~c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while (head && head->val == val){
            ListNode* tmp = head;
            head = head->next;
            delete tmp;
        }
        // 删除非头结点
        ListNode* cur = head;
        while (cur && cur->next){
            if (cur->next->val == val){
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            }
            else{
                cur = cur->next;
            }
        }
        return head;
    }
};
~~~

## 总结

* 状态: 这道题还算容易,只写了一种设置哑结点的方法.
* 对于不设置哑结点的没有考虑到判断cur->next以及cur都不为NULL才可以进入循环.设置哑结点不用考虑是因为cur不会为NULL.
* 知识点: 需要手动释放删除的结点.
* 知识点: 统一设置哑结点往往会更容易处理头结点.

# 第二题

[LeetCode707设计链表](https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html)

## 解法一

~~~c++
class MyLinkedList {
public:
    // 定义链表节点结构体
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
    };

    MyLinkedList() {
        _size = 0;
        _dummyHead = new LinkedNode(0);
    }
    
    int get(int index) {
        if (index > _size - 1 || index < 0){
            return -1;
        }
        else{
            LinkedNode* cur = _dummyHead->next;
            while (index--){
                cur = cur->next;
            }
            return cur->val;
        }
    }
    
    void addAtHead(int val) {
        LinkedNode* new_node = new LinkedNode(val);
        new_node->next = _dummyHead->next;
        _dummyHead->next = new_node;
        _size++;
    }
    
    void addAtTail(int val) {
        // 先找到尾节点
        LinkedNode* new_node = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while (cur->next){
            cur = cur->next;
        }
        cur->next = new_node;
        new_node->next = nullptr;
        _size++;
    }
    
    void addAtIndex(int index, int val) {
        if (index > _size) return;
        if (index < 0) index = 0;
        if (index <= _size && index >= 0){
            LinkedNode* new_node = new LinkedNode(val);
            LinkedNode* cur = _dummyHead;
            while (index--){
                cur = cur->next;
            }
            new_node->next = cur->next;
            cur->next = new_node;
            _size++;
        }
    }
    
    void deleteAtIndex(int index) {
        if (index >= 0 && index <= _size - 1){
            LinkedNode* cur = _dummyHead;
            while (index--){
                cur = cur->next;
            }
            LinkedNode* tmp = cur->next;
            cur->next = cur->next->next;
            delete tmp;
            //delete命令指示释放了tmp指针原本所指的那部分内存，
            //被delete后的指针tmp的值（地址）并非就是NULL，而是随机值。也就是被delete后，
            //如果不再加上一句tmp=nullptr,tmp会成为乱指的野指针
            //如果之后的程序不小心使用了tmp，会指向难以预想的内存空间
            tmp=nullptr;
            _size--;
        }
    }
private:
    int _size;
    LinkedNode* _dummyHead;
};
~~~

## 总结

* 状态: 查看了定义实现的函数方法

* 碰到的问题: 在写addAtIndex方法时,判断index==_size时,个人首先写的是单独利用已有的函数方法添加,但没有考虑到在添加完后会带来二次插入的错误,如下注释为错误写法,未注释的可以通过.

~~~c++
      // void addAtIndex(int index, int val) {
      //     if (index > _size) return;
      //     if (index < 0) index = 0;
      //     if (index == _size){
      //         this->addAtTail(val); //在这一步会更新_size导致在下一个if判断后二次插入带来错误
      //     }
      //     if (index < _size && index >= 0){
      //         LinkedNode* new_node = new LinkedNode(val);
      //         LinkedNode* cur = _dummyHead;
      //         while (index--){
      //             cur = cur->next;
      //         }
      //         new_node->next = cur->next;
      //         cur->next = new_node;
      //         _size++;
      //     }
      // }
  // 如果要继续单独将index==_size写一个判断,可以如下写法
      void addAtIndex(int index, int val) {
          if (index > _size) return;
          if (index < 0) index = 0;
          if (index == _size){
               this->addAtTail(val);
               index += 1; // 主要让index加上1避免二次插入
          }
          if (index < _size && index >= 0){
              LinkedNode* new_node = new LinkedNode(val);
              LinkedNode* cur = _dummyHead;
              while (index--){
                  cur = cur->next;
              }
              new_node->next = cur->next;
              cur->next = new_node;
              _size++;
          }
      }
~~~

# 第三题

[LeetCode206反转链表](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html)

## 解法一[头插法]

* 时间复杂度: 空间复杂度O(n)

* 空间复杂度: 时间复杂度O(n)

~~~c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == nullptr || head->next == nullptr){
            return head;
        }
        ListNode* cur = head; // 用于遍历原始链表
        ListNode* dummyHead = new ListNode(); // 创建新链表哑节点
        while(cur){
            ListNode* new_node = new ListNode();
            new_node->val = cur->val;
            new_node->next = dummyHead->next;
            dummyHead->next = new_node;
            cur = cur->next;
        }
        return dummyHead->next;
    }
};
~~~

## 解法二[双指针法]

* 时间复杂度: 空间复杂度O(1)

* 空间复杂度: 时间复杂度O(n)

~~~c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) { 
        if (head == nullptr || head->next == nullptr){
            return head;
        }
        ListNode* dummyHead = new ListNode(); // 创建哑节点
        dummyHead->next = head;
        ListNode* pre = dummyHead;
        ListNode* cur = head;
        while (cur){
            ListNode* temp = new ListNode();
            temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        head->next = nullptr;
        return pre;
    }
};
~~~

## 解法三[递归法]

* 时间复杂度: 空间复杂度O(1)

* 空间复杂度: 时间复杂度O(n)

~~~c++
class Solution {
public:
    ListNode* reverse(ListNode* pre, ListNode* cur){
        if (cur==nullptr){
            return pre;
        }
        ListNode* temp = cur->next;
        cur->next = pre;
        return reverse(cur, temp);
    }
    ListNode* reverseList(ListNode* head) {
        // 递归法
        return reverse(nullptr, head);
    }
};
~~~

## 总结

* 最先想到的是双指针法,但是写第一遍没有很快速写出来,然后就先写完了头插法,到第二天感觉更清醒一点写出来双指针法.
* 递归法是没有想到的,递归的结束条件往往是重点,这里利用的思想与双指针一样.
* 感觉链表主要在画图,图画明白了,代码自然而然就能写出来,图画不明白就很难写出来.

# 知识点

## 链表定义

这里加入了构造函数.C++构造函数有多种写法,

~~~c++
struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
    };
~~~

## 手动释放内存

在删除结点后,手动释放内存,来避免一些麻烦.c++使用delete来删除,使用new来新建.


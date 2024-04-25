---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十天 				# 标题 
subtitle:   栈和队列[LeetCode232用栈实现队列、LeetCode225用队列实现栈] #副标题
date:       2024-04-12 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode232用栈实现队列](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html)

## 解法一[双栈实现队列]

* push和empty为O(1), pop和peek为O(n)

* 空间复杂度O(n)

~~~c++
class MyQueue {
public:
    stack<int> stIn;
    stack<int> stOut;
    MyQueue() {

    }
    
    void push(int x) {
        stIn.push(x);
    }
    
    int pop() {
        if (stOut.empty()){
            while (!stIn.empty()){
                stOut.push(stIn.top());
                stIn.pop();
            }
            
        }
        int result = stOut.top();
        stOut.pop();
        return result;
    }
    
    int peek() {
        int res = this->pop(); // 直接使用已有的pop函数
        stOut.push(res); // 因为pop函数弹出了元素res，所以再添加回去
        return res;
    }
    
    bool empty() {
        if (stIn.empty() && stOut.empty()){
            return true;
        }
        else{
            return false;
        }
    }
};
~~~

## 总结

本体较为简单,几分钟就可以写好.

# 第二题

[LeetCode225用队列实现栈](https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html)

## 解法一[双队列实现栈]

* push,empty时间复杂度O(1)
* push, empty空间复杂度O(1)
* pop,top时间复杂度O(n)
* pop,top空间复杂度O(n)

~~~c++
class MyStack {
public:
    queue<int> queue1;
    queue<int> queue2;
    MyStack() {}

    void push(int x) {
        if (queue1.empty() && !queue2.empty()) {
            queue2.push(x);
        } 
        if (!queue1.empty() && queue2.empty()){
            queue1.push(x);
        }
        if (queue1.empty() && queue2.empty()){
            queue1.push(x);
        }

    }

    int pop() {
        int tmp;
        if (!queue1.empty()) {
            while (queue1.size() > 1) {
                queue2.push(queue1.front());
                queue1.pop();
            }
            tmp = queue1.front();
            queue1.pop();
        } else {
            while (queue2.size() > 1) {
                queue1.push(queue2.front());
                queue2.pop();
            }
            tmp = queue2.front();
            queue2.pop();
        }
        return tmp;
    }

    int top() {

        int tmp = pop();
        if (!queue1.empty()) {
            queue1.push(tmp);
        } 
        if (!queue2.empty()){
            queue2.push(tmp);
        }
        if (queue2.empty() && queue1.empty()){
            queue1.push(tmp);
        }
        return tmp;
    }

    bool empty() {
        return queue1.empty() && queue2.empty();
    }
};
~~~

## 解法二[单个队列实现栈]

* push时间复杂度O(n)
* push空间复杂度O(n)
* pop, top, empty时间复杂度O(1)
* pop, top, empty空间复杂度O(1)

~~~c++
class MyStack {
public:
    queue<int> queue;
    MyStack() {}

    void push(int x) {
        if (queue.empty()){
            queue.push(x);
        }
        else{
            queue.push(x);
            int count = queue.size() - 1;
            while (count > 0){
                int tmp = queue.front();
                queue.pop();
                count--;
                queue.push(tmp);
            }
        }
    }

    int pop() {
        int tmp = queue.front();
        queue.pop();
        return tmp;
    }

    int top() {
        return queue.front();
    }

    bool empty() {
        return queue.empty();
    }
};
~~~

## 总结

整体看来这题算是比较简单,也较短时间能够写出来,主要考察对于栈和队列的方法是否熟悉.

# 总结

因为各种各样的原因停了一个礼拜多,今天重新开始,尽量早点赶上进度.今天的题比较简单.


---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十三天 				# 标题 
subtitle:   栈和队列[LeetCode239滑动窗口最大值、LeetCode347前k个高频元素] #副标题
date:       2024-04-15 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode239滑动窗口最大值](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html)

## 解法一[单调队列]

* 空间复杂度O(n)

* 空间复杂度O(k)

~~~c++
class Solution {
private:
    class MyQueue {
    public:
        deque<int> que;
        // 当队列不空且比较当前要弹出的数值是否等于队列出口元素的数值,因为较小的数会在push的时候pop
        void pop(int value){
            if (!que.empty() && que.front() == value){
                que.pop_front();
            }
        }

        void push(int value){
            // 主要用于保证pop后还是最大的数字在开头
            while (!que.empty() && que.back() < value){
                que.pop_back();
            }
            que.push_back(value);
        }

        int front(){
            return que.front();
        }

    };

public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> result;
        MyQueue que;
        for (int i = 0; i < k; i++){
            que.push(nums[i]);
        }
        result.push_back(que.front()); // result 记录前k的元素的最大值
        for (int i = k; i < nums.size(); i++){
            que.pop(nums[i - k]);
            que.push(nums[i]);
            result.push_back(que.front());
        }
        return result;
    }
};
~~~

## 总结

* 本题有点巧妙,第一次做不容易想到这种做法
* 在单调队列pop的时候比较单调队列的出口元素以及当前原本需要pop的元素,在相等的时候就需要pop,不相等的时候早已经pop完毕所以不需要操作.
* 在单调队列push的时候pop掉较小的数,主要为了在后续一直保持出口元素是最大的元素.

# 第二题

[LeetCode347前k个高频元素](https://programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html)

## 解法一[优先队列与小顶堆]

* 时间复杂度O(nlogk)
* 空间复杂度O(n)

~~~c++
class Solution {
private:
    class mycomparison{
    public:
        bool operator() (const pair<int, int>& lhs, const pair<int, int>& rhs) {
            return lhs.second > rhs.second;
        }
    };
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        // 统计元素出现频率
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); i++){
            map[nums[i]]++;
        }

        // 对频率排序
        // 定义一个小顶堆
        priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pri_que;
        for (unordered_map<int, int>::iterator it = map.begin(); it != map.end(); it++){
            pri_que.push(*it);
            if (pri_que.size() > k) { // 如果堆的大小大于了K，则队列弹出，保证堆的大小一直为k
                pri_que.pop();
            }
        }

        // 结果数组, 倒序输出数组
        vector<int> result(k);
        for (int i = k - 1; i >= 0; i--){
            result[i] = pri_que.top().first;
            pri_que.pop();
        }

        return result;
    }
};
~~~

## 总结

* 整体来看这题是要实现数字出现次数统计,以及排序
* 感觉考察更多的是对于优先队列,map等容器与容器适配器的使用

# 总结

* 今天两道都没有思路,第二道虽然有思路,但是对于优先队列不了解,也不熟悉它的用法,后续需要研究一下.


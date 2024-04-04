---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第一天				# 标题 
subtitle:   数组[LeetCode704二分查找、LeetCode27移除元素] #副标题
date:       2024-04-03 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构						#标签
---

[TOC]

# 第一题

[LeetCode704二分查找](https://leetcode.cn/problems/binary-search/)

## 解法一[左闭右开]

~~~c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int size = nums.size();
        int right = size;
        int left = 0;
        while (left < right){
            int mid = (right + left) / 2;
            if (nums[mid] == target){
                return mid;
            }
            else if (nums[mid] > target){
                right = mid;  
            }
            else{
                left = mid + 1;
            }
        }
        return -1;
    }
};
~~~

## 解法二[左闭右闭]

~~~cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int size = nums.size();
        int right = size - 1;
        int left = 0;
        while (left <= right){
            int mid = (right + left) / 2;
            if (nums[mid] == target){
                return mid;
            }
            else if (nums[mid] > target){
                right = mid - 1;  
            }
            else{
                left = mid + 1;
            }   
        }
        return -1;
    }
};
~~~

总用时：十五分钟

## 总结

前几天做过一次了，这是第二次刷这题，还算是比较顺利的写出来这两种方法，总体看来左闭右开与左闭右闭的区别在于是否能取到right下标的值。几个关键的区别的地方：

* 初始right在左闭右开取数组长度，在左闭右闭时取数组最大下标。

* 左闭右开在while时不需要等号，因为[a， b)，当b = a + 1数组就已经遍历完成。
* 左闭右开在判断nums[mid] < target时直接将right = mid即可，因为mid已经判断完毕，mid在这种方式是取不到的所以最新的右下标等于mid，而左闭右闭时right = mid - 1，此时这种方式可以取到，将right指向新的未判断的下标。

# 第二题

[LeetCode27移除元素](https://leetcode.cn/problems/remove-element/)

## 解法一[暴力解法]

~~~c+
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        // 暴力解法
        int n = nums.size();
        for (int i = 0; i < n; i++){
            if (nums[i] == val){
                for (int j = i + 1; j < n; j++){
                    nums[j - 1] = nums[j];
                }
                i--;
                n--;
             }
        }
        return n;
    }
};

~~~

## 解法二[双指针法]

~~~c+
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size();
        int right = 0;
        int left = 0;
        for (int right = 0; right < n; right++){
            // 找第一个不等于的值放到数组前
            if (nums[right] != val){
                nums[left++] = nums[right];
            }
        }
        return left;
    }
};

~~~

用时：30分钟

## 总结

目前暴力解法可以通过，暴力解法不太熟悉，主要卡在没有更新数组长度，导致超时，这是因为加入末尾有需要删除的值，将会拷贝很多份，如果不实时更新数组长度，会陷入死循环导致超时。

双指针法：好像还没那么难，这次直接写出来通过了，主要在于右指针找第一个不为val的值放入数组前面，同时更新左指针。

# 知识点

## 关于vector与array的区别



待完成：35.搜索插入位置 和 34. 在排序数组中查找元素


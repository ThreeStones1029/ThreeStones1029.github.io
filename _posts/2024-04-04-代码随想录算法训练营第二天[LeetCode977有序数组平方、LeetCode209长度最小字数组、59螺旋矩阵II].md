---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第二天				# 标题 
subtitle:   数组[LeetCode977有序数组平方、LeetCode209长度最小字数组、59螺旋矩阵II] #副标题
date:       2024-04-04 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---



[TOC]

# 第一题

[LeetCode977有序数组平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

## 解法一[双指针法]

~~~c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        // 双指针法
        int left = 0;
        int right = nums.size() - 1;
        int index = right;
        vector<int> new_nums(nums);
        while (left <= right){
             if (nums[left] * nums [left] > nums[right] * nums[right]){
                 new_nums[index--] = nums[left] * nums [left];
                 left++;
             }
             else {
                 new_nums[index--] = nums[right] * nums[right];
                 right--;
             }
         }
         return new_nums;
    }
};
~~~



## 解法二[暴力法]

~~~c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        //暴力法
        for (int i = 0; i < nums.size(); i++){
            nums[i] *= nums[i];
        }
        sort(nums.begin(), nums.end());
        return nums;
    }
};
~~~

## 总结

* 状态:这道题也做过,所以还是比较顺利地做出来了.
* 关键点:关键在于最大值只有可能出现在数组两端.
* sort为快排,时间复杂度为O(n*logn)

# 第二题

[LeetCode209长度最小字数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

## 解法一[暴力法]

~~~c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        //暴力解法 会超时
         int min_length = nums.size() + 1;
         for (int i = 0; i < nums.size(); i++){
             int sum = 0;
             for (int j = i; j < nums.size(); j++){
                 sum += nums[j];
                 while (sum >= target){
                 min_length = j - i + 1 < min_length ? (j - i + 1) : min_length;
                 break;
                 }
             }
         }
         return min_length == nums.size() + 1 ? 0 : min_length;
    }
};
~~~

## 解法二[滑动窗口]

~~~c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        // 滑动窗口解法
        int left= 0;
        int sum = 0;
        int min_length = __INT32_MAX__;
        for (int right = 0; right < nums.size(); right++){
            sum += nums[right];
            while(sum >= target){
                min_length = (right - left + 1) < min_length ? (right - left + 1) : min_length;
                sum -= nums[left++];
            }
        }
        return min_length == __INT32_MAX__? 0 : min_length;
    }
};
~~~

## 总结

* 状态: 只写出来暴力解法, 滑动窗口还是没写出来.
* 关键点: 滑动窗口在于巧妙利用结束位置来更新起始位置, 由于是叠加, 直接减去起始位置的值则可以从新的起始点计算.
* 算法思想: left指针表示子数组的起始位置, right指针表示子数组结束位置.当子数组的和值大于target, 则需要更新起始位置. 因为这时可以获得一个子数祖的长度, 然后while的意义就是在这个子数组里面找到比它更小的子数组长度. 跳出while后, 需要从上一个子数组的起始位置之后开始寻找.
* 算法关键处举例理解: 当前面的数都比较小,然后跟一个比较大的数字后,首先计算子数组和会开始都小于target,而碰到较大的数后就超过target了,但此时如果去掉最前面的比较小的数也可以超过target,例如在[1, 1, 1, 1, 6, 2]中找到target=7就需要一直从第一个加到第五个数才会进入while,然后才继续在这个子数组里面找更小的子数组,一直循环到单独一个数字6小于7才会跳出while,此时以left = 0, 1, 2, 3开头的子数组都已经遍历完了,继续从left=4开始的子数组遍历.
* 这题区间的定义为左闭右闭.
* 32位最大int值为__INT32_MAX__.

# 第三题

[LeetCode59螺旋矩阵II](https://leetcode.cn/problems/spiral-matrix-ii/)

## 解法一[左闭右闭]

~~~c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        // 左闭右闭
        vector<vector<int>> matrix(n, vector<int>(n, 0));
        int loop = 1; //loop表示圈数
        int x = 0; // x起始坐标
        int y = 0; // y起始坐标
        int k = 1;
        int length = n - 1; // 圈边的长度
        while (loop <= n / 2){
            // 从左到右
            for (int i = x, j = y; j <= y + length - 1; j++){
                matrix[i][j] = k++;
            }
            //从上到下
            for (int i = x, j = y + length; i <= x + length - 1; i++){
                matrix[i][j] = k++;
            }
            // 从右到左
            for (int i = x + length, j = y + length; j >= y + 1; j--){
                matrix[i][j] = k++;
            }
            // 从下到上
            for (int i = x + length , j = y; i >= x + 1; i--){
                matrix[i][j] = k++;
            }
            x++;
            y++;
            length -= 2;
            loop++;
        }
        if ((n % 2) != 0){
            matrix[n / 2][n / 2] = k;
        }
        return matrix;
    }
};
~~~

## 解法二[左闭右开]

~~~c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        // 左闭右开
        vector<vector<int>> matrix(n, vector<int>(n, 0));
        int x = 0;
        int y = 0;
        int loop = n / 2;
        int count = 1;
        int offset = 1;
        int i, j;
        while (loop--){
            i = x;
            j = y;
            for (j = y; j < n - offset; j++){
                matrix[i][j] = count++;
            }
            for (i = x; i < n - offset; i++){
                matrix[i][j] = count++;
            }
            for (; j > y; j--){
                matrix[i][j] = count++;
            }
            for (; i > x; i--){
                matrix[i][j] = count++;
            } 
            x++;
            y++;
            offset++;
        }
        if ( n % 2 != 0){
                matrix[n / 2][n / 2] = count;
            }
        return matrix;
    }
};
~~~

## 总结:

* 状态:较为艰难的写出来了左闭右闭.

* 关键点:主要在于需要想到转圈的方法,不断更新圈的起始位置,然后从左到右,从上到下,从右到左,从下到上遍历,需要处理好边界问题.
* 左闭右开和左闭右闭再次用到.



# 学到的基础知识

## Int 极限值

~~~c++
int max_value = __INT32_MAX__;
~~~

## 二维数组定义

~~~c++
vector<vector<int>> matrix(n); // 先指定一维数组大小
vector<vector<int>> matrix(n, vector<int>(n, 0)); //定义一个n * n的二维矩阵
~~~

## 迭代器

~~~c++
sort(nums.begin(), nums.end());
~~~

nums.begin()指向第一个元素, nums.end()指向最后一个元素的后一个元素.

## array与vector

[可查看](https://www.cnblogs.com/loganxu/p/15702011.html)
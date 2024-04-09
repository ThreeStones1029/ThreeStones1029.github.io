---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第七天 				# 标题 
subtitle:   哈希表[LeetCode454四数相加、LeetCode383赎金信  、LeetCode15三数之和 、LeetCode18四数之和] #副标题
date:       2024-04-09 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---



[TOC]

# 第一题

[LeetCode454四数相加](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html)

## 解法一

* 时间复杂度O(n*n)
* 空间复杂度O(n*n)

~~~c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        std::unordered_map <int,int> map;
        // 遍历nums1和nums2数组，统计两个数组元素之和，和出现的次数，放到map中
        for (int a : nums1){
            for (int b : nums2){
                map[a + b]++;
            }
        }
        int count = 0; // 统计a+b+c+d = 0 出现的次数
        // 在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就把map中key对应的value也就是出现次数统计出来。
        for (int c: nums3){
            for (int d: nums4){
                if (map.find(0 - (c + d)) != map.end()){
                    count += map[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
~~~

## 总结

* 总体来看感觉比较难想到, 建立a+b的次数的map比较巧妙,与上一题的两数之和有点像,但又感觉有很多不同,不过这个是统计次数,所以以次数作为键值.
* 0 - (c+d) = a+b;

# 第二题

[LeetCode383赎金信](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)

* 时间复杂度O(n)
* 空间复杂度O(1)

## 解法一

~~~c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int num1[26] = {0};
        int num2[26] = {0};
        for (int i = 0; i < ransomNote.size(); i++){
            num1[ransomNote[i] - 'a']++;
        }
        for (int i = 0; i < magazine.size(); i++){
            num2[magazine[i] - 'a']++;
        }
        int count1 = 0;
        int count2 = 0;
        for (int i = 0; i < 26; i++){
            if (num1[i] != 0){
                count1++;
            }
            if (num1[i] != 0 && num1[i] <= num2[i]){
                count2++;
            }
        }
        if (count1 == count2){
            return true;
        }
        else{
            return false;
        }
        return false;
    }
};
~~~

## 总结

* 这道题如果理解透彻数组法还是比较简单的,感觉只要统计字母次数的题都可以用数组法.

# 第三题

[LeetCode15三数之和](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)



# 第四题

[LeetCode18四数之和](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)

# 总结


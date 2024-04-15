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

## 解法一[暴力解法]

* 状态:不通过
* 原因: 时间复杂度过高

~~~c++
class Solution {
public:
    bool comparison_of_triples(vector<int> &first, vector<int> &second){
        if ((first[0] == second[0] && first[1] == second[1] && first[2] == second[2])){
            return true;
        }
        if ((first[0] == second[0] && first[1] == second[2] && first[2] == second[1])){
            return true;
        }
        if ((first[0] == second[1] && first[1] == second[0] && first[2] == second[2])){
            return true;
        }
        if ((first[0] == second[1] && first[1] == second[2] && first[2] == second[0])){
            return true;
        }
        if ((first[0] == second[2] && first[1] == second[0] && first[2] == second[1])){
            return true;
        }
        if ((first[0] == second[2] && first[1] == second[1] && first[2] == second[0])){
            return true;
        }
        return false;
    }
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        for (int i = 0; i < nums.size(); i++){
            for (int j = i + 1; j < nums.size(); j++){
                for (int k = j + 1; k < nums.size(); k++){
                    if (nums[i] + nums[j] + nums[k] == 0){
                        vector<int> sub_result = {nums[i], nums[j], nums[k]};
                        bool same = false;
                        for (int row = 0; row < result.size(); row++){
                            vector<int> row_result = {result[row][0], result[row][1], result[row][2]};
                            if (comparison_of_triples(sub_result, row_result)){
                                same = true;
                                break;
                            }
                        }
                        if (same == false){
                            result.push_back(sub_result);
                        }
                    }
                }
            }
        }
        return result;
    }
};
~~~

## 解法二[双指针法]

* 时间复杂度: O(n * n)
* 空间复杂度: O(1)

~~~c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        // 双指针法
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] > 0){
                return result;
            }
            if (i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while(left < right){
                if (nums[i] + nums[left] + nums[right] > 0){
                    right--;
                }
                else if (nums[i] + nums[left] + nums[right] < 0){
                    left++;
                }
                else{
                    result.push_back({nums[i], nums[left], nums[right]});
                    while (right > left && nums[left] == nums[left + 1]){
                    left++;
                    }
                    while (right > left && nums[right] == nums[right - 1]){
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
};
~~~

## 总结

* 这个双指针真的巧妙,对于去重也有很多细节,第一次见真想不到.
* 暴力法大概率可以运行大半,排除时间限制应该没有问题.
* 再找到一组解后,left++和right--是因为如果他们都是另一个不同的数,才能保证和可能为0,如果只改变一个,那三数之和一定不是0.

# 第四题

[LeetCode18四数之和](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)

## 解法一[双指针法]

* 时间复杂度: O(n * n)
* 空间复杂度: O(1)

~~~c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); k++) {
            // 剪枝处理
            if (nums[k] > target && nums[k] >= 0) {
            	break; // 这里使用break，统一通过最后的return返回
            }
            // 对nums[k]去重
            if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
            for (int i = k + 1; i < nums.size(); i++) {
                // 2级剪枝处理
                if (nums[k] + nums[i] > target && nums[k] + nums[i] >= 0) {
                    break;
                }

                // 对nums[i]去重
                if (i > k + 1 && nums[i] == nums[i - 1]) {
                    continue;
                }
                int left = i + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    // nums[k] + nums[i] + nums[left] + nums[right] > target 会溢出
                    if ((long) nums[k] + nums[i] + nums[left] + nums[right] > target) {
                        right--;
                    // nums[k] + nums[i] + nums[left] + nums[right] < target 会溢出
                    } else if ((long) nums[k] + nums[i] + nums[left] + nums[right]  < target) {
                        left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        // 对nums[left]和nums[right]去重
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        // 找到答案时，双指针同时收缩
                        right--;
                        left++;
                    }
                }

            }
        }
        return result;
    }
};
~~~

## 总结

* 虽然和上一题有点像,但需要注意的细节更多.
* (long)nums[right] + nums[left] + nums[i] + nums[k] 这里long用于防止溢出

# 总结

* 总体来说第三体和第四题还是挺难的,对于双指针确实没有想到这么用来解题.
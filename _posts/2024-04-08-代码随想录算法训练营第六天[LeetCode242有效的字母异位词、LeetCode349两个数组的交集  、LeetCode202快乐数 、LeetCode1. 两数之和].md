---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第六天 				# 标题 
subtitle:   哈希表[LeetCode242有效的字母异位词、LeetCode349两个数组的交集、LeetCode202快乐数、LeetCode1两数之和] #副标题
date:       2024-04-08 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---



[TOC]

# 第一题

[LeetCode242.有效的字母异位词 ](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html)

## 解法一[multi_set容器]

* 时间复杂度: O(logn*n)

* 空间复杂度: O(logn)

~~~c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        // 直接调用库版本
        if (s.size() != t.size()){
            return false;
        }
        multiset<int> s_multi_set;
        multiset<int> t_multi_set;
        for (int i = 0; i < s.size(); i++){
            s_multi_set.insert(s[i]);
            t_multi_set.insert(t[i]);
        }
        for (auto  m = s_multi_set.begin(), n = t_multi_set.begin(); m != s_multi_set.end() && n != t_multi_set.end(); m++, n++){
            if (*m != *n){
                return false;
            }
        }
        return true;
    }   
};
~~~

## 解法二[哈希数组]

* 时间复杂度: O(n)

* 空间复杂度: O(1)

~~~c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        // 数组法
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++){
            record[s[i] - 'a'] += 1;
        }
        for (int i = 0; i < t.size(); i++){
            record[t[i] - 'a'] -= 1;
        }
        for (int i = 0; i < 26; i++){
            if (record[i] != 0){
                return false;
            }
        }
        return true;
    }   
};
~~~

## 解法三[暴力解法调库]

* 时间复杂度: O(n * n)

* 空间复杂度: O(1)

~~~c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        // 暴力解法
        // 先排序
        sort(s.begin(),s.end());
        sort(t.begin(),t.end());
        return s==t;
    }    
};
~~~

## 解法四[暴力解法非调库]

* 时间复杂度: O(n * n)
* 空间复杂度: O(1)

~~~c++
class Solution {
public:
    string choose_sort(string s){
        int n = s.size();
        for (int i = 0; i < n - 1; ++i) {
            int min_index = i;
            for (int j = i + 1; j < n; ++j) {
                if (s[j] < s[min_index]) {
                    min_index = j;
                }
            }
            // 将未排序部分的最小元素与已排序部分的末尾交换位置
            int temp = s[i];
            s[i] = s[min_index];
            s[min_index] = temp;
        }
        return s;
    }
    bool isAnagram(string s, string t) {
        // 暴力解法2
        // 选择排序
        s = choose_sort(s);
        t = choose_sort(t);
        return s==t;
    }
        
};
~~~

## 总结

* 对于哈希的三种数据结构还不太熟悉,需要熟悉各个结构的时间空间复杂度.
* 整体来说这道题比较简单, 用哈希法确实可以很快解决.
* 自己实现的排序会超出时间.

# 第二题

[LeetCode349. 两个数组的交集 ](https://programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html)

## 解法一[暴力解法]

* 时间复杂度: O(n * m)

* 空间复杂度: O(n)

~~~c++
class Solution {
public:
    bool is_in_vector(vector<int>& nums, int val){
        for (int i = 0; i < nums.size(); i++){
            if (nums[i] == val){
                return true;
            }
        }
        return false;
    }
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> intersection_array;
        for (int i = 0; i < nums1.size(); i++){
            for (int j = 0; j < nums2.size(); j++){
                if (nums2[j] == nums1[i] && !is_in_vector(intersection_array, nums1[i])){
                    intersection_array.push_back(nums1[i]);
                    break;
                }
            }
        }
        return intersection_array;
    }
};
~~~

## 解法二[数组法]

* 时间复杂度: O(n)

* 空间复杂度: O(n)

~~~c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        // 数组法
        int result1[1001] = {0};
        int result2[1001] = {0};
        vector<int> result;
        for (int i = 0; i < nums1.size(); i++){
            result1[nums1[i]]++;
        }
        for (int i = 0; i < nums2.size(); i++){
            result2[nums2[i]]++;
        }
        for (int i = 0; i < 1001; i++){
            if (result1[i] != 0 && result2[i] != 0){
                result.push_back(i);
            }
        }
        return result;
    }
};
~~~

## 解法三[unordered_set]

* 时间复杂度: O(n + m)

* 空间复杂度: O(n)

~~~c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        // unordered_set
        unordered_set<int> result_set;
        unordered_set<int> nums1_set(nums1.begin(), nums1.end());
        for (int num : nums2){
            if (nums1_set.find(num) != nums1_set.end()){
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
~~~

## 总结

* 整体来说暴力解法最容易想到,也比较容易实现,直接遍历数组1,对每一个元素都判断是否在数组2里面有相同的数,然后再判断这个相同的数是否已经在共同的数组里面了.
* 数组法与第一题有点像,主要在于限制元素值的范围了,所以可以用数组法,整体来说知道思想写起来不难.
* 可以使用vector<int>(set.begin(), set.end())将set转为vector.相当于强制转化符号int.
* 使用unordered_set<int> set(vector1.begin(), vector1.end())将vector强制转化为unordered_set.

# 第三题

[LeetCode202. 快乐数](https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html)

## 解法一

* 时间复杂度: O(logn)

* 空间复杂度: O(logn)

~~~c++
class Solution {
public:
    int sum_of_each_degree(int n){
        int sum = 0;
        int end_number = 0; // 最后一位数字
        int other_number = n; // 除最后一位数字的其余位
        while (other_number){
            end_number = other_number % 10;
            other_number = other_number / 10;
            sum += end_number* end_number;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> result;
        int sum = n;
        while (result.find(sum) == result.end()){
            if (sum == 1){
                return true;
            }
            result.insert(sum);
            sum = sum_of_each_degree(sum); 
        }
        return false;
    }
};
~~~

## 总结

* 这道题了解unordered_set就比较简单,主要是利用result.find(sum) == result.end()判断是否出现过,时间复杂度也主要是查找unordered_set的元素.

# 第四题

[LeetCode1.两数之和](https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html)

## 解法一[unordered_map]

* 时间复杂度: O(n)

* 空间复杂度: O(n)

~~~c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map <int,int> map;
        for(int i = 0; i < nums.size(); i++) {
            // 遍历当前元素，并在map中寻找是否有匹配的key
            auto iter = map.find(target - nums[i]); 
            if(iter != map.end()) {
                return {iter->second, i};
            }
            // 如果没找到匹配对，就把访问过的元素和下标加入到map中
            map.insert(pair<int, int>(nums[i], i)); 
        }
        return {};
    }
};
~~~

## 解法二[暴力解法]

* 时间复杂度: O(n * n)

* 空间复杂度: O(n)

~~~c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        // 暴力解法
        vector<int> result;
        for (int i = 0; i < nums.size(); i++){
            for (int j = i + 1; j < nums.size(); j++){
                if (nums[i] + nums[j] == target){
                    result.push_back(i);
                    result.push_back(j);
                }
            }
        }
        return result;
    }   
};
~~~

## 总结

* 对于map的使用有点不熟悉,现在看来应该与python的dict是一样的,猜测cpython的dict就是由map实现的.
* iter->second表示键值,iter->first表示键.
* 暴力解法比较简单直接两个for遍历.

# 总结

* 今天几道题还算比较简单,比链表容易理解,主要看熟悉不熟悉容器的使用.
* 对于强制转换类型有学到.不管是set转vector还是vector转set.
* 学习到了set与map的使用.
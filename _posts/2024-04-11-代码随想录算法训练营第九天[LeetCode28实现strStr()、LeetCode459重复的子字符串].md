---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第九天 				# 标题 
subtitle:   字符串[LeetCode28实现strStr()、LeetCode459重复的子字符串] #副标题
date:       2024-04-11 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode28实现strStr()](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)

## 解法一[kmp]

* 时间复杂度O(m + n)

* 空间复杂度O(m)

~~~c++
class Solution {
public:
    void getNext(int *next, string s){
        // i为前缀的末尾
        // j为后缀的末尾
        int j = 0;
        next[0] = 0;
        for (int i = 1; i < s.size(); i++){
            while (j > 0 && s[i] != s[j]){
                j = next[j - 1];
            }
            if (s[i] == s[j]) {
                j++;
            }
            next[i] = j;
        }

    }

    int strStr(string haystack, string needle) {
        if (needle.size() == 0){
            return 0;
        }
        vector<int> next(needle.size());
        getNext(&next[0], needle);
        int j = 0;
        for (int i = 0; i < haystack.size(); i++){
            // 直到找到相等的再重新重这个相等的开始匹配
            while (j > 0 && haystack[i] != needle[j]){
                j = next[j - 1];
            }
            if (haystack[i] == needle[j]) {
                j++;
            }
            if (j == needle.size() ) {
                return (i - needle.size() + 1);
            }
        }
        return -1;

    }
};
~~~

## 总结

本题主要用于复习kmp算法以及如何求next数组.

# 第二题

[LeetCode459重复的子字符串](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

## 解法一[kmp应用]

* 时间复杂度O(n)

* 空间复杂度O(n)

~~~c++
class Solution {
public:
    void getNext (int* next, const string& s){
        next[0] = 0;
        int j = 0;
        for(int i = 1;i < s.size(); i++){
            while(j > 0 && s[i] != s[j]) {
                j = next[j - 1];
            }
            if(s[i] == s[j]) {
                j++;
            }
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern (string s) {
        if (s.size() == 0) {
            return false;
        }
        int next[s.size()];
        getNext(next, s);
        int len = s.size();
        if (next[len - 1] != 0 && len % (len - (next[len - 1] )) == 0) {
            return true;
        }
        return false;
    }
};
~~~

## 总结

* 感觉kmp确实还是没有理解透彻,想不到好的方法来处理这题.

# 总结

* 学习kmp算法
* 学习如何求next数组


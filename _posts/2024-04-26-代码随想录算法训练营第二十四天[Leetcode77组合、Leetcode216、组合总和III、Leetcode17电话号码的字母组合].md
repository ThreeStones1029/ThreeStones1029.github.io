---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第二十四天 				# 标题 
subtitle:   回溯[Leetcode77组合、Leetcode216组合总和III、Leetcode17电话号码的字母组合] #副标题
date:       2024-04-26 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[Leetcode77组合](https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html)

## 解法一[递归法]

* 空间复杂度O(n)

* 空间复杂度O(n)

~~~c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    // 减枝
    void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) { // 优化的地方
            path.push_back(i); // 处理节点
            backtracking(n, k, i + 1);
            path.pop_back(); // 回溯，撤销处理的节点
        }
    }

    void backtracking2(int n, int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(path);
        }
        for (int i = startIndex; i <= n; i++) {
            path.push_back(i);
            backtracking2(n, k, i+1);
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combine(int n, int k) {
        backtracking2(n, k, 1);
        return result;
    }
};
~~~

## 总结

* 正如代码随想录里面提到的，虽然能够写出显示的for循环来实现，但是一直纠结使用迭代的方法，然后想用while来实现k个for但发现还是写不出来。
* 减枝确实可以用来减少不必要的遍历。

# 第二题

[Leetcode216组合总和III](https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html)

## 解法一[递归法（先求组合，再判断组合是否满足条件）]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= 10 - (k - path.size()); i++) {
            path.push_back(i);
            backtracking(k, i+1);
            path.pop_back();
        }
    }


public:
    vector<vector<int>> combinationSum3(int k, int n) {
        backtracking(k, 1);
        vector<vector<int>> consistent_result;
        for (int i = 0; i < result.size(); i++) {
            int sum = 0;
            for (int j = 0; j < result[i].size(); j++) {
                sum += result[i][j];
            }
            if (sum == n) {
                consistent_result.push_back(result[i]);
            }
        }
        return consistent_result;
    }
};
~~~

## 解法二[递归法（在求组合的同时判断是否满足条件）]

~~~c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int k, int startIndex, int sum, int n) {
        if (sum > n) { // 剪枝操作
            return; 
        }
        if (path.size() == k) {
            if (sum == n) result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= 10 - (k - path.size()); i++) {
            sum += i;
            path.push_back(i);
            backtracking(k, i+1, sum, n);
            path.pop_back();
            sum -= i;
        }
    }


public:
    vector<vector<int>> combinationSum3(int k, int n) {
        backtracking(k, 1, 0, n);
        return result;
    } 
};
~~~

## 总结

* 确实在做了第一题来做这题会好很多。

# 第三题

[Leetcode17电话号码的字母组合](https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
private:
    vector<string> result;
    string path;
    void backtracking(vector<string> letters) {
        if (path.size() == letters.size()) {
            result.push_back(path);
            return;
        }
        string letter = letters[path.size()];
        for (int i = 0; i < letter.size(); i++) {
            path += letter[i];
            backtracking(letters);
            path = path.substr(0, path.size() - 1);
        }
    }


public:
    vector<string> letterCombinations(string digits) {
        if (digits.size() == 0) return result;
        unordered_map<char, string> number2letter;
        number2letter['2'] = "abc";
        number2letter['3'] = "def";
        number2letter['4'] = "ghi";
        number2letter['5'] = "jkl";
        number2letter['6'] = "mno";
        number2letter['7'] = "pqrs";
        number2letter['8'] = "tuv";
        number2letter['9'] = "wxyz";
        vector<string> letters;
        for (int i = 0; i < digits.size(); i++){
            string letter = number2letter[digits[i]];
            letters.push_back(letter);
        }
        backtracking(letters);
        return result;
    }
};
~~~

## 总结

* 感觉确实把回溯当作树来遍历会比较好处理，第一道自己写出来的回溯题。
* 与代码随想录不一样，在遍历第一个数对应的字母时，我是根据当前已经有的子字符串的长度来获得的。

# 总结

* 终于开始回溯了，论文被拒了，有点难受了。
* 回溯主要是理解这个组合的过程，以及考虑每一种情况。
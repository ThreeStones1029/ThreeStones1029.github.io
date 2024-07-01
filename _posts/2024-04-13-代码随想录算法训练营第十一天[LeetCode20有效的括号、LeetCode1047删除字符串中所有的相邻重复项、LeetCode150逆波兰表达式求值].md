---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十一天 				# 标题 
subtitle:   栈和队列[LeetCode20有效的括号、LeetCode1047删除字符串中所有的相邻重复项、LeetCode150逆波兰表达式求值] #副标题
date:       2024-04-13 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode20有效的括号](https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html)

## 解法一[存储左括号]

* 空间复杂度O(n)

* 空间复杂度O(n)

~~~c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> left_symbol;
        for (int i = 0; i < s.size(); i++){
            if (s[i] == '(' || s[i] == '[' || s[i] == '{'){
                left_symbol.push(s[i]);
            }
            else{
                if (!left_symbol.empty()){
                    char right_symbol = left_symbol.top();
                    left_symbol.pop();
                    if ((s[i] == right_symbol != '[') || (s[i] == '}' && right_symbol != '{') || (s[i] == ')' && right_symbol != '(')){
                        return false;
                    }
                }
                else{
                    return false;
                }
                
            }
        }
        return left_symbol.empty();
    }
};
~~~

## 解法二[存储右括号]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> right_symbol;
        if (s.size() % 2 != 0) return false;
        for (int i = 0; i < s.size(); i++){
            if (s[i] == '('){
                right_symbol.push(')');
            }
            else if (s[i] == '['){
                right_symbol.push(']');
            }
            else if (s[i] == '{'){
                right_symbol.push('}');
            }
            else if (!right_symbol.empty() && s[i] == right_symbol.top()){
                right_symbol.pop();
            }
            else{
                return false;
            }
        }
        return right_symbol.empty();
    }
};
~~~

## 总结

* 由于以前做过这样的题所以大概还是不难写出来,自己首先写的是左括号存储的方法,这种方法对于条件处理相比右括号存储的方法更多一点.
* 注意只要使用栈的pop以及top都需要先判断empty.
* 右括号存储的方式目前看起来更为方便.

# 第二题

[LeetCode1047删除字符串中所有相邻重复项](https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html)

## 解法一[栈的方式]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> char_stack;
        stack<char> result;
        string result_string(s);
        char_stack.push(s[0]);
        // 删除重复元素,判断栈顶元素与当前元素是否相等
        for (int i = 1; i < s.size();i++){
            if (!char_stack.empty() && char_stack.top() == s[i]){
                // 相等则出栈
                char_stack.pop();
            }
            else{
                // 不相等则入栈
                char_stack.push(s[i]);
            }
        }
        // 翻转栈
        while (!char_stack.empty()){
            result.push(char_stack.top());
            char_stack.pop();
        }
        // 将栈的元素以字符串的形式输出
        int n = result.size();
        int i = 0;
        while (i < n){
            result_string[i] = result.top();
            result.pop();
            i++;
        }
        result_string.resize(n);
        return result_string;
    }
};
~~~

## 解法二[原地暴力删除]

* 时间复杂度O(n * n)
* 空间复杂度O(1)

~~~c++
class Solution {
public:
    string removeDuplicates(string s) {
        int n = s.size();
        int i = 1;
        while (i < n){
            if (i - 1 >= 0 && s[i] == s[i - 1]){
                // 当有相等的字符,则向前移动两个字符
                for (int j = i; j < n - 1; j++){
                    s[j - 1] = s[j + 1];
                }
                n -= 2;
                i -= 2;
            }
            else{
                i += 1;
            }
        }
        s.resize(n);
        return s;
    }
};
~~~

## 解法三[字符串作为栈]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    string removeDuplicates(string s) {
        string result = "";
        for (char a : s){
            if (result.empty() || result.back() != a){
                result.push_back(a);
            }
            else{
                result.pop_back();
            }
        }
        return result;
    }
};
~~~

## 总结

* 整体看来这题算是比较简单,也较短时间能够写出来,使用栈的方式应该更为简单.

* 对于暴力法原地删除两个元素,主要时间消耗在移动元素上.

* 开始找回题感了.

# 第三题

[LeetCode150逆波兰表达式求值](https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html)

## 解法一[栈保存数字]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> number;
        for (int i = 0; i < tokens.size(); i++){
            // 输入数字,需要将字符串转为数字
            if (tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/"){
                int int_number = std::stoi(tokens[i]);
                number.push(int_number);
            }
            else{
                // 主要在于知道顺序,减法以及除法需要有顺序,在逆波兰表达式第一个栈顶的为表达式的第二项
                int second = number.top();
                number.pop();
                int first = number.top();
                number.pop();
                if (tokens[i] == "+"){
                    number.push(first + second);
                }
                if (tokens[i] == "-"){
                    number.push(first - second);
                }
                if (tokens[i] == "*"){
                    number.push(first * second);
                }
                if (tokens[i] == "/"){
                    number.push(first / second);
                }
            }
        }
        return number.top();
    }
};
~~~

## 总结

* 总体看来这题主要考察是否了解逆波兰表达式
* 学习std::stoi()将字符串转为数字
* 对于表达式求值顺序需要考虑清楚

# 总结

* 因为各种各样的原因,停了一个礼拜多,果然和卡哥说的一样,停了超过三天以上感觉丧失了题感.目前正在赶进度中.
* 这一天的总体难度不高,只要利用好栈的性质会比较好解决.


---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第八天 				# 标题 
subtitle:   字符串[LeetCode344反转字符串、LeetCode541反转字符串II 、卡码网54.替换数字、LeetCode151翻转字符串里的单词,卡码网55右旋转字符串] #副标题
date:       2024-04-10 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode344反转字符串](https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

## 解法一

* 时间复杂度O(n)

* 空间复杂度O(1)

~~~c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int n = s.size() - 1;
        for (int i = 0; i < s.size() / 2; i++){
            char temp = s[i];
            s[i] = s[n - i];
            s[n - i] = temp;
        }
    }
};
~~~

## 总结

本体较为简单,几分钟就可以写好.

# 第二题

[LeetCode541反转字符串II](https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html)

## 解法一[不使用reverse]

* 时间复杂度O(n)

* 空间复杂度O(1)

~~~c++
class Solution {
public:
    string reverseStr(string s, int k) {
        int num = s.size() / (2* k) + 1;
        for (int i = 0; i < num; i++){
            if (i != num - 1){
                int end = i * 2 * k + k - 1;
                int start = i * 2 * k;
                for (int m = start; m < (end + 1 + start) / 2; m++){
                    char tmp = s[m];
                    s[m] = s[end - (m - start)];
                    s[end - (m - start)] = tmp;
                }
            }
            else{
                int remain = s.size() - (num - 1) * 2 * k;
                int start = (num - 1) * 2 * k;
                if (remain < k){
                    int end = s.size() - 1;
                    for (int m = start; m < (end + 1 + start) / 2; m++){
                        char tmp = s[m];
                        s[m] = s[end - (m - start)];
                        s[end - (m - start)] = tmp;
                    }
                }
                if (remain >= k && remain < 2 * k){
                    int end = (num - 1) * 2 * k + k - 1;
                    for (int m = start; m < (end + 1 + start) / 2; m++){
                        char tmp = s[m];
                        s[m] = s[end - (m - start)];
                        s[end - (m - start)] = tmp;
                    }
                }
            }
            
        }
    return s;
    }
};
~~~

## 解法二[自己实现reverse,使用双指针]

* 时间复杂度O(n)

* 空间复杂度O(1)

~~~c++
class Solution {
public:
    void reverse(string &s, int start, int end){
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += (2 * k)) {
            // 1. 每隔 2k 个字符的前 k 个字符进行反转
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size()) {
                reverse(s, i, i + k - 1);
                continue;
            }
            // 3. 剩余字符少于 k 个，则将剩余字符全部反转。
            reverse(s, i, s.size() - 1);
        }
        return s;
    }
};
~~~

## 总结

* 暴力解法虽然容易想到,但边界还是容易出现问题,本题在于处理start和end段的字符串,将其反转即可,与上一题不同的地方在于start不是0.
* 使用双指针来实现reverse会更容易处理.

# 第三题

[卡码网54.替换数字](https://programmercarl.com/kama54.%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97.html)

## 解法一[双指针,从后往前添加]

* 时间复杂度O(n)

* 空间复杂度O(n)

~~~c++
#include <iostream>
using namespace std;
int main() {
    string s;
    while (cin >> s) {
        int sOldIndex = s.size() - 1;
        int count = 0; // 统计数字的个数
        for (int i = 0; i < s.size(); i++) {
            if (s[i] >= '0' && s[i] <= '9') {
                count++;
            }
        }
        // 扩充字符串s的大小，也就是将每个数字替换成"number"之后的大小
        s.resize(s.size() + count * 5);
        int sNewIndex = s.size() - 1;
        // 从后往前将数字替换为"number"
        while (sOldIndex >= 0) {
            if (s[sOldIndex] >= '0' && s[sOldIndex] <= '9') {
                s[sNewIndex--] = 'r';
                s[sNewIndex--] = 'e';
                s[sNewIndex--] = 'b';
                s[sNewIndex--] = 'm';
                s[sNewIndex--] = 'u';
                s[sNewIndex--] = 'n';
            } else {
                s[sNewIndex--] = s[sOldIndex];
            }
            sOldIndex--;
        }
        cout << s << endl;       
    }
}
~~~

## 解法二[从前往后]

* 时间复杂度O(n * n)

* 空间复杂度O(n)

~~~c++
#include <iostream>
using namespace std;
int main() {
    string s;
    string number = "number";
    while (cin >> s) {
        int sOldIndex = s.size() - 1;
        int count = 0; // 统计数字的个数
        for (int i = 0; i < s.size(); i++) {
            if (s[i] >= '0' && s[i] <= '9') {
                count++;
            }
        }
        // 扩充字符串s的大小，也就是将每个数字替换成"number"之后的大小
        s.resize(s.size() + count * 5);
        int sNewIndex = s.size() - 1;
        for (int i = 0; i < s.size(); i++){
            // 移动
            if (s[i] >= '0' && s[i] <= '9'){
                for (int j = sOldIndex; j > i; j--){
                    s[j + 5] = s[j];
                }
                sOldIndex +=5;
            // 修改
                for (int k = i; k <= i + 5; k++){
                    s[k] = number[k - i];
                }
            }
        }
        cout << s << endl;       
    }
}
~~~



## 总结

* 从后往前一个关键点在于从最后面添加能够避免元素整体移动,减少时间复杂度.
* 从前往后可能更容易想也比较容易实现.

# 第四题

[LeetCode151翻转字符串里的单词](https://programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)

## 解法一[自己的解法]

* 时间复杂度O(n * n)

* 空间复杂度O(1)

~~~c++
class Solution {
public:
    void reverse(string &s, int start, int end){
        for (int left = start, right = end; left < right; left++, right--){
            int temp = s[left];
            s[left] = s[right];
            s[right] = temp;
        } 
    }


    string reverseWords(string s) {
        // 统计单词数量以及字母个数确定新长度
        int word_size = 0;
        int words_num = 0;
        for (int i = 0; i < s.size(); i++){
            if (s[i] != ' '){
                for (int j = i; j < s.size(); j++){
                    if (s[j] != ' '){
                        word_size++;
                        i = j;
                    }
                    else{
                        i = j;
                        break;
                    }
                }
                words_num++;
                
            }
        }
        int new_length = word_size + words_num - 1;
        // std:: cout << new_length << std::endl;
        // std:: cout << "word_size:" << word_size << std::endl;
        // std:: cout << "words_num:" << words_num << std::endl;
        // 移除多余空字符
        int space_size = 0;
        for (int i = 0; i < s.size(); i++){
            space_size = 0;
            if (s[i] == ' '){
                for (int j = i; j < s.size(); j++){
                    if (s[j] == ' '){
                        space_size++;
                        i = j;
                    }
                    else{
                        i = j;
                        break;
                    }
                }
                // std::cout << "space_size:" << space_size << std::endl;
                if (space_size > 1){
                    for (int k = i; k < s.size(); k++){
                        s[k - (space_size - 1)] = s[k];
                    }
                }
                // std::cout << s[i] << std::endl;
                // std::cout << i << std::endl;
                i = i - (space_size - 1);
            }
            
        }
        if (s[0] == ' '){
            for (int k = 1; k < s.size(); k++){
                s[k - 1] = s[k];
            }
        }
        s.resize(new_length);
        //for (int m = 0; m < s.size(); m++){
        //    std::cout << s[m];
        //}
        // std::cout << std::endl;
        // 整体反转翻转
        reverse(s, 0, s.size() - 1);
        // 局部翻转
        int start = 0;
        while (start < s.size() -1){
            for (int end = start; end < s.size(); end++){
                if (s[end] == ' '){
                    reverse(s, start, end - 1);
                    start = end + 1;
                    break;
                }
                if (end == s.size() - 1){
                    reverse(s, start, end);
                    start = end;
                    break;
                }
            }
        }
        return s;
    }
};
~~~

## 解法二[代码随想录]

* 时间复杂度O(n)

* 空间复杂度O(1)

~~~c++
class Solution {
public:
    void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
        int slow = 0;   //整体思想参考https://programmercarl.com/0027.移除元素.html
        for (int i = 0; i < s.size(); ++i) { //
            if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                if (slow != 0) 
                    s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow); //slow的大小即为去除多余空格后的大小。
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
        reverse(s, 0, s.size() - 1);
        int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
        return s;
    }
};
~~~

## 总结

* 这道题自己的写法说实话写了挺久,但好在写出来了,而且发现自己的想法和代码随想录是一样的,先移除空字符,然后整体反转,再局部反转.主要难点卡在了移除空字符,因为不使用erase,使用resize来调整字符串大小.
* 我的移除空字符的方法: 每一次从第一个不为空的字符开始统计连续空字符的大小,然后向前移动空字符 - 1的距离,最后得到的字符串可能在开头剩下一个空字符,再整体往前移动一位即可.有一个比较关键的就是每次移动完后需要更新i的位置,因为字符串变了,所以i也要往前移动空字符数-1的大小.
* 对于整体反转与局部反转则比较简单,需要区分以下最后一个单词和其余的单词.
* 双指针来移除空字符确实比较难想到,以后再碰到这种可以使用双指针,将每次从第一个不为空字符的往前移动,移动前需要判断是否是第一个单词,第一个单词前面不需要有空格,后续单词前面都需要有空格.

# 第五题

## 解法一

* 时间复杂度O(n * k)

* 空间复杂度O(1)

~~~c++
#include <iostream>
using namespace std;

int main(){
    int k;
    string s;
    while (cin >> k >> s ){
        for (int i = 1; i <= k; i++){
            char tmp = s[s.size() - 1];
            for (int j = s.size() - 1; j > 0; j--){
                s[j] = s[j - 1];
            }
            s[0] = tmp;
        }
    }
    cout << s;
}
~~~

## 总结

* 这道题比较简单,感觉直接移动会比代码随想录的方法好,而且更容易理解,没有必要反转几次字符串,因为字符串本来就没有在题目里面要求反转

# 总结

* 第四题应该是这里面最难的一题, 也是花时间写的最久的一道题.
* 字符串整体看来和数组差不多,不过多了一些例如resize的操作.
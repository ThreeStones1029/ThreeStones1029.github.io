---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十四天 				# 标题 
subtitle:   二叉树[LeetCode144二叉树前序遍历、Leetcode94二叉树的中序遍历、Leetcode145二叉树的后续遍历] #副标题
date:       2024-04-16 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode144二叉树前序遍历](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)

## 解法一[递归法]

* 空间复杂度O(n)

* 空间复杂度O(n)

~~~c++
class Solution {
public:
    void traversal(vector<int> &result, TreeNode* cur){
        if (cur != nullptr){
            result.push_back(cur->val);
            traversal(result, cur->left);
            traversal(result, cur->right);
        }
    }

    vector<int> preorderTraversal(TreeNode* root) {
    // 递归法
        vector<int> result;
        traversal(result, root);
        return result;
    }
};
~~~

## 解法二[迭代法]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 迭代法
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr){
            return result;
        }
        stack<TreeNode*> result_stack;
        result_stack.push(root);
        while (!result_stack.empty()){
            TreeNode* cur = result_stack.top();
            result.push_back(cur->val);
            result_stack.pop();
            if (cur->right != nullptr){
                result_stack.push(cur->right);
            }
            if (cur->left != nullptr){
                result_stack.push(cur->left);
            }
        }
        return result;

    }
};
~~~

## 解法三[统一迭代]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 迭代法
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> node_stack;
        if (root != nullptr) node_stack.push(root);
        while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            if (cur != nullptr){
                // 入栈右左中, 出栈中左右
                node_stack.pop();
                if (cur->right != nullptr) node_stack.push(cur->right);
                if (cur->left != nullptr) node_stack.push(cur->left);
                node_stack.push(cur);
                node_stack.push(nullptr);
            }
            else{
                node_stack.pop();
                cur = node_stack.top();
                result.push_back(cur->val);
                node_stack.pop();
            }
        }
        return result;

    }
};
~~~

## 总结

* 递归法时间复杂度是O(n)是因为需要遍历每一个结点一次.
* 递归法空间复杂度为O(n),最坏情况下为O(n),最好情况为O(logn).
* 迭代法主要是用栈来实现递归.

# 第二题

[Leetcode94二叉树的中序遍历](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    void traversal(vector<int>& result, TreeNode* cur){
        if (cur != nullptr){
            traversal(result, cur->left);
            result.push_back(cur->val);
            traversal(result, cur->right);
        }
        
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(result, root);
        return result;

    }
};
~~~

## 解法二[迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> node_stack;
        TreeNode* cur = root;
        while (cur != nullptr || !node_stack.empty()){
            if (cur != nullptr){
                node_stack.push(cur);
                cur = cur->left;
            }
            else{
                cur = node_stack.top();
                result.push_back(cur->val);
                node_stack.pop();
                cur = cur->right;
            }
        }
        return result;
    }
};
~~~

## 解法三[统一迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> node_stack;
        if (root != nullptr) node_stack.push(root);
        while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            if (cur != nullptr){
                // 入栈顺序右中左,出栈顺序左中右
                node_stack.pop();
                if (cur->right != nullptr) node_stack.push(cur->right);
                node_stack.push(cur);
                node_stack.push(nullptr);
                if (cur->left != nullptr) node_stack.push(cur->left);
            }
            else{
                // 只有遇到空节点的时候，才将下一个节点放进结果集
                node_stack.pop();
                cur = node_stack.top();
                result.push_back(cur->val);
                node_stack.pop();
            }
        }    
        return result;
    }
};
~~~

## 总结

* 这题与前序遍历有点不同,因为需要先遍历左结点,所以需要先找到子树的最左边的结点入栈,然后再开始出栈,在出栈后再次从当前出栈的右结点重复之前的过程,直到栈空.
* 而对于在while里面判断加入cur != nullptr 是因为在中途还没有遍历完成的时候,在遍历完根结点的左边子树的时候栈是空的.需要用这个来重新进入右子树,而在右边,每次遍历完一个左子树的时候,栈是空的,这与在左边时有点不同,利用这个不断的进入没有遍历的右子, 栈为空的次数为最右边结点的个数.

# 第三题

[Leetcode145二叉树的后续遍历](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    void traversal(vector<int>& result, TreeNode* cur){
        if (cur != nullptr){
            traversal(result, cur->left);
            traversal(result, cur->right);
            result.push_back(cur->val);
        }
        
    }

    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(result, root);
        return result;
    }
};
~~~

## 解法二[迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> node_stack;
        TreeNode* cur = root;
        if (root == nullptr){
            return result;
        }
        node_stack.push(cur);
        while (!node_stack.empty()){
            cur = node_stack.top();
            result.push_back(cur->val);
            node_stack.pop();
            if (cur->left != nullptr){
                node_stack.push(cur->left);
            }
            if (cur->right != nullptr){
                node_stack.push(cur->right);
            }
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
};
~~~

## 解法三[统一迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 迭代法
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> node_stack;
        if (root != nullptr) node_stack.push(root);
        while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            if (cur != nullptr){
                // 入栈中右左, 出栈左右中
                node_stack.pop();
                node_stack.push(cur);
                node_stack.push(nullptr);
                if (cur->right != nullptr) node_stack.push(cur->right);
                if (cur->left != nullptr) node_stack.push(cur->left);
            }
            else{
                node_stack.pop();
                cur = node_stack.top();
                result.push_back(cur->val);
                node_stack.pop();
            }
        }
        return result;
    }
};
~~~

## 总结

* 后续遍历迭代法与前序遍历一样,不过调整了一下push顺序,翻转的结果为后序遍历结果,这点没想到.
* 统一迭代感觉不太容易想到,在面试的时候感觉还是递归是最实用的.

# 总结

* 主要是复习递归法以及迭代法
* 其余两种方法理解下来比较好,但是统一迭代法对于何时添加nullptr感觉还需要进一步理解.
* 赶进度中.趁着五一把落下的进度赶上,等刷完题可以开始看计算机网络了.


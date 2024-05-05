---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十六天				# 标题 
subtitle:   二叉树[LeetCode104二叉树的最大深度、Leetcode111二叉树的最小深度、Leetcode222完全二叉树的结点个数] #副标题
date:       2024-04-18 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode104二叉树的最大深度](https://programmercarl.com/0104.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.html)

## 解法一[后序遍历递归法]

* 时间复杂度O(n)

* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int get_depth(TreeNode* cur){
        if (cur == NULL) return 0;
        int left_depth = get_depth(cur->left); // 左
        int right_depth = get_depth(cur->right); // 右
        return left_depth > right_depth ? left_depth + 1 : right_depth + 1; // 中
    }
    
    int maxDepth(TreeNode* root) {
        return get_depth(root);    
    }
};
~~~

## 解法二[层序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        queue<TreeNode*> node_queue;
        if (root != NULL) node_queue.push(root);
        int depth = 0;
        while(!node_queue.empty()){
            int size = node_queue.size();
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                node_queue.pop();
                if (cur->left != NULL) node_queue.push(cur->left);
                if (cur->right != NULL) node_queue.push(cur->right);
            }
            depth++;
        }
        return depth;
    }
};
~~~

## 解法三[前序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int result;
    void get_depth(TreeNode*cur, int depth){
        result = depth > result ? depth : result; // 中
        if (cur->left == NULL && cur->right == NULL) return ;
        if (cur->left){ // 左
            depth++;
            get_depth(cur->left, depth);
            depth--;
        }
        if (cur->right){ // 右
            depth++;
            get_depth(cur->right, depth);
            depth--;
        }
        return ;
    }
    int maxDepth(TreeNode* root) {
        result = 0;
        if (root == NULL) return 0;
        get_depth(root, 1);
        return result;
    }
};
~~~

## 总结

* 最开始自己写的应该是前序遍历的递归,但是没有考虑回溯,所以左右子树高度会重复计算.
* 二叉树的高度:当前结点到叶子结点的高度.
* 二叉树的深度:根结点到当前结点的高度.
* 后序遍历实际上求的是根结点的高度,从下往上求.
* 前序遍历求的是叶子结点的深度,这个才是真正的二叉树的深度,从上往下求.

# 第二题

[Leetcode111二叉树的最小深度](https://programmercarl.com/0111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.html)

## 解法一[后序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int get_depth(TreeNode* cur){
        if (cur == NULL) return 0; // 终止条件
        int left_depth = get_depth(cur->left);
        int right_depth = get_depth(cur->right);

        if (cur->left != NULL && cur->right == NULL){
            return 1 + left_depth;
        }
        if (cur->left == NULL && cur->right != NULL){
            return 1 + right_depth;
        }
        int result = 1 + min(left_depth, right_depth);
        return result;
    }

    int minDepth(TreeNode* root) {
        return get_depth(root);
    }
};
~~~

## 解法二[前序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
private:
    int result;
    void getdepth(TreeNode* node, int depth) {
        // 函数递归终止条件
        if (node == nullptr) {
            return;
        }
        // 中，处理逻辑：判断是不是叶子结点
        if (node -> left == nullptr && node->right == nullptr) {
            result = min(result, depth);
        }
        if (node->left) { // 左
            getdepth(node->left, depth + 1);
        }
        if (node->right) { // 右
            getdepth(node->right, depth + 1);
        }
        return ;
    }

public:
    int minDepth(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        result = INT_MAX;
        getdepth(root, 1);
        return result;
    }
};
~~~

## 解法三[层序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int minDepth(TreeNode* root) {
        queue<TreeNode*> node_queue;
        if (root != NULL) node_queue.push(root);
        int depth = 0;
        while(!node_queue.empty()){
            int size = node_queue.size();
            int num = 0;
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                node_queue.pop();
                if (cur->left != NULL) node_queue.push(cur->left);
                if (cur->right != NULL) node_queue.push(cur->right);
                if (cur->right == NULL && cur->left == NULL) num++;
            }
            depth++;
            if (num != 0) break;   
        }
        return depth;
    }
};
~~~

## 总结

* 递归法三部曲: 1.确定递归函数参数与返回值.2.确定终止条件.3.确定单层递归的逻辑.
* 状态: 这道题有点不太会用递归法解决.
* 前序遍历递归关键在于中的处理是更新最小的深度.

# 第三题

[Leetcode222完全二叉树的结点个数](https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html)

## 解法一[前序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int countNodes(TreeNode* root) {
        stack<TreeNode*> node_stack;
        if (root == NULL) return 0;
        node_stack.push(root);
        int num = 0;
        while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            num += 1;
            node_stack.pop();
            if (cur->left != NULL) node_stack.push(cur->left);
            if (cur->right != NULL) node_stack.push(cur->right);
        }
        return num;
    }
};
~~~

## 解法二[中序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++

~~~

## 解法三[后序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int countNodes(TreeNode* root) {
        stack<TreeNode*> node_stack;
        if (root == NULL) return 0;
        node_stack.push(root);
        int num = 0;
        while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            num += 1;
            node_stack.pop();
            if (cur->right != NULL) node_stack.push(cur->right);
            if (cur->left != NULL) node_stack.push(cur->left);  
        }
        return num;
    }
};
~~~

## 解法四[前序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

## 解法五[中序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

## 解法六[后序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

## 解法七[前序遍历统一迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

## 解法八[中序遍历统一迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

## 解法九[后序遍历统一迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

## 解法十[层序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++

~~~

## 总结

* 这道题感觉可以用来复习几种遍历方式,包括递归,迭代,统一迭代等.

# 总结

感觉归根结底还是在于几种遍历方式是否熟悉,特别最后一题都可以用十种方式来遍历,在遍历的时候统计结点数量即可.
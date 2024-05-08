---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十七天 				# 标题 
subtitle:   二叉树[Leetcode110平衡二叉树、Leetcode257二叉树的所有路径、Leetcode404左叶子之和] #副标题
date:       2024-04-19 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode110平衡二叉树](https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html)

## 解法一[递归法]

* 空间复杂度O(n)

* 空间复杂度O(log(n))

~~~c++
class Solution {
public:
    int getHeight(TreeNode* cur){
        if (cur == NULL) return 0;
        int left_height = getHeight(cur->left);
        // 在左子树不平衡的时候提前返回
        if (left_height == -1) return -1;
        int right_height = getHeight(cur->right);
        // 在右子树不平衡的时候提前返回
        if (right_height == -1) return -1;
        return abs(left_height - right_height) > 1 ? -1 : 1 + max(left_height, right_height);
    }

    bool isBalanced(TreeNode* root) {
        return getHeight(root) == -1 ? false : true;;
    }
};
~~~

## 解法二[迭代法]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
private:
    int getDepth(TreeNode* cur){
        stack<TreeNode*> node_stack;
        if (cur !=NULL) node_stack.push(cur);
        int height = 0;
        int depth = 0;
        while(!node_stack.empty()){
            TreeNode* tmp = node_stack.top();
            if (tmp != NULL){
                node_stack.pop();
                node_stack.push(tmp);
                node_stack.push(NULL);
                if (tmp->right) node_stack.push(tmp->right);
                if (tmp->left) node_stack.push(tmp->left);
                depth++;
            }
            else{
                node_stack.pop();
                tmp = node_stack.top();
                node_stack.pop();
                depth--;
            }
            height = height > depth ? height : depth;
        }
        return height;
    }

public:
    bool isBalanced(TreeNode* root) {
        stack<TreeNode*> st;
        if (root == NULL) return true;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();                       // 中
            st.pop();
            if (abs(getDepth(node->left) - getDepth(node->right)) > 1) {
                return false;
            }
            if (node->right) st.push(node->right);           // 右（空节点不入栈）
            if (node->left) st.push(node->left);             // 左（空节点不入栈）
        }
        return true;
    }
};
~~~

## 总结

* 对于高度与深度的概念需要区分，二叉树的高度指的是从下往上，也就是当前结点到根结点的结点个数。二叉树的深度是从上往下，也就是根结点到当前结点的结点个数。
* 对于求二叉树的高度，一般使用后序遍历，求二叉树的深度一般使用前序遍历。
* 对于迭代法，本题的两个后序遍历的应用，一个用于求子树的高度，一个用于遍历结点。

# 第二题

[Leetcode257二叉树的所有路径和](https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html)

## 解法一[递归法]

~~~c++
class Solution {
public:
    void subTreepath(TreeNode* cur, vector<int>& path, vector<string>& result){
        path.push_back(cur->val); // 中
        if (cur->left == NULL && cur->right==NULL){
            string spath;
            for (int i = 0; i < path.size() - 1; i++){
                spath += to_string(path[i]);
                spath += "->";
            }
            spath += to_string(path[path.size() - 1]);
            result.push_back(spath);
        }
        if (cur->left){
            subTreepath(cur->left, path, result);
            path.pop_back(); // 回溯
        }
        if (cur->right){
            subTreepath(cur->right, path, result);
            path.pop_back(); // 回溯
        }
    }

    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        vector<int> path;
        if (root == NULL) return result;
        subTreepath(root, path, result);
        return result;
    }
};
~~~

## 解法二[迭代法]

~~~c++
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        stack<TreeNode*> node_stack;
        stack<string> spath;
        if (root == NULL) return result;
        node_stack.push(root);
        spath.push(to_string(root->val));
        while(!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            node_stack.pop();
            string path = spath.top();
            spath.pop();
            if (cur->left != NULL) {
                node_stack.push(cur->left);
                spath.push(path + "->" + to_string(cur->left->val));
            }
            if (cur->right != NULL) {
                node_stack.push(cur->right);
                spath.push(path + "->" + to_string(cur->right->val));
            }
            if (cur->left == NULL && cur->right == NULL) {
                result.push_back(path);
            }
        }
        
        return result;
    }
};
~~~

## 总结

* 感觉对于递归和迭代还是没有学习的很透彻，已经连续几道题没有写出来了，这道题主要没有想到需要一个vector来记录当前根结点到目前结点的结点。
* 对于迭代法，本题使用一个栈用来遍历，一个栈用来记录当前路径。

# 第三题

[Leetcode404左叶子之和](https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html)

## 解法一[后序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if (root == NULL) return 0;
        int leftvalue = sumOfLeftLeaves(root->left);
        if (root->left && !root->left->left && !root->left->right) { // 左子树就是一个左叶子的情况
            leftvalue = root->left->val;
        }
        int rightvalue = sumOfLeftLeaves(root->right);
        int sum = leftvalue + rightvalue;
        return sum;
    }
};
~~~

## 解法二[前序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
         int result = 0;
         stack<TreeNode*> node_stack;
         node_stack.push(root);
         while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            //std::cout << cur->val << std::endl;
            node_stack.pop();
            if (cur->left != NULL) {
                node_stack.push(cur->left);
                if (cur->left->right == NULL && cur->left->left == NULL){
                    result += cur->left->val;
                }
            }
            if (cur->right != NULL) node_stack.push(cur->right);
         }
         return result;
    }
};
~~~

## 解法三[中序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
         int result = 0;
         stack<TreeNode*> node_stack;
         TreeNode* cur = root;
         while (cur != NULL || !node_stack.empty()){
            if (cur != NULL){
                node_stack.push(cur);
                cur = cur->left;
            }
            else{
                cur = node_stack.top();
                if (cur->left != NULL && cur->left->left == NULL && cur->left->right == NULL){
                    result += cur->left->val;
                }
                node_stack.pop();
                cur = cur->right;
            }
         }
         return result;
    }
};
~~~

## 总结

* 这道题算是比较简单，迭代法应该遍历即可，递归法需要后序遍历。
* 可以用来复习集中遍历方法。

# 总结

感觉自己对于遍历的应用不是很熟悉，还是需要持续的刷题，前面断了一段时间，感觉就没有想法了。
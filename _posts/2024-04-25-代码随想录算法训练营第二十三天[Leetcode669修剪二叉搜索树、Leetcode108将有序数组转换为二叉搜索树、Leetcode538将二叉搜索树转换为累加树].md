---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第二十三天 				# 标题 
subtitle:   二叉树[Leetcode669修剪二叉搜索树、Leetcode108将有序数组转换为二叉搜索树、Leetcode538将二叉搜索树转换为累加树] #副标题
date:       2024-04-25 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[Leetcode669修剪二叉搜索树](https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)

## 解法一[递归法]

* 空间复杂度O(n)

* 空间复杂度O(n)

~~~c++
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (root == NULL) return NULL;
        if (root->val < low) return trimBST(root->right, low, high);
        if (root->val > high) return trimBST(root->left, low, high);
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
~~~

## 解法二[迭代法]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    TreeNode* trimBST1(TreeNode* root, int low, int high) {
        if (root == NULL) return root;
        // 处理头结点，让root移动到[L, R] 范围内，注意是左闭右闭
        while (root != NULL && (root->val < low || root->val > high)) {
            if (root->val < low) root = root->right;
            else root = root->left;
        }
        TreeNode *cur = root;
        while (cur != NULL) {
            while (cur->left && cur->left->val < low) {
                cur->left = cur->left->right;
            }
            cur = cur->left;
        }
        cur = root;
        while (cur != NULL) {
            while (cur->right && cur->right->val > high) {
                cur->right = cur->right->left;
            }
            cur = cur->right;
        }
        return root;
    }
};
~~~

## 总结

* 这道题的比较难，不太能做出来。
* 还需要多模拟思考思考。

# 第二题

[Leetcode108将有序数组转换为二叉搜索树](https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 中序遍历递归 左闭右开
    TreeNode* transval(vector<int>& nums, int start, int end) {
        if (end - start == 0) return NULL;
        else if (end - start == 1) {
            TreeNode* cur = new TreeNode(nums[start]);
            return cur;
        }
        else{
            int mid = (start + end) / 2;
            TreeNode* cur = new TreeNode(nums[mid]);
            cur->left = transval(nums, start, mid);
            cur->right = transval(nums, mid + 1, end);
            return cur;
        }
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode* root = NULL;
        root = transval(nums, 0, nums.size());
        return root;
    }
};
~~~

## 解法二[迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 代码随想录，迭代法，三个队列 左闭右闭
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if (nums.size() == 0) return nullptr;

        TreeNode* root = new TreeNode(0);   // 初始根节点
        queue<TreeNode*> nodeQue;           // 放遍历的节点
        queue<int> leftQue;                 // 保存左区间下标
        queue<int> rightQue;                // 保存右区间下标
        nodeQue.push(root);                 // 根节点入队列
        leftQue.push(0);                    // 0为左区间下标初始位置
        rightQue.push(nums.size() - 1);     // nums.size() - 1为右区间下标初始位置

        while (!nodeQue.empty()) {
            TreeNode* curNode = nodeQue.front();
            nodeQue.pop();
            int left = leftQue.front(); 
            leftQue.pop();
            int right = rightQue.front(); 
            rightQue.pop();
            int mid = left + ((right - left) / 2);

            curNode->val = nums[mid];       // 将mid对应的元素给中间节点

            if (left <= mid - 1) {          // 处理左区间
                curNode->left = new TreeNode(0);
                nodeQue.push(curNode->left);
                leftQue.push(left);
                rightQue.push(mid - 1);
            }

            if (right >= mid + 1) {         // 处理右区间
                curNode->right = new TreeNode(0);
                nodeQue.push(curNode->right);
                leftQue.push(mid + 1);
                rightQue.push(right);
            }
        }
        return root;
    }
};
~~~

## 总结

* 这道题递归还可以，思路也和卡哥代码随想录一样
* 但迭代法有点想不到，使用三个队列来记录，一个记录开始下标，一个应该是记录的结束下标而不是右边的初始下标。

# 第三题

[Leetcode538将二叉搜索树转换为累加树](https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
	// 递归法
    void transval(TreeNode* cur, int& sum) {
        if (cur->right != NULL) transval(cur->right, sum);
        if (cur != NULL) {
            cur->val += sum;
            sum = cur->val;
        }
        if (cur->left != NULL) transval(cur->left, sum);
    }

    TreeNode* convertBST(TreeNode* root) {
        if (root == NULL) return NULL;
        int sum = 0;
        transval(root, sum); // 注意这里不能直接赋值0,需要用变量名
        return root;
    }
};
~~~

## 解法二[迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 右根左 迭代法
    TreeNode* convertBST(TreeNode* root) {
        TreeNode* cur = root;
        stack<TreeNode*> node_stack;
        int sum = 0;
        while (cur || !node_stack.empty()) {
            if (cur != NULL) {
                while (cur) {
                    node_stack.push(cur);
                    cur = cur->right;
                }
            }
            else {
                cur = node_stack.top();
                node_stack.pop();
                cur->val += sum;
                sum = cur->val;
                cur = cur->left;
            }
        }
        return root;
    }
};
~~~

## 解法三[统一迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 统一迭代法
    TreeNode* convertBST(TreeNode* root) {
        if (root == NULL) return NULL;
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        TreeNode* cur = NULL;
        int sum = 0;
        while (!node_stack.empty()) {
            TreeNode* cur = node_stack.top();
            node_stack.pop();
            if (cur != NULL){
                if (cur->left != nullptr) node_stack.push(cur->left);
                node_stack.push(cur);
                node_stack.push(nullptr);
                if (cur->right != nullptr) node_stack.push(cur->right);
            }
            else{
                // 只有遇到空节点的时候，才开始遍历
                cur = node_stack.top();
                cur->val += sum;
                sum = cur->val;
                node_stack.pop();
            }
        }
        return root;
    }
};
~~~

## 总结

* 这道题比较简单，主要在于理解题意即可，也即理解到他是与中序遍历相反的遍历即可，然后就可以以中序遍历相反的方向遍历，然后不断叠加数值。
* 统一迭代法还需要复习一下，不是很熟练，有些细节需要再理解一下。

# 总结

* 树终于做完了，现在看来起始如果掌握了遍历也不算难，这部分主要还是考察熟练度。
* 关键在于几种遍历方式，前序、中序、后序以及层序遍历。
* 对于回溯需要注意。
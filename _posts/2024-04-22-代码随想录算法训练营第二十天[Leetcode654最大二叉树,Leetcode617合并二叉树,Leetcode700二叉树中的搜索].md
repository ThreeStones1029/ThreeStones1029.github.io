---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十九天 				# 标题 
subtitle:   二叉树[Leetcode654最大二叉树,Leetcode617合并二叉树,Leetcode700二叉树中的搜索] #副标题
date:       2024-04-22 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[Leetcode654最大二叉树](https://programmercarl.com/0654.%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html)

## 解法一[前序遍历递归法]

~~~c++
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if (nums.size() == 0) return NULL;
        int max = nums[0];
        int max_index = 0;
        for (int index = 0; index < nums.size(); index++){
            if (nums[index] > max){
                max = nums[index];
                max_index = index;
            }
        }
        TreeNode* root = new TreeNode(nums[max_index]);
        vector<int> nums_left(nums.begin(), nums.begin() + max_index);
        vector<int> nums_right(nums.begin() + max_index + 1, nums.end());
        root->left = constructMaximumBinaryTree(nums_left);
        root->right = constructMaximumBinaryTree(nums_right);
        return root;
    }
};
~~~

## 解法二[前序遍历递归法-使用数组下标]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++

class Solution {
public:
    TreeNode* traversal(vector<int>& nums, int nums_begin, int nums_end){
        if (nums_end - nums_begin == 0) return NULL;
        int max = nums[nums_begin];
        int max_index = nums_begin;
        for (int i = nums_begin; i < nums_end; i++){
            if (nums[i] > max){
                max = nums[i];
                max_index = i;
            }
        }
        TreeNode* root = new TreeNode(nums[max_index]);
        int nums_left_begin = nums_begin;
        int nums_left_end = max_index;
        int nums_right_begin = max_index + 1;
        int nums_right_end = nums_end;
        root->left = traversal(nums, nums_left_begin, nums_left_end);
        root->right = traversal(nums, nums_right_begin, nums_right_end);
        return root;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if (nums.size() == 0) return NULL;
        return traversal(nums, 0, nums.size());
    }
};
~~~

## 总结

* 这道题如果做了构造二叉树还是比较简单的.
* 总算碰到简单题了.

# 第二题

[Leetcode617合并二叉树](https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (root1 == NULL) return root2;
        if (root2 == NULL) return root1;
        TreeNode* root = new TreeNode(root1->val + root2->val);
        root->left = mergeTrees(root1->left, root2->left);
        root->right = mergeTrees(root1->right, root2->right);
        return root;
    }
};
~~~

## 解法二[层序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (root1 == NULL) return root2;
        if (root2 == NULL) return root1;
        queue<TreeNode*> node_queue;
        node_queue.push(root1);
        node_queue.push(root2);
        while (!node_queue.empty()){
            TreeNode* node1 = node_queue.front();
            node_queue.pop();
            TreeNode* node2 = node_queue.front();
            node_queue.pop();
            node1->val += node2->val;
            if (node1->left != NULL && node2->left != NULL){
                node_queue.push(node1->left);
                node_queue.push(node2->left);
            }
            if (node1->right != NULL && node2->right != NULL){
                node_queue.push(node1->right);
                node_queue.push(node2->right);
            }
            if (node1->left == NULL && node2->left != NULL) {
                node1->left = node2->left;
            }
            if (node1->right == NULL && node2->right != NULL) {
                node1->right = node2->right;
            }
        }
        return root1;
    }
};
~~~

## 总结

* 这道题确实比较简单，和公司对接代码停了一段时间，继续重新开始刷题。
* 层序遍历这种方法有点像改变指针指向的做法，在当前结点都存在左结点或者的情况下，将第二棵树的值加到第一颗树上，如果第一颗树左结点或者右结点不存在，而对应第二棵树的左结点或者右结点存在，则改变第一颗树的指针指向第二棵树的结点。当第一颗树左结点或者右结点存在而第二棵树的对应左右结点不存在，则什么操作都不做，因为返回的是第一颗树的根结点。

# 第三题

[Leetcode700二叉树中的搜索](https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
// 普通二叉树做法
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == NULL) return NULL;
        if (root->val == val) return root;
        TreeNode* left = searchBST(root->left, val);
        if (left != NULL) return left;
        TreeNode* right = searchBST(root->right, val);
        if (right != NULL) return right;
        return NULL;
    }
};
// 二叉搜索树的做法
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == NULL || root->val == val) return root;
        if (val > root->val) return searchBST(root->right, val);
        if (val < root->val) return searchBST(root->left, val);
        return NULL;
    }
};
~~~

## 解法二[普通二叉树前序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
// 普通二叉树做法
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == NULL) return NULL;
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        TreeNode* cur;
        while (!node_stack.empty()) {
            cur = node_stack.top();
            node_stack.pop();
            if (cur->val == val) break;
            if (cur->left != NULL) node_stack.push(cur->left);
            if (cur->right != NULL) node_stack.push(cur->right);
        }
        if (cur->val != val) return NULL;
        return cur;
    }
};
~~~

## 解法三[普通二叉树层序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
// 普通二叉树做法
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == NULL) return NULL;
        queue<TreeNode*> node_queue;
        node_queue.push(root);
        TreeNode* cur;
        while (!node_queue.empty()) { 
            int size = node_queue.size();
            int i = 0;
            for (i = 0; i < size; i++) {
                cur = node_queue.front();
                node_queue.pop();
                if (cur->val == val) break;
                if (cur->left != NULL) node_queue.push(cur->left);
                if (cur->right != NULL) node_queue.push(cur->right);
            }
            // i小于size说明找到了这个值，提前退出
            if (i < size) break;
        }
        // 需要判断最后的值等不等于val
        if (cur->val != val) return NULL;
        return cur;
    }
};
~~~

## 解法四[普通二叉树中序遍历迭代法]

~~~c++
// 普通二叉树做法 中序遍历迭代法
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == NULL) return NULL;
        stack<TreeNode*> node_stack;
        TreeNode* cur = root;
        // 当cur不为空或者队列不为空
        while (cur != NULL || !node_stack.empty()){
            // 加入左结点
            if (cur != NULL) {
                node_stack.push(cur);
                cur = cur->left;
            }
            else{
                cur = node_stack.top();
                node_stack.pop();
                if (cur->val == val) break;
                cur = cur->right;
            }
        }
        return cur; // 这里可以直接返回，因为如果没找到就是为空结点
    }
};
~~~

## 解法五[二叉搜索树迭代法]

~~~c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        TreeNode* cur = root;
        while (cur != NULL) {
            if (cur->val > val) cur = cur->left;
            else if (cur->val < val) cur = cur->right;
            else return cur;
        }
        return NULL;
    }
};
~~~

## 总结

* 这一题总体来说更像是对于遍历方法的一个复习

  对于普通二叉树的搜索与二叉搜索树的搜索有点不同，二叉搜索树的左边子树结点值均小于根结点，右子树的结点值均大于根结点，同时左子树与右子树均为二叉搜索树。

# 第四题

[Leetcode98验证搜索二叉树](https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)

## 解法一[自己的递归法]

~~~c++
class Solution {
public:
    // 判断子树的值是否都小于上一层的结点的值
    bool is_max(TreeNode* root, int val) {
        if (root == NULL) return true;
        if (root != NULL && root->val >= val) return false;
        return is_max(root->left, val) && is_max(root->right, val);
    }
    // 判断子树的值是否都大于上一层的结点的值
    bool is_min(TreeNode* root, int val) {
        if (root == NULL) return true;
        if (root != NULL && root->val <= val) return false;
        return is_min(root->left, val) && is_min(root->right, val);
    }


    bool isValidBST(TreeNode* root) {
        if (root == NULL) return true;
        bool left_is_validbst = false;
        bool right_is_validbst = false;
        // 对于左边子树在不为空的情况下
        if (root->left != NULL) {
            // 在当前结点的值大于左结点的值，同时下一个作子树需要符合要求
            if (root->val > root->left->val && isValidBST(root->left)) 
                // 判断当前结点是否大于左子树的下面的所有结点的值
                left_is_validbst = is_max(root->left, root->val);
        }
        else {
            left_is_validbst = true;
        }
        // 对于右边子树在不为空的情况下
        if (root->right != NULL) {
            if (root->val < root->right->val && isValidBST(root->right)) 
                // 判断当前结点是否大于左子树的下面的所有结点的值
                right_is_validbst = is_min(root->right, root->val);
        }
        else {
            right_is_validbst = true;
        }
        return right_is_validbst && left_is_validbst;
    }
};
~~~

## 解法二[代码随想录的递归法]

~~~c++
class Solution {
public:
    long long maxVal = LONG_MIN; // 因为后台测试数据中有int最小值
    bool isValidBST(TreeNode* root) {
        if (root == NULL) return true;
        bool left = isValidBST(root->left);
        // 中序遍历，验证遍历的元素是不是从小到大
        // 相当于在一直判断当前的结点是否一直在变大，如果没有变大证明不是二叉搜索树
        if (maxVal < root->val) maxVal = root->val;
        else return false;
        bool right = isValidBST(root->right);
        return left && right;
    }
};
~~~

## 解法三[中序遍历迭代法]

~~~c++
class Solution {
public:
    long long maxVal = LONG_MIN; // 因为后台测试数据中有int最小值
    bool isValidBST(TreeNode* root) {
        if (root == NULL) return true;
        stack<TreeNode*> node_stack;
        TreeNode* cur = root;
        while (cur != NULL || !node_stack.empty()) {
            if (cur != NULL) {
                node_stack.push(cur);
                cur = cur->left;
            }
            else {
                cur = node_stack.top();
                node_stack.pop();
                if (cur->val > maxVal) maxVal = cur->val;
                else return false;
                cur = cur->right;
            }
        }
        return true;
    }
};
~~~

## 总结

* 这一题坑还是能够发现，但是自己写的递归法过于复杂，存在一些重复的代码。

* 中序遍历是否数值在增大也可以用来判断是否是二叉搜索树。

# 总结

* 停了一段时间没刷题，确实题感少了很多，好在又继续了。
* 树感觉主要还是在考察几种递归与迭代的遍历方式的熟练与否。
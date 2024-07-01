---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第二十一天 				# 标题 
subtitle:   二叉树[Leetcode530二叉搜索树中的最小绝对差、Leetcode501二叉搜索树中的众数、Leetcode236二叉树中的最近公共祖先] #副标题
date:       2024-04-23 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[Leetcode530二叉搜索树中的最小绝对差](https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html)

## 解法一[中序遍历迭代法]

* 空间复杂度O(n)

* 空间复杂度O(n)

~~~c++
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        TreeNode* cur = root;
        int pre_val = root->val;
        int i = 1; // 主要用来在第一个的时候不计算差，等到第二个元素才开始计算
        int different = INT32_MAX;
        stack<TreeNode*> node_stack;
        while (cur != NULL || !node_stack.empty()) {
            if (cur != NULL) {
                node_stack.push(cur);
                cur = cur->left;
            }
            else {
                cur = node_stack.top();
                node_stack.pop();
                if (i != 1) {
                    if (cur->val - pre_val < different) different = cur->val - pre_val;
                }
                i += 1;
                pre_val = cur->val;
                cur = cur->right;
            }
        }
        return different;
    }
};
~~~

## 解法二[中序遍历递归法]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    void middle_order_traversal(TreeNode* root, vector<int>& result) {
        if (root->left != NULL) middle_order_traversal(root->left, result);
        if (root != NULL) result.push_back(root->val);
        if (root->right != NULL) middle_order_traversal(root->right, result);
    }

    int getMinimumDifference(TreeNode* root) {
        vector<int> result;
        middle_order_traversal(root, result);
        int different = result[1] - result[0];
        for (int i = 1; i < result.size(); i++) {
            if (result[i] - result[i - 1] < different) different = result[i] - result[i - 1];
        }
        return different;
    }
};
~~~

## 解法三[中序遍历统一迭代]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
// 中序遍历统一迭代法
class Solution {
public:
    void middle_order_traversal(TreeNode* root, vector<int>& result) {
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        while (!node_stack.empty()) {
            TreeNode* cur = node_stack.top();
            if (cur != NULL) {
                node_stack.pop();
                if (cur->right != nullptr) node_stack.push(cur->right);
                node_stack.push(cur);
                node_stack.push(nullptr);
                if (cur->left != nullptr) node_stack.push(cur->left);
            }
            else {
                // 只有遇到空节点的时候，才将下一个节点放进结果集
                node_stack.pop();
                cur = node_stack.top();
                result.push_back(cur->val);
                node_stack.pop();
            }
        }
    }

    int getMinimumDifference(TreeNode* root) {
        vector<int> result;
        middle_order_traversal(root, result);
        int different = result[1] - result[0];
        for (int i = 1; i < result.size(); i++) {
            if (result[i] - result[i - 1] < different) different = result[i] - result[i - 1];
        }
        return different;
    }
};
~~~

## 总结

* 这道题总体来说还是比较简单不管是用前向指针或者数组记录中序遍历的值，关键在于如何中序遍历。
* 复习了一下统一迭代法的中序遍历，发现还是理解的不透彻。

# 第二题

[Leetcode501二叉搜索树中的众数](https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html)

## 解法一[递归法、迭代法、统一迭代法使用unorder_map]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 统一迭代中序遍历
    void unified_iteration_middle_order_traversal(TreeNode* root, vector<int>& result) {
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        while (!node_stack.empty()) {
            TreeNode* cur = node_stack.top();
            if (cur != NULL) {
                node_stack.pop();
                if (cur->right != NULL) node_stack.push(cur->right);
                node_stack.push(cur);
                node_stack.push(NULL);
                if (cur->left != NULL) node_stack.push(cur->left);
            }
            else {
                node_stack.pop();
                cur = node_stack.top();
                node_stack.pop();
                result.push_back(cur->val);
            }
        }
    }
    // 递归法中序遍历
    void recursion_middle_order_traversal(TreeNode* root, vector<int>& result) {
        if (root->left != NULL) recursion_middle_order_traversal(root->left, result);
        if (root != NULL) result.push_back(root->val);
        if (root->right != NULL) recursion_middle_order_traversal(root->right, result); 
    }

    // 迭代法中序遍历
    void iteration_middle_order_traversal(TreeNode* root, vector<int>& result) {
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
                result.push_back(cur->val);
                cur = cur->right;
            }
        }
    }

    vector<int> findMode(TreeNode* root) {
        vector<int> tree_vals;
        vector<int> result;
        unordered_map<int, int> result_map;
        // unified_iteration_middle_order_traversal(root, tree_vals); // 统一迭代中序遍历
        // recursion_middle_order_traversal(root, tree_vals);// 递归法中序遍历
        iteration_middle_order_traversal(root, tree_vals);
        int num = 1;
        // if (tree_vals.size() == 1) return tree_vals[0];
        for (int i = 0; i < tree_vals.size(); i++) {
            auto it = result_map.find(tree_vals[i]);
            if (it != result_map.end()) {
                result_map[tree_vals[i]]++;
            }
            else {
                result_map[tree_vals[i]] = 1;
            }
        }
        auto max_pair = result_map.begin();
        // 迭代映射，寻找值最大的键
        for (auto it = result_map.begin(); it != result_map.end(); ++it) {
            if (it->second > max_pair->second) {
                max_pair = it;
            }
        }
        result.push_back(max_pair->first);
        for (auto it = result_map.begin(); it != result_map.end(); ++it) {
            if (it->second == max_pair->second && it->first != max_pair->first) {
                result.push_back(it->first);
            }
        }
        return result;
    }
};
~~~

## 解法二[递归法、迭代法、统一迭代法直接使用数组]

~~~c++
class Solution {
public:
    // 统一迭代中序遍历
    void unified_iteration_middle_order_traversal(TreeNode* root, vector<int>& result) {
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        while (!node_stack.empty()) {
            TreeNode* cur = node_stack.top();
            if (cur != NULL) {
                node_stack.pop();
                if (cur->right != NULL) node_stack.push(cur->right);
                node_stack.push(cur);
                node_stack.push(NULL);
                if (cur->left != NULL) node_stack.push(cur->left);
            }
            else {
                node_stack.pop();
                cur = node_stack.top();
                node_stack.pop();
                result.push_back(cur->val);
            }
        }
    }
    
    // 递归法中序遍历
    void recursion_middle_order_traversal(TreeNode* root, vector<int>& result) {
        if (root->left != NULL) recursion_middle_order_traversal(root->left, result);
        if (root != NULL) result.push_back(root->val);
        if (root->right != NULL) recursion_middle_order_traversal(root->right, result); 
    }

    // 迭代法中序遍历
    void iteration_middle_order_traversal(TreeNode* root, vector<int>& result) {
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
                result.push_back(cur->val);
                cur = cur->right;
            }
        }
    }

    vector<int> findMode2(TreeNode* root) {
        vector<int> tree_vals;
        vector<int> result;
        
        // unified_iteration_middle_order_traversal(root, tree_vals); // 统一迭代中序遍历
        // recursion_middle_order_traversal(root, tree_vals);// 递归法中序遍历
        iteration_middle_order_traversal(root, tree_vals);
        int cur_val = tree_vals[0];
        int num = 1;
        int vals_max_num = 1;
        for (int i = 1; i < tree_vals.size(); i++) {
            if (tree_vals[i] == cur_val) {
                num++;
            }
            else {
                if (num > vals_max_num) {
                    // 若大于当前结果里面的数对应的数量，则全部删除后加入
                    while (result.size() > 0)
                    {
                       result.pop_back();
                    }
                    result.push_back(cur_val);
                    vals_max_num = num;
                }else if (num == vals_max_num){
                    result.push_back(cur_val);
                }
                cur_val = tree_vals[i];
                num = 1;
            }
        }
        // 处理最后的一个元素
        if (num > vals_max_num) {
            while (result.size() > 0)
            {
                result.pop_back();
            }
            result.push_back(cur_val);
        }
        if (num == vals_max_num) {
            result.push_back(cur_val);
        }
        return result;
    }

    vector<int> findMode(TreeNode* root) {
        // return findMode1(root); // 第一种求众数
        return findMode2(root); // 第二种求众数
    }
};
~~~

## 解法三[迭代法遍历树的同时操作]

~~~c++
class Solution {
public:
    // 迭代法在遍历树的时候操作
    vector<int> findMode4(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> node_stack;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        int vals_max_num = 0;
        int num = 0;
        while (cur != NULL || !node_stack.empty()) {
            if (cur != NULL) {
                node_stack.push(cur);
                cur = cur->left;
            }
            else {
                cur = node_stack.top();
                node_stack.pop();
                if (pre == NULL) {
                    num = 1;
                }
                else if (pre->val == cur->val) {
                    num++;
                }
                else {
                    num = 1;
                }
                if (num == vals_max_num) {
                    result.push_back(cur->val);
                }
                if (num > vals_max_num) { // 如果计数大于最大值频率
                    vals_max_num = num;   // 更新最大频率
                    result.clear();     // 很关键的一步，不要忘记清空result，之前result里的元素都失效了
                    result.push_back(cur->val);
                }
                pre = cur;
                cur = cur->right;
            }
        }     
        return result;
    }
    
    vector<int> findMode(TreeNode* root) {
        return findMode4(root); // 迭代法在遍历树的时候操作
    }
};
~~~

## 解法四[递归法遍历树的时候同时操作(来自代码随想录)]

~~~c++
class Solution {
private:
    int maxCount = 0; // 最大频率
    int count = 0; // 统计频率
    TreeNode* pre = NULL;
    vector<int> result;
    void searchBST(TreeNode* cur) {
        if (cur == NULL) return ;

        searchBST(cur->left);       // 左
                                    // 中
        if (pre == NULL) { // 第一个节点
            count = 1;
        } else if (pre->val == cur->val) { // 与前一个节点数值相同
            count++;
        } else { // 与前一个节点数值不同
            count = 1;
        }
        pre = cur; // 更新上一个节点

        if (count == maxCount) { // 如果和最大值相同，放进result中
            result.push_back(cur->val);
        }

        if (count > maxCount) { // 如果计数大于最大值频率
            maxCount = count;   // 更新最大频率
            result.clear();     // 很关键的一步，不要忘记清空result，之前result里的元素都失效了
            result.push_back(cur->val);
        }

        searchBST(cur->right);      // 右
        return ;
    }

public:
    vector<int> findMode(TreeNode* root) {
        count = 0;
        maxCount = 0;
        pre = NULL; // 记录前一个节点
        result.clear();

        searchBST(root);
        return result;
    }
~~~

## 总结

* 这道题关键还是在于如何利用排序好的数组求众数。
* 第一种求众数的方法可以用于没有排序的数组。
* 第二种求众数的方法适用于排序好的数组。
* 第三种第四种方法是遍历树的时候操作。

# 第三题

[Leetcode236二叉树中的最近公共祖先](https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) return root;
        if (root == p || root == q) return root;
        // 后续遍历
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left != NULL && right != NULL) return root;
        else if (left == NULL && right != NULL){
            return right;
        } else if (left != NULL && right == NULL){
            return left;
        }
        else return NULL;
    }
};
~~~

## 总结

* 这道题还是比较难，虽然代码比较短，但关键在于掌握回溯也即如何从下往上遍历返回结点。

# 总结

* 继续补卡，发现对于遍历可以掌握，但是变换大一点像这个第三题就有点无从下手。
* 还是需要继续坚持刷题，虽然没有及时刷完，但还是得继续。
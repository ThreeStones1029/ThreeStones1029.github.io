---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第十五天 				# 标题 
subtitle:   二叉树[二叉树层序遍历、Leetcode226翻转二叉树、Leetcode101对称二叉树] #副标题
date:       2024-04-17 				# 时间
author:     BY ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题[层序遍历]

## 第一小题[LeetCode102二叉树层序遍历]

[LeetCode102二叉树层序遍历](https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        // 队列实现
        vector<vector<int>> result;
        queue<TreeNode*> node_queue;
        if (root != nullptr) node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            vector<int> every_layer;
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                if (cur->left != nullptr) node_queue.push(cur->left);
                if (cur->right != nullptr) node_queue.push(cur->right);
                every_layer.push_back(cur->val);
                node_queue.pop();
            }
            result.push_back(every_layer);
        }
    return result;

    }
};
~~~

## 第二小题[Leetcode107二叉树的层次遍历]

[Leetcode107二叉树的层次遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        // 队列实现
        vector<vector<int>> result;
        queue<TreeNode*> node_queue;
        if (root != nullptr) node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            vector<int> every_layer;
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                if (cur->left != nullptr) node_queue.push(cur->left);
                if (cur->right != nullptr) node_queue.push(cur->right);
                every_layer.push_back(cur->val);
                node_queue.pop();
            }
            result.push_back(every_layer);
        }
        // 翻转二维列表, 与上一题基本相同,交换二维列表每一行即可
        int row = result.size();
        for (int i = 0; i < row / 2; i++){
            swap(result[i], result[row - i - 1]);
        }
        return result;

    }
};
~~~

## 第三小题[Leetcode199二叉树的右视图]

[Leetcode199二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> node_queue;
        if (root != nullptr) node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            vector<int> every_layer;
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                if (cur->left != nullptr) node_queue.push(cur->left);
                if (cur->right != nullptr) node_queue.push(cur->right);
                every_layer.push_back(cur->val);
                node_queue.pop();
            }
            result.push_back(every_layer);
        }
        // 取二维列表的每一行的最后一个元素
        vector<int> final_result;
        for (int i = 0; i < result.size(); i++){
            final_result.push_back(result[i][result[i].size() - 1]);
        }
        return final_result;

    }
};
~~~

## 第四小题[Leetcode637二叉树的层平均值]

[Leetcode637二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        // 队列实现
        vector<vector<int>> result;
        queue<TreeNode*> node_queue;
        if (root != nullptr) node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            vector<int> every_layer;
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                if (cur->left != nullptr) node_queue.push(cur->left);
                if (cur->right != nullptr) node_queue.push(cur->right);
                every_layer.push_back(cur->val);
                node_queue.pop();
            }
            result.push_back(every_layer);
        }
        vector<double> mean_layer_value_result;
        for (int i = 0; i < result.size(); i++){
            double sum = 0;
            for (int j = 0; j < result[i].size(); j++){
                sum += result[i][j];
            }
            double mean = sum / result[i].size();
            mean_layer_value_result.push_back(mean);
        }
        return mean_layer_value_result;

    }
};
~~~

## 第五小题[Leetcode429n叉树的层序遍历]

[Leetcode429n叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> result;
        queue<Node*> node_queue;
        if (root != nullptr) node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            vector<int> layer;
            for (int i = 0; i < size; i++){
                Node* cur = node_queue.front();
                for (int j = 0; j < cur->children.size(); j++){
                    node_queue.push(cur->children[j]);
                }
                layer.push_back(cur->val);
                node_queue.pop();
            } 
            result.push_back(layer);
        }
        return result;
    }
};
// 熟悉二叉树的层序遍历写起来还是不难的.
~~~

## 第六小题[Leetcode515在每行树中找最大值]

[Leetcode515在每行树中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> node_queue;
        if (root != nullptr) node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            vector<int> layer;
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                if (cur->left != nullptr) node_queue.push(cur->left);
                if (cur->right != nullptr) node_queue.push(cur->right);
                layer.push_back(cur->val);
                node_queue.pop();
            }
            result.push_back(layer);
        }
        vector<int> max_value_result;
        for (int i = 0; i < result.size(); i++){
            int max = result[i][0];
            for (int j = 0; j < result[i].size(); j++){
                if (result[i][j] > max){
                    max= result[i][j];
                }
            }
            max_value_result.push_back(max);
        }
        return max_value_result;

    }
};
~~~

## 第七小题[Leetcode116填充每个结点的下一个右侧指针]

[Leetcode116填充每个结点的下一个右侧指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    Node* connect(Node* root) {
        vector<vector<Node*>> result;
        queue<Node*> node_queue;
        if (root == NULL) return root;
        node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            vector<Node*> layer;
            for (int i = 0; i < size; i++){
                Node* cur = node_queue.front();
                layer.push_back(cur);
                node_queue.pop();
                if (cur->left != NULL) node_queue.push(cur->left);
                if (cur->right != NULL) node_queue.push(cur->right);
            }
            result.push_back(layer);
        }
        for (int i = 0; i < result.size(); i++){
            for (int j = 0; j < result[i].size(); j++){
                if ( j != result[i].size() - 1){
                    result[i][j]->next = result[i][j + 1];
                }
                else{
                    result[i][j]->next = NULL;
                }
            }
        }
        return result[0][0];
    }
};
~~~

## 第八小题[Leetcode117填充每个结点的下一个右侧指针II]

[Leetcode117填充每个结点的下一个右侧指针II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

~~~c++
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> que;
        if (root != NULL) que.push(root);
        while (!que.empty()) {
            int size = que.size();
            vector<int> vec;
            Node* nodePre;
            Node* node;
            for (int i = 0; i < size; i++) {
                if (i == 0) {
                    nodePre = que.front(); // 取出一层的头结点
                    que.pop();
                    node = nodePre;
                } else {
                    node = que.front();
                    que.pop();
                    nodePre->next = node; // 本层前一个节点next指向本节点
                    nodePre = nodePre->next;
                }
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            nodePre->next = NULL; // 本层最后一个节点指向NULL
        }
        return root;
    }
};
~~~

## 第九小题[Leetcode104二叉树的最大深度]

[Leetcode104二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

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

## 第十小题[Leetcode111二叉树的最小深度]

[Leetcode111二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

* 层序遍历空间复杂度O(n)
* 层序遍历空间复杂度O(n)

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

* 整体看这些题都很像,相当于举一反三,写起来确实也不难.
* 只要层序遍历会,其他的不过是层序遍历的应用.

# 第二题

[Leetcode226翻转二叉树](https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html)

## 解法一[前序遍历递归法]

* 时间复杂度O(n):每个结点操作一次
* 空间复杂度:树的深度

~~~c++
class Solution {
public:
    void invert(TreeNode* cur){
        if (cur != NULL){
            TreeNode* tmp = cur->right;
            cur->right = cur->left;
            cur->left = tmp;
            invert(cur->left);
            invert(cur->right);
        }
    }
    TreeNode* invertTree(TreeNode* root) {
        invert(root);
        return root;
    }
};
~~~

## 解法二[中序遍历递归法]

~~~c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        invertTree(root->left);
        swap(root->left, root->right);
        // 重复翻转左子树,因为交换了
        invertTree(root->left);
        return root;
    }
};
~~~

## 解法三[前序遍历迭代法]

* 时间复杂度O(n)
* 空间复杂度,树的宽度

~~~c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        while(!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            node_stack.pop();
            swap(cur->left, cur->right);
            if (cur->left != NULL) node_stack.push(cur->left);
            if (cur->right != NULL) node_stack.push(cur->right);
        }
        return root;
    }
};
~~~

## 解法四[层序遍历翻转]

* 时间复杂度O(n)
* 空间复杂度数的最大宽度

~~~c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        queue<TreeNode*> node_queue;
        node_queue.push(root);
        while (!node_queue.empty()){
            int size = node_queue.size();
            for (int i = 0; i < size; i++){
                TreeNode* cur = node_queue.front();
                swap(cur->left, cur->right);
                if (cur->left != NULL) node_queue.push(cur->left);
                if (cur->right != NULL) node_queue.push(cur->right);
                node_queue.pop();
            }
        }
        return root;
    }
};
~~~

## 解法五[前序遍历统一迭代法]

~~~c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            if (cur != NULL){
                node_stack.pop();
                if (cur->right != NULL) node_stack.push(cur->right); // 右
                if (cur->left != NULL) node_stack.push(cur->left); // 左
                node_stack.push(cur);                                     // 中
                node_stack.push(NULL);
            }
            else{
                node_stack.pop();
                cur = node_stack.top();
                swap(cur->right, cur->left);
                node_stack.pop();
            }
        }
        return root;
    }
};
~~~

## 解法六[中序遍历统一迭代法]

~~~c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        stack<TreeNode*> node_stack;
        node_stack.push(root);
        while (!node_stack.empty()){
            TreeNode* cur = node_stack.top();
            if (cur != NULL){
                node_stack.pop();
                if (cur->right != NULL) node_stack.push(cur->right); // 右
                node_stack.push(cur);                              // 中
                node_stack.push(NULL);
                if (cur->left != NULL) node_stack.push(cur->left); // 左
            }
            else{
                node_stack.pop();
                cur = node_stack.top();
                swap(cur->right, cur->left);
                node_stack.pop();
            }
        }
        return root;
    }
};
~~~

## 总结

* 这道题递归写起来不难.
* 迭代法相当于前序遍历
* 层序遍历与前序遍历都很像,就是在入栈或者入队列前交换左右顺序.
* 再次复习统一迭代法,对于空结点的标记,只有遇到空结点才会开始出栈并做处理.
* 感觉做一道题,使用不同方法像是做了n道题

# 第三题

[Leetcode101对称二叉树](https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html)

## 解法一[后序遍历递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    bool  compare(TreeNode* left, TreeNode* right){
        // 首先排除空节点的情况
        if (left == NULL && right != NULL) return false;
        else if (left != NULL && right == NULL) return false;
        else if (left == NULL && right == NULL) return true;
        else if (left->val != right->val) return false;

        // 下面为：左右节点都不为空，且数值相同的情况
        bool outside = compare(left->left, right->right);   // 左子树：左、 右子树：右
        bool inside = compare(left->right, right->left);    // 左子树：右、 右子树：左
        bool isSame = outside && inside;                    // 左子树：中、 右子树：中 （逻辑处理）
        return isSame;
    }

    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        return compare(root->left, root->right);
    }
};
~~~

## 解法二[后序遍历迭代法--双栈]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if ((root->left != NULL && root->right == NULL) || (root->left == NULL && root->right != NULL)) return false;
        // 分别左右中以及右左中遍历
        if (root->left != NULL && root->right != NULL){
            stack<TreeNode*> left_stack;
            stack<TreeNode*> right_stack;
            left_stack.push(root->left);
            right_stack.push(root->right);
            while (!left_stack.empty() && !right_stack.empty()){
                TreeNode* left_cur = left_stack.top();
                TreeNode* right_cur = right_stack.top();
                if (left_cur->val != right_cur->val) return false;
                left_stack.pop();
                right_stack.pop();
                // 当左边栈顶元素的右结点与右边栈顶元素的左结点不空时分别入栈
                if (left_cur->right != NULL && right_cur->left != NULL) {
                    left_stack.push(left_cur->right);
                    right_stack.push(right_cur->left);
                }
                // 当左边栈顶元素的右结点与右边栈顶元素的左结点有一个为空则说明不对称
                if (left_cur->right != NULL && right_cur->left == NULL) return false;
                if (left_cur->right == NULL && right_cur->left != NULL) return false;
                // 当左边栈顶元素的左结点与右边栈顶元素的右结点不空时分别入栈
                if (left_cur->left != NULL && right_cur->right != NULL) {
                    left_stack.push(left_cur->left);
                    right_stack.push(right_cur->right);
                }
                // 当左边栈顶元素的左结点与右边栈顶元素的右结点有一个为空则说明不对称
                if (left_cur->left != NULL && right_cur->right == NULL) return false;
                if (left_cur->left == NULL && right_cur->right != NULL) return false;  
            }
            // 当有一个栈为空后,判断另外一个栈是否为空,不为空表示元素更多,不是对称二叉树
            if (!left_stack.empty() || !right_stack.empty()){
                return false;
            }
        }
        return true;
    }
};
~~~

## 解法三[后续遍历迭代法--单栈]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        stack<TreeNode*> result;
        result.push(root->left);
        result.push(root->right);
        while (!result.empty()){
            TreeNode* leftNode = result.top(); 
            result.pop();
            TreeNode* rightNode = result.top(); 
            result.pop();
            if (!leftNode && !rightNode) continue;
            if (!leftNode || !rightNode || (leftNode->val != rightNode->val)) return false;
            result.push(leftNode->left);   // 加入左节点左孩子
            result.push(rightNode->right); // 加入右节点右孩子
            result.push(leftNode->right);  // 加入左节点右孩子
            result.push(rightNode->left);  // 加入右节点左孩子
        }
        return true;
    }
};
~~~

## 解法四[后续遍历迭代法--单队列]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (root == NULL) return true;
        queue<TreeNode*> result;
        result.push(root->left);
        result.push(root->right);
        while (!result.empty()){
            TreeNode* leftNode = result.front(); 
            result.pop();
            TreeNode* rightNode = result.front(); 
            result.pop();
            if (!leftNode && !rightNode) continue;
            if (!leftNode || !rightNode || (leftNode->val != rightNode->val)) return false;
            result.push(leftNode->left);   // 加入左节点左孩子
            result.push(rightNode->right); // 加入右节点右孩子
            result.push(leftNode->right);  // 加入左节点右孩子
            result.push(rightNode->left);  // 加入右节点左孩子
        }
        return true;
    }
};
~~~

## 总结

* 我写的是迭代法的后序遍历,不过用的双栈,现在看来确实只用一个栈或者一个队列更好,可以同时比较栈顶前两个或者队列前两个结点.
* 总体来说主要还是考察的后序遍历的使用.

# 总结

赶进度中,今天的题有点多,方法也有点多,有一种做一题做了多题的感觉.

学习了层序遍历以及应用,复习了前序遍历,中序遍历以及后序遍历的迭代法以及递归法.
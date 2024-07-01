---
layout:     post   				    # 使用的布局（不需要改）
title:      代码随想录算法训练营第二十二天 				# 标题 
subtitle:   二叉树[Leetcode235二叉搜索树的最近公共祖先、Leetcode701二叉搜索树中的插入操作、Leetcode450删除二叉搜索树中的节点] #副标题
date:       2024-04-24 				# 时间
author:     ThreeStones1029 						# 作者
header-img: img/about_bg.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:	数据结构							#标签
---

[TOC]

# 第一题

[LeetCode235二叉搜索树中的最近公共祖先](https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)

## 解法一[看作普通二叉树使用递归回溯法]

* 空间复杂度O(n)

* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 解法一：正常二叉树的最近公共祖先
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) return NULL;
        if (root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left != NULL && right != NULL) return root;
        else if (left == NULL && right != NULL) return right;
        else if (left != NULL && right == NULL) return left;
        else return NULL;
    }
};
~~~

## 解法二[二叉搜索树的递归法]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 解法二：二叉搜索树的最近公共祖先
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        // 情况一
         if (root == NULL) return NULL;
         else if ((root->val > p->val && root->val < q->val) || (root->val < p->val && root->val > q->val)) return root;
         else if (root->val == p->val || root->val == q->val) return root;
         else if (root->val > p->val && root->val > q->val) return lowestCommonAncestor(root->left, p, q);
         else if (root->val < p->val && root->val < q->val) return lowestCommonAncestor(root->right, p, q);
         else return NULL;
    }
};
~~~

## 解法三[迭代]

* 空间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:    
	TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        while(root) {
            if (root->val > p->val && root->val > q->val) {
                root = root->left;
            } else if (root->val < p->val && root->val < q->val) {
                root = root->right;
            } else return root;
        }
        return NULL;
    }
};
~~~

## 总结

* 这题确实相比上一道求普通二叉树中的最近公共祖先简单很多。
* 主要还是利用它左右树结点值的特性来做。

# 第二题

[Leetcode701二叉搜索树中的插入操作](https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html)

## 解法一[递归法]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 递归法
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == NULL) {
            TreeNode* node = new TreeNode(val);
            return node;
        }
        if (root->val > val) root->left = insertIntoBST(root->left, val);
        if (root->val < val) root->right = insertIntoBST(root->right, val);
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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        while (cur) {
            pre = cur;
            if (val > cur->val) cur = cur->right;
            else cur = cur->left;
        }
        TreeNode* node = new TreeNode(val);
        if (pre == NULL) return node;
        if (pre->val > val) pre->left = node;
        else pre->right = node;
        return root;
    }
};
~~~

## 总结

* 这道题同样比较简单，特别是迭代法，主要就是找到最后一个节点，插入即可。

# 第三题

[Leetcode450删除二叉搜索树中的节点](https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html)

## 解法一[递归法（代码随想录）]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    // 代码随想录的迭代法
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == NULL) return NULL;
        if (root->val == key) {
            if (root->left == NULL && root->right == NULL) {
                delete root;
                return NULL;
            } 
            else if (root->left == NULL) {
                auto retNode = root->right;
                ///! 内存释放
                delete root;
                return retNode;
            }
            else if (root->right == NULL) {
                auto retNode = root->left;
                delete root;
                return retNode;
            }
            else {
                TreeNode* cur = root->right;
                while (cur->left) {
                    cur = cur->left;
                }
                cur->left = root->left;
                TreeNode* tmp = root;   // 把root节点保存一下，下面来删除
                root = root->right;     // 返回旧root的右孩子作为新root
                delete tmp;             // 释放节点内存
                return root;
            }  
        }
        if (root->val > key) root->left = deleteNode(root->left, key);
        if (root->val < key) root->right = deleteNode(root->right, key);
        return root;
    }
};
~~~

## 解法二[迭代法（自己的方法）]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        // 找到需要删除的节点
        TreeNode* dummy = new TreeNode(1000001); // 构造一个哑结点用来处理删除根节点的情况
        dummy->left = root;
        dummy->right = NULL;
        TreeNode* cur = dummy;
        TreeNode* pre = dummy;
        while (cur) {
            if (cur->val > key) {
                pre = cur;
                cur = cur->left;
            }
            else if (cur->val < key) {
                pre = cur;
                cur = cur->right;
            }
            else break;
        }
        if (cur == NULL) return dummy->left;
        // 以下为当删除的不是根结点时候的处理
        if (cur->left == NULL && cur->right == NULL) {
            if (pre->left != NULL && pre->left->val == key) pre->left = NULL;
            if (pre->right != NULL && pre->right->val == key) pre->right = NULL;
        } else if (cur->left == NULL && cur->right != NULL) {
            if (pre->left != NULL && pre->left->val == key) pre->left = cur->right;
            if (pre->right != NULL && pre->right->val == key) pre->right = cur->right;
        } else if (cur->left != NULL && cur->right == NULL) {
            if (pre->left != NULL && pre->left->val == key) pre->left = cur->left;
            if (pre->right != NULL && pre->right->val == key) pre->right = cur->left;
        } else {
            // 找到以key为根节点的右子树的最左下的结点
            TreeNode* delete_node = cur; // 记录将被删除的节点
            cur = cur->right; // 从右子树中找最左边的节点
            if (cur->left == NULL) { // 如果右子树中没有左边子树
                TreeNode* subright = cur->right;
                if (pre->left != NULL && pre->left->val == key) {
                    pre->left = cur;
                }
                if (pre->right != NULL && pre->right->val == key) {
                    pre->right = cur;
                }
                cur->left = delete_node->left;
                cur->right = subright;
            }
            else { // 如果右子树存在左边子树
                TreeNode* subpre = NULL; // 用于记录（被用来替换的节点）的父节点
                while (cur->left) {
                    subpre = cur;
                    cur = cur->left;
                }
                subpre->left = cur->right; // 将用于替换的结点的右子树给前面得到的subpre的左边子树
                if (pre->left != NULL && pre->left->val == key) {
                    pre->left = cur;
                }
                if (pre->right != NULL && pre->right->val == key) {
                    pre->right = cur;
                }
                cur->left = delete_node->left;
                cur->right = delete_node->right;
            }
            
        }
        return dummy->left;
    }
};
~~~

## 解法三[递归法（交换删除）]

* 时间复杂度O(n)
* 空间复杂度O(n)

~~~c++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) return root;
        if (root->val == key) {
            if (root->right == nullptr) { // 这里第二次操作目标值：最终删除的作用
                return root->left;
            }
            TreeNode *cur = root->right;
            while (cur->left) {
                cur = cur->left;
            }
            swap(root->val, cur->val); // 这里第一次操作目标值：交换目标值其右子树最左面节点。
        }
        root->left = deleteNode(root->left, key);
        root->right = deleteNode(root->right, key);
        return root;
    }
};

~~~

## 解法四[迭代法]

~~~c++
    TreeNode* deleteOneNode(TreeNode* target) {
        // if (target == NULL) return target;
        if (target->right == NULL) return target->left;
        TreeNode* cur = target->right;
        while (cur->left) {
            cur = cur->left;
        }
        cur->left = target->left;
        return target->right;
    }


    TreeNode* deleteNode(TreeNode* root, int key) {
        // 将目标节点（删除节点）的左子树放到 目标节点的右子树的最左面节点的左孩子位置上
        // 并返回目标节点右孩子为新的根节点
        if (root == NULL) return root;
        TreeNode* cur = root;
        TreeNode* pre = NULL; // 记录cur的父节点，用来删除cur
        while (cur) {
            if (cur->val == key) break;
            pre = cur;
            if (cur->val > key) cur = cur->left;
            else cur = cur->right;
        }
        if (pre == NULL) { // 如果搜索树只有头结点
            return deleteOneNode(cur);
        }
        // pre 要知道是删左孩子还是右孩子
        if (pre->left && pre->left->val == key) {
            pre->left = deleteOneNode(cur);
        }
        if (pre->right && pre->right->val == key) {
            pre->right = deleteOneNode(cur);
        }
        return root;
    }
~~~

## 总结

* 虽然这道题比较难，但好在独立做出来了，也花了三个小时才完全考虑清楚所有情况。
* 对于解法三确实很巧妙，主要在于不断的把要删除的节点通过交换移动到右子树的最左边的节点，直到不存在右子树删除。
* 我的方法也是找到要删除的节点，然后将右子树的最左边的节点放到删除的位置，同时根据右子树中是否有左子树分情况，对于有左子树则将右子树的最左边的节点移动到删除的地方，同时将移动节点的右子树变成被删除节点的右子树的左子树。如果右子树没有左子树，则直接将被删除节点的右子树的移动到被删除的地方，将被删除节点的左子树挂到新节点的左子树。
* 代码随想录的做法确实更简单也更容易理解，主要是将左边子树直接变成被删除结点的右子树中最左边节点的左子树。
* 同时对于删除根结点仿照链表定义了一个哑结点，这样可以不用单独处理删除根节点的情况。

# 总结

* 前两道题比较简单，主要是第三道题比较难，但好在都用自己的方法做出来了，所以印象还是比较深刻的。
* 对于第三道题确实代码随想录卡哥解法确实更简单。
---
title: lc235 Lowest Common Ancestor of a Binary Search Tree
date: 2016-10-30 19:32:56
tags: [lc-easy, tree]
categories: leetcode
---

因為是BST直接搜索p,q直到分叉或者碰到p/q，第一個開始分叉的節點，或先碰到的p/q是LCA。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        int m = min(p -> val, q -> val);
        int n = max(p -> val, q -> val);
        if (!root || (root->val > m && root->val < n) || root->val == m || root->val == n) return root;
        else if (root->val < m) return lowestCommonAncestor(root->right, p, q);
        else return lowestCommonAncestor(root->left, p, q);
    }
   
};
```
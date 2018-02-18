---
title: lc236 Lowest Common Ancestor of a Binary Tree
date: 2016-10-16 19:48:49
tags: [lc-medium, tree]
categories: leetcode
---
遞歸地在子樹里找p/q，如果在不同子樹那麼LCA是parent，否則返回在子樹遞歸的結果。

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
        if (!root || root == p || root == q) return root;
        TreeNode *l = lowestCommonAncestor(root -> left, p, q);
        TreeNode *r = lowestCommonAncestor(root -> right, p, q);
        if (l && r) return root;
        else if (!l) return r;
        else if (!r) return l;
        else return NULL;
    }
};
```
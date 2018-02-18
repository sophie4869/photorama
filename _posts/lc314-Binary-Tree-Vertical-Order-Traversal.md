---
title: lc314 Binary Tree Vertical Order Traversal
date: 2016-10-30 18:42:03
tags: [lc-medium, tree]
categories: leetcode
---

沒看清題直接寫，一開始忽略了top to bottom的順序。有了這個條件於是用queue來level-wise地訪問。

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
    map<int, vector<int>> mp;
public:
    vector<vector<int>> verticalOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;
        queue<pair<TreeNode*,int>> q;
        q.push(pair<TreeNode*,int>(root,0));
        while (!q.empty()) {
            pair<TreeNode*,int> cur = q.front();
            q.pop();
            mp[cur.second].push_back(cur.first->val);
            if (cur.first->left) q.push(pair<TreeNode*,int>(cur.first->left,cur.second-1));
            if (cur.first->right) q.push(pair<TreeNode*,int>(cur.first->right,cur.second+1));
        }
        for (auto i:mp) 
            res.push_back(i.second);
        return res;
    }
    
};
```

改進：保持hashmap按照key排序比較耗時，可以直接把節點放進結果的vector。因為二叉樹的從左到右的column編號是連續的，可以事先遍歷一遍找到最左和最右的節點的column編號，然後就可以把當前level放進vector的第level-left_most_column_number位置上。

```c++
class Solution {
    int miin = 0, maax = 0;
public:
    vector<vector<int>> verticalOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;
        traversal(root, 0);
        
        queue<pair<TreeNode*,int>> q;
        q.push({root,0});
        for (int i = miin; i <= maax; i++)
            res.push_back({});
        while (!q.empty()) {
            pair<TreeNode*,int> cur = q.front();
            q.pop();
            res[cur.second-miin].push_back(cur.first->val);
            if (cur.first->left) q.push({cur.first->left,cur.second-1});
            if (cur.first->right) q.push({cur.first->right,cur.second+1});
        }
        return res;
    }
    void traversal(TreeNode* r, int level) {
        if (level < miin) miin = level;
        if (level > maax) maax = level;
        if (r->left) traversal(r->left, level-1);
        if (r->right) traversal(r->right, level+1);
        
    }
};
```
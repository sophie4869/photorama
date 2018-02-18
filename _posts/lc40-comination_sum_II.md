---
title: lc40-combination sum II
date: 2016-11-17 16:36:01
tags: [lc-medium]
categories: leetcode
---

```c++
class Solution {
    vector<vector<int>> res;
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int> p;
        helper(p, target, candidates, 0);
        return res;
    }
    void helper(vector<int> &partial, int target, vector<int>& candidates, int start) {
        int l = candidates.size();
        if (!target) res.push_back(partial);
        int w;
        for (int i = start; i < l; i++) {
            if (w == candidates[i]) continue;
            w = candidates[i];
            if (w<=target)  {
                partial.push_back(w);
                helper(partial, target-w, candidates, i+1);
            }
            else break;
            partial.pop_back();
        }
    }
};
```
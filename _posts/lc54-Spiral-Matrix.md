---
title: lc54 Spiral Matrix
date: 2016-10-29 23:49:43
tags: [lc-medium, array]
categories: leetcode
---

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int m = matrix.size();
        vector<int> res;
        if (!m) return res;
        int n = matrix[0].size();
        
        int r = n, c = m-1;
        int i = 0, j = -1;
        while (r>0 || c>0) {
            int t = r--;
            if (t == 0) break;
            while(t-->0) {res.push_back(matrix[i][++j]);}
            t = c--;
            if (t == 0) break;
            while(t-->0) {res.push_back(matrix[++i][j]);}
            t = r--;
            if (t == 0) break;
            while(t-->0) {res.push_back(matrix[i][--j]);}
            t = c--;
            if (t == 0) break;
            while(t-->0) {res.push_back(matrix[--i][j]);}
        }
        return res;
    }
};
```
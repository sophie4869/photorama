---
title: lc120-Triangle
date: 2016-9-18 20:32:24
tags: [lc-medium, dynamic-programming, array]
categories: leetcode
---

```c++
class Solution {
private:
    vector<vector<int>> dp;
    int bound;
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        bound = triangle.size();
        for (int i = 0; i < bound; i++) {
            vector<int> tmp(i+1,INT_MIN);
            dp.push_back(tmp);
        }
        return solve(triangle, 0, 0);
    }
    int solve(vector<vector<int>> &triangle, int i, int j) {
        if (dp[i][j] !=INT_MIN) return dp[i][j];
        else if (i >= bound-1)
            dp[i][j] = triangle[i][j];
        else
            dp[i][j] = triangle[i][j]+min(solve(triangle, i+1, j), solve(triangle, i+1, j+1));
        return dp[i][j];
    }
};
```
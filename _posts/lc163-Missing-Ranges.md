---
title: lc163 Missing Ranges
date: 2016-10-17 19:55:25
tags: [lc-medium, array]
categories: leetcode
---

```c++
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        vector<string> res;
        int prev = INT_MAX;
        int lb = INT_MAX, ub;
        nums.insert(nums.begin(),lower-1);
        nums.push_back(upper+1);
        for (auto i : nums) {
            if (prev != INT_MAX) {
                if (i - prev == 2) res.push_back(to_string(prev+1));
                stringstream ss;
                ss<<prev+1<<"->"<<i-1;
                if (i - prev > 2 || i - prev < 0) res.push_back(ss.str());
            }
            prev = i;
        }
        return res;
    }
};
```
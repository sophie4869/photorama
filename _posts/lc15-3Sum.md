---
title: lc15 3Sum
date: 2016-10-18 19:53:08
tags: [lc-medium, array]
categories: leetcode
---

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        int l = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = 0; i < l; i++) {
            if (i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            int a = nums[i];
            int j = i+1;
            int k = l-1;
            while (j < k) {
                // cout << nums[j] << ' ' << nums[k] << ' '<< -a << endl;
                int sum = nums[j]+nums[k];
                if (sum < 0-a) {
                    j++;
                    while (j > 0 && nums[j] == nums[j-1]) j++;
                }
                else if(sum > 0-a) {
                    k--;
                    while (k < l-1 && nums[k] == nums[k+1]) k--;
                }
                else {
                    vector<int> res;
                    res.push_back(nums[i]);
                    res.push_back(nums[j]);
                    res.push_back(nums[k]);
                    result.push_back(res);
                    j++;
                    while (j > 0 && nums[j] == nums[j-1]) j++;
                    k--;
                    while (k < l-1 && nums[k] == nums[k+1]) k--;
                }
            }
        }
        sort(result.begin(), result.end());
        result.erase( unique( result.begin(), result.end() ), result.end() );
        
        return result;
    }
};
```
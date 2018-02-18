---
title: lc1-Two Sum
date: 2016-10-15 20:14:18
tags: [lc-easy, array, hash-table]
categories: leetcode
---

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> rem;
        vector<int> res;
        for (int i = 0; i < nums.size(); i++) {
            if (rem.find(nums[i])!=rem.end()) {
                res.push_back(rem[nums[i]]);
                res.push_back(i);
            }
            else {
                rem[target-nums[i]] = i;
            }
        }
        return res;
    }
};
```

version of 08/30/2015

```c++

#include <iostream>
#include <vector>
#include <map>
using namespace std;

class Solution {

public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res;
        map<int, int>test;
        for(int i=0;i<nums.size();i++){
            int t=nums[i];
            map<int,int>::iterator it = test.find(t);
            if(it!=test.end()){
                
//                if(test[j]==t){
                res.push_back(it->second+1);
                    res.push_back(i+1);
  //              }
            }
            test[target-nums[i]]=i;
        }
        return res;
    }
    
};
```
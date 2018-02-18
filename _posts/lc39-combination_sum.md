---
title: lc39-Combination Sum
date: 2016-1-30 20:52:35
tags: [lc-medium, array, backtracking]
categories: leetcode
---


```c++
class Solution {
    vector<vector<int>> res;
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int> p;
        helper(p, target, candidates, 0);
        return res;
    }
    void helper(vector<int> &partial, int target, vector<int>& candidates, int start) {
        int l = candidates.size();
        if (!target) res.push_back(partial);
        for (int i = start; i < l; i++) {
            int w = candidates[i];
            if (w<=target)  {
                partial.push_back(w);
                helper(partial, target-w, candidates, i);
            }
            else break;
            partial.pop_back();
        }
    }
};
```


```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        d = {}
        for i in range(target+1):
            d[i] = []

        for i in range(len(candidates)):
            d[candidates[i]] = [[candidates[i]]]
        for i in range(target+1):
            for j in range(len(candidates)):
                if i-candidates[j] in d and d[i-candidates[j]] != []:
                    for x in d[i-candidates[j]]:
                        a = sorted(x + [candidates[j]])
                        if a not in d[i]:
                            d[i].append(a)
        return d[target]
```

```c++
class Solution {
struct cmp{bool operator()(const int &x,const int &y) const{return x<y;}};
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<vector<int>>> d;
        for(int i = 0; i <= target; i++){
            vector<vector<int>> tmp;
            d.push_back(tmp);
        }
        for(int i = 0; i < candidates.size(); i++){
            vector<int> tmp;
            tmp.push_back(candidates[i]);
            if(candidates[i]<=target)
                d[candidates[i]].push_back(tmp);
        }
        for(int i = 0; i <= target; i++){
            for(int j = 0; j < candidates.size(); j++){
                if(i-candidates[j]<0) continue;
                if(!d[i-candidates[j]].empty()){
                    for(int k = 0; k < d[i-candidates[j]].size(); k++){
                        vector<int> t;
                        
                        t = d[i-candidates[j]][k];
                        t.push_back(candidates[j]);
                        sort(t.begin(), t.end(), cmp());
                        int l;
                        for(l = 0; l < d[i].size(); l++){
                            if(d[i][l]==t) break;
                        }
                        if(l == d[i].size()){
                            d[i].push_back(t);
                        }
                    }
                }
            }
        }
        return d[target];
    }
};
```
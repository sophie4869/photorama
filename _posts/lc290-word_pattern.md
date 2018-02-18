---
title: lc290-Word Pattern
date: 2016-11-16 23:20:37
tags: [lc-easy, hashtable, string]
categories: leetcode
---

学习一个如何split string。

```c++
class Solution {
    unordered_map<char, string> mp;
    set<string> words;
public:
    bool wordPattern(string pattern, string str) {
        int l = pattern.length();
        stringstream ss;
        ss.str(str);
        string item;
        vector<string> v;
        while (getline(ss, item, ' ')) {
            v.push_back(item);
        }
        if (l!=v.size()) return false;
        for (int i = 0; i < l; i++) {
            if (!mp.count(pattern[i]))  {
                if (words.find(v[i])!=words.end()) return false;
                words.insert(v[i]);
                mp[pattern[i]] = v[i];
                
            }
            else if (v[i]!=mp[pattern[i]]) return false;
        }
        return true;
    }
};
```

或者：

```c++
        stringstream ss(str);
        string item;
        vector<string> v;
        while (ss>>item) {
            v.push_back(item);
        }
```
---
title: lc291-Word Pattern II
date: 2016-11-14 09:18:38
tags: [backtracking, dropbox, lc-hard, locked]
categories: leetcode
---

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty substring in str.

Examples:

```
pattern = "abab", str = "redblueredblue" should return true.
pattern = "aaaa", str = "asdasdasdasd" should return true.
pattern = "aabb", str = "xyzabcxzyabc" should return false.
```
Notes:
You may assume both pattern and str contains only lowercase letters.

```c++
class Solution {
    unordered_map<char, string> mp;
    set<string> visited;
public:
    bool wordPatternMatch(string pattern, string str) {
        if (!pattern.length() && !str.length()) return true;
        if (!pattern.length() || !str.length()) return false;
        char p = pattern[0];
        if (mp.find(p) != mp.end()) {
            if (str.substr(0, mp[p].length()) != mp[p]) return false;
            return wordPatternMatch(pattern.substr(1), 
            	str.substr(mp[p].length()));
        }
        else {
            for (int i = 1; i <= str.length(); i++) {
                string s = str.substr(0, i);
                // cout << s;
                if (visited.find(s)!=visited.end()) continue;
                visited.insert(s);
                mp[p] = s;
                if (wordPatternMatch(pattern.substr(1), 
                	str.substr(mp[p].length())))  
                		return true;
                visited.erase(s);
                mp.erase(p);
            }
            return false;
        }
    }
};
```
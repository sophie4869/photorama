---
title: lc171 Excel Sheet Column Number
date: 2016-10-30 10:07:53
tags: [lc-easy]
categories: leetcode
---

沒什麼好說的..

```c++
class Solution {
public:
    int titleToNumber(string s) {
        int res = 0;
        int l = s.length();
        for (int i = 0; i < l; i++) {
            res *= 26;
            res += s[i]-'A'+1;
        }
        return res;
    }
};
```
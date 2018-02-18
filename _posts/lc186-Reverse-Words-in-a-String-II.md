---
title: lc186 Reverse Words in a String II
date: 2016-10-29 16:34:26
tags: [lc-easy, string]
categories: leetcode
---
in place:

```c++
class Solution {
public:
    void reverseWords(string &s) {
        auto p1 = s.begin();
        for (auto p2 = s.begin(); p2 != s.end()+1; p2++) {
            if (*p2 == ' ' || p2 == s.end()) {
                reverse(p1, p2);
                p1 = p2+1;
            }
        }
        reverse(s.begin(), s.end());
    }
};
```

not in place:

```c++
class Solution {
public:
    void reverseWords(string &s) {
        int l = s.length();
        // split string s;
        vector<string> v;
        int p1 = 0;
        int p2 = s.find(" ");
        while (p2 != string::npos) {
            v.push_back(s.substr(p1, p2-p1));
            p1 = p2+1;
            p2 = s.find(" ", p1);
        }
        if (p1 < l) v.push_back(s.substr(p1));
        stringstream ss;
        for (int i = v.size()-1; i>=0; i--) {
            ss << v[i] ;
            if (i > 0) ss<< " ";
        }
        s = ss.str();
        
    }
};
```
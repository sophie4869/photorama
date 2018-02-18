---
title: lc394 Decode String
date: 2016-10-17 19:56:58
tags: [lc-medium, DFS]
categories: leetcode
---

```c++
class Solution {
public:
    string decodeString(string s) {
        stack<string> stk;
        int cur_num = 0;
        string cur_str;
        // string res_str="";
        for(int i = 0; i < s.length(); i++) {
            if (s[i] >= '0' && s[i] <= '9'){
                cur_num *= 10;
                cur_num += s[i]-'0';
            }
            else if (s[i] == '[') {
                stk.push(to_string(cur_num));
                cur_num = 0;
                stk.push("[");
            }
            else if (s[i] >= 'a' && s[i] <= 'z') {
                stk.push(string(1,s[i]));
            }
            else { //s[i] = ']'
                string tmp;
                while(stk.top() != "[") {tmp.insert(0,stk.top()); stk.pop();}
                stk.pop();
                int rep = stoi(stk.top());
                stk.pop();
                for (int i = 0; i < rep; i++)
                    cur_str = cur_str + tmp;
                stk.push(cur_str);
                cur_str = "";
            }
        }
        string res;
        while (!stk.empty()) {res.insert(0, stk.top()); stk.pop();}
        return res;
    }
};
```
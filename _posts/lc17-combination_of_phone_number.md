---
title: lc17-Letter Combinations of a Phone Number
date: 2016-11-16 21:46:04
tags: [lc-medium, backtracking, string]
categories: leetcode
---

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

![](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.


```c++
class Solution {
    vector<string> dic = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    vector<string> res;
public:
    vector<string> letterCombinations(string digits) {
        string s = "";
        if (!digits.length()) return res;
        backtracking(digits, s);
        return res;
    }
    
    void backtracking(string digits, string s) {
        int len = digits.length();
        if (!len) {res.push_back(s); return;}
        string str = dic[digits[0]-'2'];
        for (int i = 0; i < str.length(); i++) {
            string tmp = s + str[i];
            backtracking(digits.substr(1), tmp);
        }
    }
};
```
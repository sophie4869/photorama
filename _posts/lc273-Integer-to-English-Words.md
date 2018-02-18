---
title: lc273 Integer to English Words
date: 2016-10-30 08:30:37
tags: [lc-hard, string]
categories: leetcode
---

這道題竟然是hard..
邏輯很簡單，但是edge case和各種容易犯錯的小細節非常多。

- thousand/million/billion在0後面不加
- 空格的問題
- ten和eleven~nineteen的區分
- zero在不同chunk

```c++
class Solution {
    vector<string> lessThan20 = {"", "One ", "Two ", "Three ", "Four ", "Five ", "Six ", "Seven ", "Eight ", "Nine ", "Ten ", "Eleven ", "Twelve ", "Thirteen ", "Fourteen ", "Fifteen ", "Sixteen ", "Seventeen ", "Eighteen ", "Nineteen "};
    vector<string> tens = {"", "", "Twenty ", "Thirty ", "Forty ", "Fifty ", "Sixty ", "Seventy ", "Eighty ", "Ninety "};
    vector<string> thousands = {"", "Thousand ","Million ","Billion "};
public:
    string convertChunk(int num) {
        int n = num;
        string s="";
        if (num/100) s+= lessThan20[num/100] + "Hundred ";
        n%=100;
        if (n < 20) s+=lessThan20[n];
        else s+=tens[n/10] +lessThan20[n%10];
        return s;
    }
    string numberToWords(int num) {
        if (!num) return "Zero";
        string s = "";
        int cnt = 0;
        while (num) {
            string tmp = convertChunk(num%1000);
            if (tmp != "") s= tmp+thousands[cnt]+s;
            else s= tmp+s;
            cnt++;
            num/=1000;
        }
        return s.substr(0,s.length()-1);
    }
};
```

```c++
class Solution {
    vector<string> tens {"", "", "Twenty", "Thirty", "Fourty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    vector<string> lessThan20 {"Zero", "One ", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", 
        "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"
    };
public:
    string numberToWords(int num) {
        int x = num;
        stringstream ss;
        while (x) {
            ss << convertChunk(x%1000);
            x /= 1000;
        }
        return ss.str();
    }
    string convertChunk(int num) {
        // space?
        stringstream ss;
        int h = num/100;
        if (h != 0)
            ss << lessThan20[h] << " hundred";
        int t = num%100;
        if (t != 0) {
            if (h!=0) ss << " ";
            if (t >= 20) {
            ss << tens[t/10];
            int l = num % 10;
            if (l != 0)
                ss << " " << lessThan20[l];
            else ss << lessThan20[t];
        }
        
        return ss.str();
    }
};
```
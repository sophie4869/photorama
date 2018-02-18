---
title: lc140-Word Break II
date: 2016-11-17 07:51:23
tags: [lc-hard, backtracking, string, dynamic-programming]
categories: leetcode
---

一开始是Trie+backtracking，会TLE。

```c++
class TrieNode {
    public:
    vector<TrieNode*> children;
    bool leaf = false;
    TrieNode():children(26, nullptr) {}
};
class Solution {
    vector<string> res;
    unordered_set<string> wordDict;
public:
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        TrieNode* root = buildTrie(wordDict);
        this->wordDict = wordDict;
        string str = "";
        backtracking(str, s, root);
        return res;
    }
    
    TrieNode* buildTrie(unordered_set<string> words) {
        TrieNode* root = new TrieNode();
        TrieNode* t = root;
        for (auto word : words) {
            int l = word.length();
            for (int i = 0; i < l; i++) {
                if (!t -> children[word[i]-'a'])
                    t -> children[word[i]-'a'] = new TrieNode();
                t = t -> children[word[i]-'a'];
            }
            t -> leaf = true;
            t = root;
        }
        return root;
    }
    
    void backtracking(string partial, string s, TrieNode* r) {
        if (!s.length()) {
            res.push_back(partial); 
            return;
        }
        TrieNode* node = r;
        int l = s.length();
        for (int i = 0; i < l; i++) {
            if (node->children[s[i]-'a'])   node = node->children[s[i]-'a'];
            else return;
            if (node->leaf) {
                string tmp = partial+(partial.length()?" ":"")+s.substr(0, i+1);
                backtracking(tmp, s.substr(i+1), r);
            }
        }
    }
};
```

于是加上memorization，把临时的结果储存起来。不过这里backtracking也得修改成有返回值的函数，res也变成局部变量，不等到搜索到字符串结尾再push_back，而是返回局部解。这算是DP了吧。beats 71.48%, 还没想到哪里可以优化。

```c++
class TrieNode {
    public:
    vector<TrieNode*> children;
    bool leaf = false;
    TrieNode():children(26, nullptr) {}
};
class Solution {
    unordered_map<string, vector<string>> mp;
    unordered_set<string> wordDict;
public:
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        TrieNode* root = buildTrie(wordDict);
        this->wordDict = wordDict;
        vector<string> res = backtracking(s, root);
        return res;
    }
    
    TrieNode* buildTrie(unordered_set<string> words) {
        TrieNode* root = new TrieNode();
        TrieNode* t = root;
        for (auto word : words) {
            int l = word.length();
            for (int i = 0; i < l; i++) {
                if (!t -> children[word[i]-'a'])
                    t -> children[word[i]-'a'] = new TrieNode();
                t = t -> children[word[i]-'a'];
            }
            t -> leaf = true;
            t = root;
        }
        return root;
    }
    
    vector<string> backtracking(string s, TrieNode* r) {
        if (mp.find(s)!=mp.end())   return mp[s];
        vector<string> res;
        if (!s.length()) {
            res.push_back(""); 
            mp[s] = res;
            return res;
        }
        TrieNode* node = r;
        int l = s.length();
        for (int i = 0; i < l; i++) {
            if (node->children[s[i]-'a'])   node = node->children[s[i]-'a'];
            else {mp[s]=res; return res;}
            if (node->leaf) {
                vector<string> ret = backtracking(s.substr(i+1), r);
                for (auto ret_str: ret) {
                    string tmp = s.substr(0, i+1)+(ret_str.length()?" ":"")+ret_str;
                    res.push_back(tmp);
                }
            }
        }
        mp[s] = res;
        return res;
    }
};
```



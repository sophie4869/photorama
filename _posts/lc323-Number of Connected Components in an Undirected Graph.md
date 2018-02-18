---
title: lc323 Number of Connected Components in an Undirected Graph
date: 2016-10-17 19:58:41
tags: [lc-medium, graph]
categories: leetcode
---

```c++
class Solution {
private:
    vector<int> head;
public:
    int countComponents(int n, vector<pair<int, int>>& edges) {
        for (int i = 0; i < n; i++){
            head.push_back(-1);
        }
        for (auto e : edges) {
            int h1 = e.first, h2 = e.second;
            while (head[h1] != -1) h1 = head[h1];
            while (head[h2] != -1) h2 = head[h2];
            if (h1 != h2) head[h2] = h1;
        }
        int cnt = 0;
        for (auto i : head) {
            if (i == -1) cnt ++;
        }
        return cnt;
    }
};
```
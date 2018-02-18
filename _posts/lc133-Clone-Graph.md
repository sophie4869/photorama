---
title: lc133 Clone Graph
date: 2016-10-30 12:55:26
tags: [lc-medium, graph, DFS, BFS]
categories: leetcode
---

和deep copy linked list一樣的遞歸辦法(DFS)：

```c++
/**
 * Definition for undirected graph.
 * struct UndirectedGraphNode {
 *     int label;
 *     vector<UndirectedGraphNode *> neighbors;
 *     UndirectedGraphNode(int x) : label(x) {};
 * };
 */
class Solution {
    map<UndirectedGraphNode*, UndirectedGraphNode*> mp;
public:
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if (!node) return nullptr;
        if (mp[node] != nullptr) return mp[node];
        mp[node] = new UndirectedGraphNode(node->label);
        for (auto n:node->neighbors) {
            mp[node]->neighbors.push_back(cloneGraph(n));
        }
        return mp[node];
    }
};
```

Iterative version:

```c++
class Solution {
    map<int, UndirectedGraphNode*> mp;
public:
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if (!node) return nullptr;
        stack<UndirectedGraphNode*> stk;
        stk.push(node);
        UndirectedGraphNode* newHead = new UndirectedGraphNode(node->label);
        mp[node->label] = newHead;
        while (!stk.empty()) {
            UndirectedGraphNode* cur = stk.top();
            stk.pop();
            for (auto i:cur->neighbors) {
                if (mp.find(i->label) == mp.end()) {
                    mp[i->label] = new UndirectedGraphNode(i->label);
                    stk.push(i);
                }
                mp[cur->label]->neighbors.push_back(mp[i->label]);
            }
        }
        return newHead;
    }
};
```

BFS寫的時候一開始TLE，沒有注意到圖裡面有self-cycle，這種node不能push到queue里。

```c++
class Solution {
    unordered_map<int, UndirectedGraphNode*> mp;
public:
    UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
        if (node == nullptr) return nullptr;
        queue<UndirectedGraphNode*> q;
        q.push(node);
        UndirectedGraphNode *newHead = new UndirectedGraphNode(node->label);
        mp[node->label] = newHead;
        while (!q.empty()) {
            UndirectedGraphNode* tmp = q.front();
            q.pop();
            for (auto i:tmp->neighbors) {
                if (mp.find(i->label) == mp.end()) {
                    mp[i->label] = new UndirectedGraphNode(i->label);
                    q.push(i);
                }
                mp[tmp->label]->neighbors.push_back(mp[i->label]);
            }
        }
        return newHead;
    }
};

```
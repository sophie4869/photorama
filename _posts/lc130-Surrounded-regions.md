---
title: lc130 Surrounded Regions
date: 2016-10-29 11:19:36
tags: [lc-medium, union-find]
categories: leetcode
---
link: https://leetcode.com/problems/surrounded-regions/

Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

偷看了一下discussion仿照寫的，練習了并查集，基本思路是把所有的'O'連接起來，然後在boundary上的跟一個dummy node union，最後遍歷一遍找和dummy node不相連的所有的'O'；另外一開始我是用map來實現head/rk兩個數組，因為想讓index儲存坐標pair<x, y>，但這樣讓運行時間直接翻了上百倍…
my solution:

```c++
class Solution {
    int* head;
    int* rk;
    int m;
public:
    void MakeSet(int i) {
        head[i] = i;
        rk[i] = 1;
    }
    int place_t(int r, int c) {
        return r * m + c;
    }
    int Find(int i) {
        int tmp = i;
        while (head[tmp] != tmp) { //find root
            tmp = head[tmp];
        }
        int t = i;
        while (t != tmp) {
            head[t] = tmp;
            t = head[t];
        }
        return tmp;
    }
    void Union(int x, int y) {
        int rk_x, rk_y;
        int root_x = Find(x);
        int root_y = Find(y);
        if (root_x != root_y) {
            rk_x = rk[root_x];
            rk_y = rk[root_y];
            if (rk_x == rk_y) {
                head[root_y] = root_x;
                rk[root_x] += 1;
            }
            else if (rk_x > rk_y)   head[root_y] = root_x;
            else head[root_x] = root_y; 
        }
    }
    void solve(vector<vector<char>>& board) {
        int row_l;
        if (!board.empty()) row_l = board[0].size();
        else return;
        m = row_l;
        int col_l = board.size();
        head = new int[row_l*col_l+1];
        rk = new int[row_l*col_l+1];
        MakeSet(place_t(col_l-1, row_l));
        for (int i = 0; i < col_l; i++) {
            for (int j = 0; j < row_l; j++) {
                int cur = place_t(i,j);
                MakeSet(cur);
                if ((i == 0 || j == 0 || i == col_l-1 || j == row_l-1) && board[i][j] == 'O') {
                    Union(cur, place_t(col_l-1, row_l));
                }
                if (i > 0 && board[i][j] == 'O' && board[i-1][j] == 'O') Union(cur, place_t(i - 1, j));
                if (j > 0 && board[i][j] == 'O'&& board[i][j-1] == 'O') Union(cur, place_t(i, j - 1));
            }
        }
        int dum_place = Find(place_t(col_l-1, row_l));
        for (int i = 0; i < col_l; i++) {
            for (int j = 0; j < row_l; j++) {
                if (board[i][j] == 'O') {
                    int cur = Find(place_t(i,j));
                    if (cur != dum_place) board[i][j] = 'X';
                }
            }
        }
        
        delete[] head;
        delete[] rk;
    }
}; 
```

還可以直接floodfill搜索（相當於DFS），注意遞歸版本會爆棧，手動寫個stack就可以。

```c++
class Solution {
    int m;
    int n;
public:
    void solve(vector<vector<char>>& board) {
        m = board.size();
        if (!m) return;
        n = board[0].size();
        vector<vector<int>> mark(m, vector<int>(n,-1)); //-1 unvisited 0 visited 1 marked
        
        for (int j = 0; j < n; j++) {
            search(0,j,board,mark);
        }
        for (int j = 0; j < n; j++) {
            search(m-1,j,board,mark);
        }
        for (int j = 0; j < m; j++) {
            search(j,0,board,mark);
        }
        for (int j = 0; j < m; j++) {
            search(j,n-1,board,mark);
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mark[i][j] != 1 && board[i][j] == 'O')  board[i][j] = 'X';
            }
        }
    }
    
    void search(int i, int j,  vector<vector<char>>&board,  vector<vector<int>>&mark) {
        stack<pair<int,int>> stk;
        stk.push(pair<int,int>(i,j));
        while (!stk.empty()) {
            pair<int,int> cur = stk.top();
            int x = cur.first;
            int y = cur.second;
            stk.pop();
            if (board[x][y] == 'O') mark[x][y] = 1;
            else {mark[x][y] = 0; continue;}
            
            if (x > 0 && board[x-1][y]=='O' && mark[x-1][y]==-1) stk.push(pair<int,int>(x-1,y));
            if (x < m-1 && board[x+1][y]=='O' && mark[x+1][y]==-1) stk.push(pair<int,int>(x+1, y));
            if (y > 0 && board[x][y-1]=='O' && mark[x][y-1]==-1) stk.push(pair<int,int>(x,y-1));
            if (y < n-1 && board[x][y+1]=='O' && mark[x][y+1]==-1) stk.push(pair<int,int>(x,y+1));
        }
        return;
    }
};
```

---
updated Dec 13:

BFS:

```
class Solution {
    int m;
    int n;
public:
    void solve(vector<vector<char>>& board) {
        queue<pair<int, int>> q;
        m = board.size();
        if (!m) return;
        n = board[0].size();
        vector<vector<int>> marked(m, vector<int>(n,0));
        for (int i = 1; i < m-1; i++) {
            if (board[i][0] == 'O')
                q.push(make_pair(i,0));
            if (board[i][n-1] == 'O')
                q.push(make_pair(i,n-1));
        }
        for (int j = 0; j < n; j++) {
            if (board[0][j] == 'O')
                q.push(make_pair(0,j));
            if (board[m-1][j] == 'O')
                q.push(make_pair(m-1,j));
        }
        
        while (!q.empty()) {
            pair<int,int> p = q.front();
            q.pop();
            int i = p.first;
            int j = p.second;
            marked[i][j] = 1;
            if (i > 0 && board[i-1][j] == 'O' && !marked[i-1][j]) { q.push(make_pair(i-1,j));}
            if (i < m-1 && board[i+1][j] == 'O' && !marked[i+1][j]) { q.push(make_pair(i+1,j));}
            if (j > 0 && board[i][j-1] == 'O' && !marked[i][j-1]) { q.push(make_pair(i,j-1));}
            if (j < n-1 && board[i][j+1] == 'O' && !marked[i][j+1]) { q.push(make_pair(i,j+1));}
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++)
                if (board[i][j] == 'O' && !marked[i][j])    board[i][j] = 'X';
        }
    }
};
```
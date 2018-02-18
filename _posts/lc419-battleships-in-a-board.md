---
title: lc419 battleships in a board
date: 2016-10-29 15:40:44
tags: [lc-medium]
categories: leetcode
---

這題其實超簡單，但我一開始沒看清題，以為invalid的輸入也是會出現的。

```c++
class Solution {
    int m;
    int n;
    
public:
    int countBattleships(vector<vector<char>>& board) {
        n = board.size();
        if (!n) return 0;
        m = board[0].size();
        int count = 0;
        for(int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (board[i][j] == 'X') {
                    if ((i == 0 || board[i-1][j] == '.') && (j == 0 || board[i][j-1] == '.'))
                        count++;
                }
            }
        }
        return count;
    }
};
```

所以我最開始的版本用到了并查集，type用來標記不同union的類別（horizontal/vertical/invalid或者undefined）。在union的時候檢查和更新類別。其實和輸入只有valid的情況類似：輸入有invalid時，cnt++僅在左邊和上面的鄰居都是undefined；後者則需要都是'.'。差別是為了排除invalid，要union后記錄type。

```c++
class UF {
private:
    vector<int> head;
    vector<int> rk;
    vector<int> type; // 1 h 2 v 0 invalid -1 undefined
    int cnt = 0;
public:
    UF(int N):rk(N,-1),type(N,-1),head(N,-1) {}
    int getCnt() {return cnt;}
    void MakeSet(int i) {
        rk[i] = 1;
        head[i] = i;
        cnt++;
    }
    
    int Find(int x) {
        int i = x;
        while(head[i] != i) {
            i = head[i];
        }
        int j = x;
        while(j != i) {
            head[j] = i;
            j = head[j];
        }
        return i;
    }
    void Union(int x, int y, int t) {
        int rx = Find(x);
        int ry = Find(y);
        if (rk[rx] == rk[ry]) {
            head[ry] = rx;
            rk[rx]+=rk[ry];
        }
        else if (rk[rx] > rk[ry]) {head[ry] = rx; rk[ry]+=rk[rx];}
        else {head[rx] = ry; rk[rx]+=rk[ry];}
        
        if (type[rx] == -1 || type[rx] == t) {type[rx] = t; type[ry] = t; cnt-=1;}
        else if (type[rx] != t) {type[rx] = 0; type[ry] = 0; cnt-=1;}
    }
};

class Solution {
    int m;
    int n;
    
public:
    int countBattleships(vector<vector<char>>& board) {
        n = board.size();
        if (!n) return 0;
        m = board[0].size();
        UF u = UF(m*n);
        for(int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (board[i][j] == 'X') {
                    u.MakeSet(i*m+j);
                    if (i > 0 && board[i-1][j] == 'X') {
                        u.Union((i-1)*m+j, i*m+j, 2);
                    }
                    if (j > 0 && board[i][j-1] == 'X') {
                        u.Union(i*m+j-1, i*m+j, 1);
                    }
                }
            }
        }
        return u.getCnt();
        
    }
};
```
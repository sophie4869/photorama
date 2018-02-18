---
title: lc289-Game of Life
date: 2016-11-16 21:12:56
tags: [array, lc-medium]
categories: leetcode
---

O(1) space solution: 一个很好的思路，int有四个字节，只用了一位，可以用第二位来表示下一个status。注意count neighbor的时候会否把自己算进去。

```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size();
        if (m) {
            int n = board[0].size();
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    int count = -board[i][j];
                    for (int ii = max(i-1, 0); ii <= min(i+1, m-1); ii++) {
                        for (int jj = max(j-1, 0); jj <= min(j+1, n-1); jj++) {
                            count += board[ii][jj] & 1;
                        }
                    }
                    if (count == 3 || count + board[i][j] == 3) board[i][j] |= 2;
                }
            }
            
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) 
                    board[i][j] >>= 1;
            }
        }
    }
};
```
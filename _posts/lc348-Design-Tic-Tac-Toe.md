---
title: lc348 Design Tic-Tac-Toe
date: 2016-10-29 22:23:37
tags: [lc-medium, design]
categories: leetcode
---
O(1) time for each move, O(n) space.

```c++
class TicTacToe {
private:
    int n;
    vector<vector<int>> state;
    vector<vector<int>> cnt;
    
public:
    /** Initialize your data structure here. */
    TicTacToe(int n):state({vector<int>(n, 0), vector<int>(n, 0), vector<int>(2, 0)}), cnt({vector<int>(n, 0), vector<int>(n, 0), vector<int>(2, 0)}) {
        this->n = n;
    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    int move(int row, int col, int player) {
        if (state[0][row] < 6) {
            if (++cnt[0][row] == n) {state[0][row] |= 1;}
            state[0][row] |= 1 << player;
            if (state[0][row] == 3 || state[0][row] == 5) return player;
        }
        if (state[1][col] < 6) {
            if (++cnt[1][col] == n) {state[1][col] |= 1;}
            state[1][col] |= 1 << player;
            if (state[1][col] == 3 || state[1][col] == 5) return player;
        }
        
        if (row == col && state[2][0] < 6) {
            if (++cnt[2][0] == n) state[2][0] |= 1; 
            state[2][0] |= 1 << player; 
            if (state[2][0] == 3 || state[2][0] == 5) return player;
        }
        if (n-1-row == col && state[2][1] < 6) {
            if (++cnt[2][1] == n) state[2][1] |= 1; 
            state[2][1] |= 1 << player; 
            if (state[2][1] == 3 || state[2][1] == 5) return player;
        }
        return 0;
    }
};

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */ 
 
 ```
 
 
 偷看discussion有人用+-1來表示state，絕對值==size的時候代表row/column/diagonal全是player 1/2。這樣代碼簡短很多，時間複雜度一樣。
 
 ```c++
 class TicTacToe {
private:
    int n;
    vector<vector<int>> cnt;
    
public:
    /** Initialize your data structure here. */
    TicTacToe(int n):cnt({vector<int>(n, 0), vector<int>(n, 0), vector<int>(2, 0)}) {
        this->n = n;
    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    int move(int row, int col, int player) { 
        cnt[0][row] += (player == 1? 1 : -1);
        cnt[1][col] += (player == 1? 1 : -1);
        if (row == col) cnt[2][0] += (player == 1? 1 : -1);
        if (n-1-row == col) cnt[2][1] += (player == 1? 1 : -1);
        if (abs(cnt[0][row]) == n || abs(cnt[1][col]) == n || abs(cnt[2][0]) == n || abs(cnt[2][1]) == n) return player;
        return 0;
    }
};

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
 ```
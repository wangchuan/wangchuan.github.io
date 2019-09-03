---
layout: post
title: "LeetCode and LintCode Notes - Starred"
categories: hidden
tags: special
use_math: true
---

* TOC
{:toc}

### <em class="icon-check"></em> Surrounded Regions
Idea: This problem cannot use simple DFS, because there is case as below, which will cause stack-overflow.
```cpp
OOOOOOOOOO
XXXXXXXXXO
OOOOOOOOOO
OXXXXXXXXX
OOOOOOOOOO
XXXXXXXXXO
OOOOOOOOOO
OXXXXXXXXX
OOOOOOOOOO
XXXXXXXXXO
```
Good solution should be: 

1. BFS
2. Union Find (with compressed path, otherwise TLE)

```cpp
// BFS
void bfs(vector<vector<char>>& bd, int i, int j, int m, int n)
{
    queue<pair<int,int>> Q;
    Q.push({i, j});
    bd[i][j] = '#';
    while (!Q.empty())
    {
        pair<int, int> p = Q.front();
        Q.pop();
        i = p.first, j = p.second;
        if (i > 0 && bd[i - 1][j] == 'O')
        {
            Q.push({i - 1, j});
            bd[i - 1][j] = '#';
        }
        if (i < m - 1 && bd[i + 1][j] == 'O')
        {
            Q.push({i + 1, j});
            bd[i + 1][j] = '#';
        }
        if (j > 0 && bd[i][j - 1] == 'O')
        {
            Q.push({i, j - 1});
            bd[i][j - 1] = '#';
        }
        if (j < n - 1 && bd[i][j + 1] == 'O')
        {
            Q.push({i, j + 1});
            bd[i][j + 1] = '#';
        }
    }
}
void solve(vector<vector<char>>& board) 
{
    int m = board.size();
    int n = m ? board[0].size() : m;
    if (m * n == 0)
        return;
    for (int i = 0; i < m; i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (board[i][j] == 'O' && (i == 0 || j == 0 || i == m - 1 || j == n - 1))
                bfs(board, i, j, m, n);
        }
    }
    for (int i = 0; i < m; i++)
    {
        for (int j = 0; j < n; j++)
        {
            board[i][j] = board[i][j] == 'O' ? 'X' : 
                board[i][j] == '#' ? 'O' : board[i][j];
        }
    }
}
```

Or Union Find:

```cpp
int find(vector<int>& father, int id)
{
    int orig = id;
    int fa = father[id];
    while (fa != id)
    {
        id = fa;
        fa = father[id];
    }
    id = orig;
    while (id != fa)
    {
        int fa_temp = father[id];
        father[id] = fa;
        id = fa_temp;
    }
    return fa;
}
void solve(vector<vector<char>>& board) 
{
    int m = board.size();
    int n = m ? board[0].size() : m;
    if (m * n == 0)
        return;
    vector<int> father(m * n + 1, -1);
    for (int i = 0; i <= m * n; i++)
        father[i] = i;
    for (int i = 0; i < m; i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (board[i][j] == 'O' && (i == 0 || i == m - 1 || j == 0 || j == n - 1))
            {
                father[i * n + j] = m * n;
            }
        }
    }
    for (int i = 0; i < m; i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (board[i][j] == 'X')
                continue;
            int offset[4][2] = { {0, 1}, {1, 0}, {0, -1}, {-1, 0} };
            for (int k = 0; k < 4; k++)
            {
                int ii = i + offset[k][0];
                int jj = j + offset[k][1];
                if (ii < 0 || jj < 0 || ii >= m || jj >= n || board[ii][jj] == 'X')
                    continue;
                int fa0 = find(father, i * n + j);
                int fa = find(father, ii * n + jj);
                if (fa != fa0)
                    father[min(fa, fa0)] = max(fa, fa0);
            }
        }
    }
    for (int i = 0; i < m; i++)
    {
        for (int j = 0; j < n; j++)
        {
            if (board[i][j] == 'O' && find(father, i * n + j) != m * n)
                board[i][j] = 'X';
        }
    }
}
```
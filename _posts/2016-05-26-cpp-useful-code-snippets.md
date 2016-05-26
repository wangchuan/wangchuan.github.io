---
layout: post
title:  "C/C++ Useful Code Snippets"
date:   2016-05-26 20:04:15
---

## `priority_queue<T, vector<T>, TComp> pq`
`priority_queue<T, vector<T>, TComp> pq`
> Note:
> (1) Max-heap: `TComp` should return `a < b`;
> (2) Min-heap: `TComp` should return `a > b`;
> (3) `priority_queue` has `top()` rather than `front()`;

```cpp
class T
{
public:
  T(int h, int x, int y) : h(h), x(x), y(y) {}
  int h, x, y;
};
struct TComp
{
  bool operator()(const T& a, const T& b) const
  {
    // max-heap. a.h > b.h will result in min-heap.
    return a.h < b.h; 
  }
};
priority_queue<T, vector<T>, TComp> pq;
```

#### `string s; char y; s.find('y')`
> Note: `size_t found = s.find('y')`; `found` is the first `'y'`'s index. If not exist, return `string::npos`;

```cpp
string a = "123.45678";
size_t found = a.find('.');
if (found != string::npos)
{
  string a1 = a.substr(0, found); // a1: 123
  string a2 = a.substr(found + 1); // a2: 45678
}
```

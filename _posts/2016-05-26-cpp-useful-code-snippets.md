---
layout: post
title:  "C/C++ Useful Code Snippets"
date:   2016-05-26 12:12:00
---

@(Coding Snippets and Instructions)

[TOC]

### C/C++ Useful Code Snippets
#### `priority_queue<T, vector<T>, TComp> pq`
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

#### string's `find_last_not_of`, `find_last_of`
1. 从`index`往前推，第一个不为`y`的索引：`s.find_last_not_of(y, index)`；不存在即返回`string::npos`.
2. 从`index`往前推，第一个为`y`的索引：`s.find_last_of(y, index)`；不存在即返回`string::npos`.

#### `lower_bound` and `upper_bound`.
> Note: The array must be pre-sorted non-d 
> (1) `lower_bound`: `1st >= tar`
> (2) `upper_bound`: `1st > tar`, i.e. `last <= tar, last + 1`

#### `accumulate`
1. $x = m + \sum_0^{n - 1} a[i]$ 
`int x = accumulate(a.begin(), a.end(), m, plus<int>());`
2. $x = m - \sum_0^{n - 1} a[i]$
`int x = accumulate(a.begin(), a.end(), m, minus<int>());`
3. $x = \text{XOR}_0^{n - 1} a[i]\ \text{xor}\ m$
`int x = accumulate(a.begin(), a.end(), init, bit_xor<int>());`
if we want to calculate `a[0] ^ a[1] ^ ... ^ a[n - 1]`, just set `init` to 0.

#### `getline`
```cpp
string temp;
getline(cin, temp, '/'); // 遇到'/'就停
string a;
stringstream ss(a);
getline(ss, temp, '/'); // 遇到'/'就停
```

#### `std::replace`
```cpp
vector<int> A = {10, 20, 20, 30};
replace(A.begin(), A.end(), 20, 50); // 10, 50, 50, 30
```

#### `std::fill`
```cpp
vector<int> A = {1, 2, 3, 4, 5};
fill(A.begin(), A.end(), 0); // => 0, 0, 0, 0, 0
```

#### `stringstream` operator `>>`
只有在执行完`>>`之后，我们才可以知道`stringstream`的状态，也就是说，即便用一个空字符串初始化`stringstream`, 其初始状态依然是好的。
```cpp
stringstream ss("abc");
char s;
while (ss)
{
	ss >> s;
	cout << s;
}
// output: abcc
```
上述代码中，会多打出一个`c`, 原因是打印完第一个`c`之后，`ss`的状态依然是正常的，即便它已经没有东西可打了。所以把`c`多输出了一次。
这个可以形象的想象成："不到死的那一天都不是死。"

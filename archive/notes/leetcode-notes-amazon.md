---
layout: post
title: "LeetCode Notes for Amazon"
categories: hidden
tags: leetcode
use_math: true
---

* TOC
{:toc}

## The Classic 9 Problems

### <em class="icon-check"></em> Maximum Subtree
Given a binary tree, find the subtree with maximum sum. Return the root of the subtree.
```bibtex
      1
    /   \
  -5     2
 /  \   / \
0    3 -4  -5        return the node of 3
```
Idea: The return data includes `maxSum`, `Sum` and the node of `maxSum`.
```cpp
class RT
{
public:
    RT() : root(NULL), Sum(0), maxSum(INT_MIN);
    int maxSum, Sum;
    TreeNode* root;
}
inline int max3(int a, int b, int c) { return max(a, max(b, c)); }
RT helper(TreeNode* root)
{
    RT rst;
    if (root == NULL)
        return rst;
    RT left = helper(root->left);
    RT right = helper(root->right);
    rst.Sum = left.Sum + right.Sum + root->val;
    rst.maxSum = max3(rst.Sum, left.maxSum, right.maxSum);
    rst.root = rst.maxSum == left.maxSum ? left.root : 
                rst.maxSum == right.maxSum ? right.root : root;
    return rst;
}
TreeNode* findSubtree(TreeNode* root)
{
    RT rst = helper(root);
    return rst.root;
}
```

### <em class="icon-check"></em> Longest Palindrome
Find the length of the longest palindrome that can be built with those letters. Case sensitive.
Given s = `"abccccdd"`, return `7`, because one longest palindrome that can be built is `"dccaccd"`.

Idea: Check the number of occurence of each character.
```cpp
int longestPalindrome(string& s)
{
    int hash[256] = {0};
    for (int i = 0; i < s.size(); i++)
        hash[s[i]]++;
    int flag = 0, count = 0;
    for (int i = 0; i < 256; i++)
    {
        count += hash[i] / 2 * 2;
        if (hash[i] % 2 && flag == 0)
            flag = 1;
    }
    count += flag;
    return count;
}
```

### <em class="icon-check"></em> Rectangle Overlap
Given two rectangles, find if they have overlap or not.
```cpp
bool doOverlap(Point& l1, Point& r1, Point& l2, Point& r2)
{
    // l: left top, r: right bottom
    int maxX = max(l1.x, l2.x);
    int minX = min(r1.x, r2.x);
    if (maxX > minX)
        return false;
    int minY = min(l1.y, l2.y);
    int maxY = max(r1.y, r2.y);
    if (maxY > minY)
        return false;
    return true;
}
```

### <em class="icon-check"></em> Window Sum
Given array `[1,2,7,8,5]` and moving window size k = `3`.
return `[10,17,20]`.
```cpp
vector<int> winSum(vector<int>& nums, int k)
{
    int n = nums.size();
    int sum = 0;
    vector<int> rst;
    for (int i = 0; i < n; i++)
    {
        sum += nums[i];
        if (i >= k)
            sum -= nums[i - k];
        if (i >= k - 1)
            rst.push_back(sum);
    }
    return rst;
}
```
### <em class="icon-check"></em> [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/?tab=Description)
Idea: Topology Sort in BFS
```cpp
vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) 
{
    vector<int> indegree(numCourses, 0);
    vector<vector<int>> relations(numCourses);
    for (int i = 0; i < prerequisites.size(); i++)
    {
        int to = prerequisites[i].first, from = prerequisites[i].second;
        indegree[to]++;
        relations[from].push_back(to);
    }
    queue<int> Q;
    for (int i = 0; i < numCourses; i++)
    {
        if (indegree[i] == 0)
            Q.push(i);
    }
    vector<int> rst;
    while (!Q.empty())
    {
        int x = Q.front();
        Q.pop();
        for (int i = 0; i < relations[x].size(); i++)
        {
            indegree[relations[x][i]]--;
            if (indegree[relations[x][i]] == 0)
                Q.push(relations[x][i]);
        }
        rst.push_back(x);
    }
    if (rst.size() != numCourses)
        rst.clear();
    return rst;
}
```

### <em class="icon-check"></em> High Five
There are two properties in the node student `id` and `scores`, to ensure that each student will have at least 5 points. Find the highest scores for each person.

Idea: Priority Queue.
```cpp
map<int, double> highFive(vector<Record>& results)
{
    int n = results.size();
    map<int, priority_queue<double>> mmap;
    for (int i = 0; i < results.size(); i++)
    {
        int id = results[i].id;
        double sc = results[i].score;
        mmap[id].push(sc);
    }
    map<int, double> rst;
    for (auto it = mmap.begin(); it != mmap.end(); it++)
    {
        priority_queue<double>& pq = it->second;
        double avg = 0.0;
        for (int i = 0; i < 5; i++)
        {
            avg += pq.top();
            pq.pop();
        }
        avg /= 5.0;
        rst[it->first] = avg;
    }
    return rst;
}
```

### <em class="icon-check"></em> K Closest Points
Given a set of points and an origin, find the K closest points to the origin. If the same distance exists, sort it by x and y coordinate.
```cpp
class TComp
{
public:
    TComp(Point x) : orig(x) {}
    double dist2(const& Point a, const& Point b) const
    {
        return (a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y);
    }
    Point orig;
    bool operator()(const Point& a, const Point& b) const
    {
        double dista = dist2(a, orig);
        double distb = dist2(b, orig);
        if (fabs(dista - distb) < 1e-7)
        {
            if (a.x == b.x)
                return a.y < b.y;
            else
                return a.x < b.x;
        }
        else
            return dista < distb;
    }
};
vector<Point> kClosest(vector<Point>& pts, Point& origin, int k)
{
    TComp TCompObj(origin);
    sort(pts.begin(), pts.end(), TCompObj);
    int n = pts.size();
    k = min(n, k);
    vector<Point> rst(pts.begin(), pts.begin() + k);
    return rst;
}
```

### <em class="icon-check"></em> [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/?tab=Description)
Idea: Hash Table
```cpp
RandomListNode *copyRandomList(RandomListNode *head) 
{
    unordered_map<RandomListNode*, RandomListNode*> map;
    RandomListNode* p = head;
    RandomListNode dummy(0);
    RandomListNode* prev = &dummy;
    while (p)
    {
        prev->next = new RandomListNode(p->label);
        map[p] = prev->next;
        p = p->next;
        prev = prev->next;
    }
    p = head;
    RandomListNode* q = dummy.next;
    while (p)
    {
        RandomListNode* pr = p->random;
        RandomListNode* prnew = map[pr];
        q->random = prnew;
        p = p->next;
        q = q->next;
    }
    return dummy.next;
}
```
### <em class="icon-check"></em> Minimum Spanning Tree <em class="icon-thumbs-up"></em>
Given a list of Connections, which is the Connection class (the city name at both ends of the edge and a cost between them), find some edges, connect all the cities and spend the least amount. Return the connects if can connect all the cities, otherwise return empty list. 

Example 
Gievn the connections = `["Acity","Bcity",1], ["Acity","Ccity",2], ["Bcity","Ccity",3]`
Return `["Acity","Bcity",1]`, `["Acity","Ccity",2]`

Idea: Kruskal Algorithm:

1. sort all edges in ascending order
2. for-loop
    - each time pick up an edge `e` with minimal cost
    - check if its endings belongs to different regions or not, 
        - if yes
            `rst <== e`, then merge the two regions into one region
        - otherwise
            drop it
3. return `rst`

```cpp
struct TComp
{
    bool operator()(const Connection& a, const Connection& b) const
    {
        if (a.cost != b.cost)
            return a.cost < b.cost;
        if (a.city1 != b.city1)
            return a.city1 < b.city1;
        return a.city2 < b.city2;
    }
};
vector<Connection> lowestCost(vector<Connection>& connections)
{
    TComp TCompObj;
    sort(connections.begin(), connections.end(), TCompObj);
    unordered_map<string, int> hash;
    int id = 0;
    for (int i = 0; i < connections.size(); i++)
    {
        string s1 = connections[i].city1, s2 = connections[i].city2;
        if (!hash.count(s1))
            hash[s1] = id++;
        if (!hash.count(s2))
            hash[s2] = id++;
    }
    vector<int> father(hash.size(), 0);
    for (int i = 0; i < father.size(); i++)
        father[i] = i;
    vector<Connection> rst;
    for (int i = 0; i < connections.size(); i++)
    {
        int id1 = hash[connections[i].city1];
        int id2 = hash[connections[i].city2];
        int fa1 = find(id1, father);
        int fa2 = find(id2, father);
        if (fa1 != fa2)
        {
            father[fa1] = fa2;
            rst.push_back(connections[i]);
        }
    }
    if (rst.size() != (int)hash.size() - 1)
        rst.clear();
    return rst;
}
```

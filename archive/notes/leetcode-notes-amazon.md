---
layout: post
title: "LeetCode Notes for Amazon"
categories: hidden
tags: special
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
};
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

Or topological sort in DFS.
```cpp
bool dfs(int i, vector<vector<int>>& relations, vector<int>& rst, vector<int>& status)
{
    if (status[i] == 2)
        return true;
    if (status[i] == 1)
        return false;
    status[i] = 1;
    for (int j = 0; j < relations[i].size(); j++)
    {
        bool x = dfs(relations[i][j], relations, rst, status);
        if (!x)
            return false;
    }
    rst.push_back(i);
    status[i] = 2;
    return true;
}
vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) 
{
    vector<vector<int>> relations(numCourses);
    for (int i = 0; i < prerequisites.size(); i++)
    {
        int to = prerequisites[i].first;
        int from = prerequisites[i].second;
        relations[from].push_back(to);
    }
    vector<int> rst;
    vector<int> status(numCourses, 0);
    bool x = true;
    for (int i = 0; i < numCourses && x; i++)
    {
        x = dfs(i, relations, rst, status);
    }
    if (x)
        reverse(rst.begin(), rst.end());
    else
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
Given the connections = `["Acity","Bcity",1], ["Acity","Ccity",2], ["Bcity","Ccity",3]`
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
int find(int id, vector<int>& father)
{
    int fa = father[id];
    while (fa != id)
    {
        id = fa;
        fa = father[id];
    }
    return id;
}
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

---

## More Problems in LeetCode

### <em class="icon-check"></em> [H-Index](https://leetcode.com/problems/h-index/?tab=Description)
Idea: Hash Table. `n` publications, we create a hashtable `hash` of size `n + 1`. `hash[i]` records how many publications of citation `i`. For `hash[n]`, it records how many publications of citations `>= i`. Accumulate `hash` and get the result.
```cpp
int hIndex(vector<int>& citations) 
{
    int n = citations.size();
    vector<int> hash(n + 1, 0);
    for (int i = 0; i < n; i++)
    {
        int c = citations[i];
        c = min(c, n);
        hash[c]++;
    }
    if (hash[n] >= n)
        return n;
    for (int i = n - 1; i >= 0; i--)
    {
        hash[i] += hash[i + 1];
        if (hash[i] >= i)
            return i;
    }
    return 0;
}
```

### <em class="icon-check"></em> [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/?tab=Description)
Idea: Notice for `startWith` function, after finishing traversing the `prefix`, we need to check whether the current pointer: has `children` or itself contains word.
```cpp
class Trie {
    class TrieNode
    {
    public:
        TrieNode* children[26] = {NULL};
        int count = 0;
    };
public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        int n = word.size();
        TrieNode *p = root;
        for (int i = 0; i < n; i++)
        {
            int id = word[i] - 'a';
            if (p->children[id] == NULL)
                p->children[id] = new TrieNode();
            p = p->children[id];
        }
        p->count++;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        int n = word.size();
        TrieNode* p = root;
        for (int i = 0; i < n; i++)
        {
            int id = word[i] - 'a';
            if (p->children[id] == NULL)
                return false;
            p = p->children[id];
        }
        return p->count;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        int n = prefix.size();
        TrieNode* p = root;
        for (int i = 0; i < n; i++)
        {
            int id = prefix[i] - 'a';
            if (p->children[id] == NULL)
                return false;
            p = p->children[id];
        }
        for (int i = 0; i < 26; i++)
            if (p->children[i])
                return true;
        return p->count;
    }
    
    TrieNode* root;
};
```

### <em class="icon-check"></em> [3Sum](https://leetcode.com/problems/3sum/?tab=Description)
Idea: Sort, and fix the first one, apply 2Sum to the rest. `O(n^2)`
```cpp
vector<vector<int>> threeSum(vector<int>& nums) 
{
    int n = nums.size();
    sort(nums.begin(), nums.end());
    vector<vector<int>> rst;
    for (int i = 0; i < n - 2; i++)
    {
        if (i > 0 && nums[i] == nums[i - 1])
            continue;
        int s = i + 1, e = n - 1;
        while (s < e)
        {
            if (nums[s] + nums[e] < -nums[i])
                s++;
            else if (nums[s] + nums[e] > -nums[i])
                e--;
            else
            {
                rst.push_back({nums[i], nums[s], nums[e]});
                int temp1 = nums[s], temp2 = nums[e];
                while (s < e && nums[s] == temp1) s++;
                while (s < e && nums[e] == temp2) e--;
            }
        }
    }
    return rst;
}
```

### <em class="icon-check"></em> [3Sum Closest](https://leetcode.com/problems/3sum-closest/?tab=Description)
Idea: Similar to 3Sum. 
```cpp
int threeSumClosest(vector<int>& nums, int target) 
{
    int n = nums.size();
    sort(nums.begin(), nums.end());
    int mindist = INT_MAX, rst, dist;
    for (int i = 0; i < n; i++)
    {
        if (i > 0 && nums[i] == nums[i - 1])
            continue;
        int s = i + 1, e = n - 1;
        int first = nums[i], tar = target - first;
        while (s < e)
        {
            int sum = nums[s] + nums[e];
            if (sum == tar)
                return target;
            dist = abs(first + sum - target);
            rst = dist < mindist ? first + sum : rst;
            mindist = min(dist, mindist);
            if (sum < tar)
                s++;
            else
                e--;
        }
    }
    return rst;
}
```

### <em class="icon-check"></em> [String to Integer (atoi) <em class="icon-thumbs-up"></em>](https://leetcode.com/problems/string-to-integer-atoi/?tab=Description) 
Idea: 

1. Find the first non-white-space character, if no such one, then return `0`.
2. Check the first non-white-space character, if it's `+` or `-`, then record its sign.
3. Traverse each character, if found non-digit, break the loop; otherwise, update the result.
    Note that, when update, if found current value is `> INT_MAX / 10`, which means the result must be out-of-range regardless of sign; Similarly, if foudn current value is `= INT_MAX / 10` and incoming value is `>= 8`, then the result must be out-of-range regardless of sign too.

```cpp
int myAtoi(string str) 
{
    int pos = str.find_first_not_of(' ');
    if (pos == string::npos)
        return 0;
    int sign = 1;
    if (str[pos] == '-' || str[pos] == '+')
        sign = 1 - 2 * (str[pos++] == '-');
    int rst = 0;
    int n = str.size();
    for (int i = pos; i < n && str[i] != ' '; i++)
    {
        if (str[i] > '9' || str[i] < '0')
            break;
        int x = str[i] - '0';
        if (rst > INT_MAX / 10 || (rst == INT_MAX / 10 && x > 7))
        {
            return sign == 1 ? INT_MAX : INT_MIN;
        }
        rst = rst * 10 + x;
    }
    rst *= sign;
    return rst;
}
```

### <em class="icon-check"></em> [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/?tab=Description)
Idea: Find the different steps of two pointers.
```cpp
ListNode *shift(ListNode* p, ListNode* head)
{
    ListNode *p0;
    if (p)
    {
        p0 = head;
        while (p)
        {
            p = p->next;
            p0 = p0->next;
        }
        p = p0;
    }
    else
        p = head;
    return p;
}
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) 
{
    ListNode *p1 = headA, *p2 = headB, *p;
    while (p1 && p2)
    {
        p1 = p1->next;
        p2 = p2->next;
    }
    p1 = shift(p1, headA);
    p2 = shift(p2, headB);
    while (p1 && p2 && p1 != p2)
    {
        p1 = p1->next;
        p2 = p2->next;
    }
    return p1;
}
```

### <em class="icon-check"></em> [Reverse Integer](https://leetcode.com/problems/reverse-integer/?tab=Description)
Idea: Note that `2147483647` would be reversed to `7463847421`, overflow. So we need to use `long long`. Use `llabs` instead of `abs` for `long long`.
```cpp
int reverse(int x) 
{
    int sign = x > 0 ? 1 : -1;
    long long llx = x;
    llx = llabs(llx);
    long long rst = 0;
    while (llx)
    {
        int y = llx % 10;
        llx /= 10;
        rst = rst * 10 + y;
        if (rst * sign > INT_MAX || rst * sign < INT_MIN)
            return 0;
    }
    return rst * sign;
}
```

### <em class="icon-check"></em> [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/?tab=Description)
Idea: Clearup the redundant whitespaces in a string. Then add a dummy whitespace at the end. Now the string looks like `abcd␣efg␣hi␣jklmn␣`. Then reverse each words to `dcba␣gfe␣ih␣nmlkj␣`. Pop-back the string, and reverse the whole string.
```cpp
void sortout(string& s)
{
    int n = s.size();
    int i = 0, j = 0;
    while (j < n)
    {
        if (s[j] != ' ' || j > 0 && s[j - 1] != ' ' && s[j] == ' ')
            s[i++] = s[j++];
        else
            j++;
    }
    s.resize(i);
    if (!s.empty() && s.back() != ' ')
        s.push_back(' ');
}
void reverseWords(string &s) 
{
    sortout(s);
    if (s.empty())
        return;
    int prev = 0;
    while (prev < s.size())
    {
        int pos = s.find(' ', prev);
        reverse(s.begin() + prev, s.begin() + pos);
        prev = pos + 1;
    }
    s.pop_back();
    reverse(s.begin(), s.end());
}
```

### <em class="icon-check"></em> [Pow(x, n) <em class="icon-thumbs-up"></em>](https://leetcode.com/problems/powx-n/?tab=Description)
Idea: `n` could be `INT_MAX` or `INT_MIN`, so we need to use `long long`.
```cpp
double myPow(double x, int n) 
{
    if (fabs(x) < 1e-7)
        return 0;
    if (n == 0)
        return 1;
    int sign = n > 0 ? 1 : -1;
    long long lln = abs((long long)n);
    double rst = 1.0;
    while (lln)
    {
        int a = (lln & 1);
        if (a == 1)
            rst *= x;
        x *= x;
        lln >>= 1;
    }
    return sign == 1 ? rst : 1.0 / rst;
}
```

### <em class="icon-check"></em> [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/?tab=Description)
Idea: record `prev, p1, p2, q` four pointers.
```cpp
ListNode* swapPairs(ListNode* head) 
{
    if (head == NULL || head->next == NULL)
        return head;
    ListNode dummy(0);
    dummy.next = head;
    ListNode *prev = &dummy, *p1 = head, *p2 = head->next, *q;
    while (p1 && p2)
    {
        q = p2->next;
        prev->next = p2;
        p2->next = p1;
        p1->next = q;
        prev = p1;
        p1 = q;
        p2 = p1 ? p1->next : p1;
    }
    return dummy.next;
}
```

### <em class="icon-check"></em> [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/?tab=Description)
Idea: Count the number of elements of its left children `n1`. Compare `k` with `n1`.
```cpp
int nElements(TreeNode* root)
{
    if (root == NULL)
        return 0;
    return nElements(root->left) + nElements(root->right) + 1;
}
int kthSmallest(TreeNode* root, int k) 
{
    if (root == NULL)
        return 0;
    int n1 = nElements(root->left);
    if (k == n1 + 1)
        return root->val;
    else if (k < n1 + 1)
        return kthSmallest(root->left, k);
    else
        return kthSmallest(root->right, k - n1 - 1);
}
```

### <em class="icon-check"></em> [Move Zeroes](https://leetcode.com/problems/move-zeroes/#/description)
Idea: two pointers.
```cpp
void moveZeroes(vector<int>& nums) 
{
    int n = nums.size();
    int i = 0, j = 0;
    while (j < n)
    {
        if (nums[j] != 0)
            swap(nums[i++], nums[j++]);
        else
            j++;
    }
}
```

### <em class="icon-check"></em> [Two Sum](https://leetcode.com/problems/two-sum/#/description)
Idea: hash map.
```cpp
vector<int> twoSum(vector<int>& nums, int target) 
{
    unordered_map<int, int> map;
    int n = nums.size();
    vector<int> rst(2, -1);
    for (int i = 0; i < n; i++)
    {
        auto it = map.find(target - nums[i]);
        if (it != map.end())
        {
            rst[0] = it->second;
            rst[1] = i;
            return rst;
        }
        map.insert({nums[i], i});
    }
    return rst;
}
```

### <em class="icon-check"></em> [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/#/description)
Idea: record `minval` up to now.
```cpp
int maxProfit(vector<int>& prices) 
{
    int n = prices.size();
    if (n <= 1)
        return 0;
    int minval = prices[0], maxdiff = 0;
    for (int i = 1; i < n; i++)
    {
        maxdiff = max(maxdiff, prices[i] - minval);
        minval = min(minval, prices[i]);
    }
    return maxdiff;
}
```

### <em class="icon-check"></em> [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/#/description)
Idea: Note to check `carry` at the end.
```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) 
{
    ListNode *p1 = l1, *p2 = l2;
    if (l1 == NULL || l2 == NULL)
        return l1 ? l1 : l2;
    int carry = 0;
    ListNode dummy(0);
    ListNode *p, *prev = &dummy;
    while (p1 || p2)
    {
        int n1 = p1 ? p1->val : 0;
        int n2 = p2 ? p2->val : 0;
        int sum = n1 + n2 + carry;
        prev->next = new ListNode(sum % 10);
        prev = prev->next;
        carry = sum / 10;
        p1 = p1 ? p1->next : p1;
        p2 = p2 ? p2->next : p2;
    }
    if (carry)
        prev->next = new ListNode(carry);
    return dummy.next;
}
```

### <em class="icon-check"></em> [Min Stack](https://leetcode.com/problems/min-stack/#/description)
Idea: two stacks.
```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
        
    }
    
    void push(int x) {
        S1.push(x);
        if (S2.empty() || S2.top() >= x)
            S2.push(x);
    }
    
    void pop() {
        if (S1.empty())
            return;
        int x1 = S1.top(), x2 = S2.top();
        S1.pop();
        if (x1 <= x2)
            S2.pop();
    }
    
    int top() {
        return S1.top();
    }
    
    int getMin() {
        return S2.top();
    }
    
    stack<int> S1, S2;
};
```

### <em class="icon-check"></em> [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/#/description)
Idea: Divide and Conquer. Return a structure to store multiple information.
```cpp
class RT
{
public:
    RT(int x1, int x2, bool x3) : minval(x1), maxval(x2), isValid(x3) {}
    int minval, maxval;
    bool isValid;
};
inline int min3(int a, int b, int c) { return min(a, min(b, c)); }
inline int max3(int a, int b, int c) { return max(a, max(b, c)); }
RT helper(TreeNode* root)
{
    if (!root->left && !root->right)
        return RT(root->val, root->val, true);
    else if (root->left && root->right)
    {
        RT left = helper(root->left);
        RT right = helper(root->right);
        return RT(min3(left.minval, right.minval, root->val), max3(left.maxval, right.maxval, root->val), 
            left.maxval < root->val && root->val < right.minval && left.isValid && right.isValid);
    }
    else if (root->left)
    {
        RT left = helper(root->left);
        return RT(min(root->val, left.minval), max(root->val, left.maxval), left.maxval < root->val && left.isValid);
    }
    else
    {
        RT right = helper(root->right);
        return RT(min(root->val, right.minval), max(root->val, right.maxval), root->val < right.minval && right.isValid);
    }
    return RT(-1,-1,false);
}
bool isValidBST(TreeNode* root) 
{
    if (root == NULL)
        return true;
    RT rst = helper(root);
    return rst.isValid;
}
``` 

### <em class="icon-check"></em> [Longest Substring Without Repeating Characters <em class="icon-thumbs-up"></em>](https://leetcode.com/problems/longest-substring-without-repeating-characters/#/description) 
Idea: two pointers.
```cpp
int lengthOfLongestSubstring(string s) 
{
    int n = s.size();
    int hash[256] = {0};
    int i = 0, j = 0, maxlen = 0;
    while (j < n)
    {
        while (j < n && hash[s[j]] == 0)
        {
            hash[s[j]] = 1;
            j++;
            maxlen = max(maxlen, j - i);
        }
        while (i < j && hash[s[j]] == 1)
        {
            hash[s[i]] = 0;
            i++;
        }
    }
    return maxlen;
}
```

### <em class="icon-check"></em> [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/#/description) 
Idea: Quick sort. Partition to find the pivot index, then check the index we are seeking with the pivot index.
```cpp
int partition(vector<int>& nums, int s, int e)
{
    if (s > e)
        return s;
    int pivot = nums[e];
    int i = s, j = e;
    while (i <= j)
    {
        while (i <= j && nums[i] < pivot) i++;
        while (i <= j && nums[j] >= pivot) j--;
        if (i <= j)
        {
            swap(nums[i], nums[j]);
        }
    }
    swap(nums[e], nums[i]); // here swap(nums[e], nums[j]) is incorrect!!!
    return i;
}
int findKth(vector<int>& nums, int s, int e, int index)
{
    int mid = partition(nums, s, e);
    if (index == mid)
        return nums[mid];
    else if (index < mid)
        return findKth(nums, s, mid - 1, index);
    else
        return findKth(nums, mid + 1, e, index);
}
int findKthLargest(vector<int>& nums, int k) 
{
    int n = nums.size();
    random_shuffle(nums.begin(), nums.end());
    return findKth(nums, 0, n - 1, n - k);
}
```

### <em class="icon-check"></em> [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/#/description)
Idea: two pointers.
```cpp
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) 
{
    int i = m - 1, j = n - 1, k = m + n - 1;
    while (i >= 0 && j >= 0)
    {
        if (nums1[i] >= nums2[j])
            nums1[k--] = nums1[i--];
        else
            nums1[k--] = nums2[j--];
    }
    while (j >= 0)
        nums1[k--] = nums2[j--];
}
```

### <em class="icon-check"></em> [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/#/description)
Idea: two pointers.
```cpp
bool hasCycle(ListNode *head) 
{
    if (head == NULL)
        return false;
    ListNode *p = head, *q = head->next;
    while (p && q && q->next && p != q)
    {
        p = p->next;
        q = q->next->next;
    }
    return p == q;
}
```

### <em class="icon-check"></em> [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/#/description)
Idea: stack. Note to check stack empty or not at the end.
```cpp
bool isValid(string s) 
{
    stack<char> S;
    char map[256] = {0};
    map[')'] = '(';
    map[']'] = '[';
    map['}'] = '{';
    for (int i = 0; i < s.size(); i++)
    {
        if (s[i] == '(' || s[i] == '[' || s[i] == '{')
            S.push(s[i]);
        else
        {
            if (S.empty() || S.top() != map[s[i]])
                return false;
            else
                S.pop();
        }
    }
    return S.empty();
}
```

### <em class="icon-check"></em> [Merge Intervals](https://leetcode.com/problems/merge-intervals/#/description)
Idea: sort and check the tail.
```cpp
vector<Interval> merge(vector<Interval>& intervals) 
{
    sort(intervals.begin(), intervals.end(), [](Interval& a, Interval& b){ return a.start < b.start; });
    vector<Interval> rst;
    for (int i = 0; i < intervals.size(); i++)
    {
        if (rst.empty() || rst.back().end < intervals[i].start)
            rst.push_back(intervals[i]);
        else
            rst.back().end = max(rst.back().end, intervals[i].end);
    }
    return rst;
}
```

### <em class="icon-check"></em> [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/#/description)
Idea: two pointers.
```cpp
int removeDuplicates(vector<int>& nums) 
{
    int n = nums.size();
    int i = 0, j = 0;
    while (j < n)
    {
        if (j > 0 && nums[j] == nums[j - 1])
            j++;
        else
            nums[i++] = nums[j++];
    }
    nums.resize(i);
    return i;
}
```





---

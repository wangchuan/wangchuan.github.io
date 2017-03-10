---
layout: post
title: "LeetCode Notes - 1"
categories: hidden
tags: special
use_math: true
---

* TOC
{:toc}

## LeetCode and LintCode Problems Summary

### Time Complexity Analysis
1. $f(n) = n + 2 f(\frac{n}{2}) \rightarrow f(n) = n\log(n)$
2. $f(n) = n + f(\frac{n}{2}) \rightarrow f(n) = 2n$
3. $f(n) = 1 + f(\frac{n}{2}) \rightarrow f(n) = \log(n) $
4. Two pointers: `i` and `j` moving forward, but in `for-for` loop. It's `O(2n)`.

### DFS

#### <em class="icon-check"></em>  [Generate Parentheses](http://www.lintcode.com/en/problem/generate-parentheses/)
Idea: Number of left parentheses and right parentheses as parameters separately into dfs function.
```cpp
void dfs(int nleft, int nright, string& crtrst, vector<string>& finalrst)
{
    if (nleft > nright || nleft < 0 || nright < 0)
        return;
    if (nleft == 0 && nright == 0)
    {
        finalrst.push_back(crtrst);
        return;
    }
    crtrst += '(';
    dfs(nleft - 1, nright, crtrst, finalrst);
    crtrst.pop_back();
    crtrst += ')';
    dfs(nleft, nright - 1, crtrst, finalrst);
    crtrst.pop_back();
}
vector<string> generateParenthesis(int n) 
{
    vector<string> finalrst;
    string crtrst = "";
    if (n == 0)
        return finalrst;
    dfs(n, n, crtrst, finalrst);
    return finalrst;
}
```

#### <em class="icon-check"></em>  [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
Idea 1: DFS each node, to compute the `minval` and `maxval` of the subtree rooted at this node. The condition is `v0 > lmax && v0 < rmin`
Idea 2: Divide & Conquer: Use **ResultType** to return multiple values. In this problem, **ResultType** includes `isBST`, `minVal` and `maxVal`. The condition is that: both subtrees are BST, and `left's maxVal < root->val < right's minVal`
```cpp
class Node
{
public:
    Node() : maxval(INT_MIN), minval(INT_MAX), isvalid(true) {}
    Node(int x1, int x2, bool x3) : maxval(x1), minval(x2), isvalid(x3) {}
    int maxval, minval;
    bool isvalid;
};
inline int max3(int a, int b, int c)
{
    return max(a, max(b, c));
}
inline int min3(int a, int b, int c)
{
    return min(a, min(b, c));
}
Node helper(TreeNode* root)
{
    if (root == NULL)
        return Node();
    Node leftrst = helper(root->left);
    Node rightrst = helper(root->right);
    Node rst;
    rst.maxval = max3(leftrst.maxval, rightrst.maxval, root->val);
    rst.minval = min3(leftrst.minval, rightrst.minval, root->val);
    if (root->left && root->right)
    {
        rst.isvalid = leftrst.isvalid && rightrst.isvalid
            && root->val > leftrst.maxval && root->val < rightrst.minval;
        return rst;
    }
    else if (root->left == NULL && root->right == NULL)
    {
        rst.isvalid = true;
        return rst;
    }
    else if (root->left && root->right == NULL)
    {
        rst.isvalid = leftrst.isvalid && root->val > leftrst.maxval;
        return rst;
    }
    else
    {
        rst.isvalid = rightrst.isvalid && root->val < rightrst.minval;
        return rst;
    }
}
bool isValidBST(TreeNode *root) 
{
    if (root == NULL)
        return true;
    Node rst = helper(root);
    return rst.isvalid;
}
```

#### <em class="icon-check"></em>  [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
Idea: DFS two nodes sharing the same father node. The condition is 
1. two nodes NULL or
2. two nodes have the same values and `left node's left == right node's right && left node's right == right node's left`

#### <em class="icon-check"></em>  [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)
Idea: Use `crtnum`, `finalsum` to do the DFS. If the current node is a leaf node, add the `crtnum` to `finalsum`. Leaf node is the stop condition (**Leaf Node Stop Condition**), so must add `if (root->left)` or `if (root->right)` to the DFS function.
```cpp
void dfs(TreeNode* root, int crtrst, int& finalrst)
{
    int val = root->val;
    crtrst = crtrst * 10 + val;
    if (root->left == NULL && root->right == NULL)
    {
        finalrst += crtrst;
        return;
    }
    if (root->left)
        dfs(root->left, crtrst, finalrst);
    if (root->right)
        dfs(root->right, crtrst, finalrst);
}
int sumNumbers(TreeNode* root) 
{
    int finalrst = 0;
    int crtrst = 0;
    if (root == NULL)
        return 0;
    dfs(root, crtrst, finalrst);
    return finalrst;
}
```
#### <em class="icon-check"></em>  [Path Sum](https://leetcode.com/problems/path-sum/) (Classic) / [Path Sum II](https://leetcode.com/problems/path-sum-ii/)
Idea: Similar to [Sum Root to Leaf Numbers](sum-root-to-leaf-numbers), use **Leaf Node Stop Condition** to do DFS. For [Path Sum II](https://leetcode.com/problems/path-sum-ii/), we should carefully `push_back` and `pop_back` the current node before return. **DFS template will always be like this: `finalrst, crtrst, crtstatus, push_back(), pop_back()`**.
```cpp
// Path Sum I
bool hasPathSum(TreeNode* root, int sum) 
{
    if (root == NULL)
        return false;
    sum -= root->val;
    if (root->left == NULL && root->right == NULL)
        return sum == 0;
    if (root->left && hasPathSum(root->left, sum) ||
        root->right && hasPathSum(root->right, sum))
        return true;
    return false;
}
// Path Sum II
void dfs(TreeNode* root, vector<int>& crtrst, 
    vector<vector<int>>& finalrst, int sum)
{
    sum -= root->val;
    crtrst.push_back(root->val);
    if (root->left == NULL && root->right == NULL)
    {
        if (sum == 0)
            finalrst.push_back(crtrst);
        crtrst.pop_back();
        return;
    }
    if (root->left)
        dfs(root->left, crtrst, finalrst, sum);
    if (root->right)
        dfs(root->right, crtrst, finalrst, sum);
    crtrst.pop_back();
}
vector<vector<int>> pathSum(TreeNode* root, int sum) 
{
    vector<vector<int>> finalrst;
    vector<int> crtrst;
    if (root == NULL)
        return finalrst;
    dfs(root, crtrst, finalrst, sum);
    return finalrst;
}
```

#### <em class="icon-check"></em>  [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
Idea: **Leaf Node Stop Condition**: if leaf node, then return `1`; else return `min(x1,x2)+1`; x1,x2 are minDepth of left and right subtree. 

#### <em class="icon-check"></em>  [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
Idea: very similar to [Minimum Depth of Binary Tree](nolink).

#### <em class="icon-check"></em>  [Same Tree](https://leetcode.com/problems/same-tree/) 
Idea: Very similar to [Symmetric Tree](nolink). The condition is
1. two nodes NULL or
2. two nodes have the same values and `left's left == right's left && left's right == right's right`

#### <em class="icon-check"></em>  [Number of Islands](https://leetcode.com/problems/number-of-islands/)
Idea: traverse each pixel, if it's `1`, then `sum++` and set its neighbors `2`.

#### <em class="icon-check"></em>  [Flatten Binary Tree to Linked List](https://leetcode.com/submissions/detail/45641105/) (Hard)
Idea: if current node is leaf, return itself. Otherwise, make its right points to its left, DFS on left, and return the last pointer. This pointer's right points to the original right. 
```cpp
typedef pair<TreeNode*, TreeNode*> Node;
Node helper(TreeNode* root)
{
    if (root->left == NULL && root->right == NULL)
        return Node(root, root);
    else if (root->left && root->right == NULL)
    {
        Node x = helper(root->left);
        root->right = x.first;
        root->left = NULL;
        return Node(root, x.second);
    }
    else if (root->left == NULL && root->right)
    {
        Node x = helper(root->right);
        root->right = x.first;
        return Node(root, x.second);
    }
    else
    {
        Node x = helper(root->left);
        Node y = helper(root->right);
        x.second->right = y.first;
        root->right = x.first;
        root->left = NULL;
        return Node(root, y.second);
    }
}
void flatten(TreeNode* root) 
{
    if (root == NULL)
        return;
    Node x = helper(root);
}
```

#### <em class="icon-check"></em>  [Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/) (Find the middle node of a linkedlist within a range)
Idea: Find the mid node `mid` of a range `[head,tail)` of a linked list, set `mid` as the root of the BST. Its left child is the DFS result of `[head,mid)`, and its right child is the DFS result of `[mid->next,tail)`.
```cpp
// find the middle node of a linked list within range [p,q)
ListNode* find_mid_node(ListNode* p, ListNode* q)
{
	if (p == q)
		return NULL;
	ListNode *mid, *temp;
	mid = temp = p;
	while (temp != q && temp->next != q)
	{
		mid = mid->next;
		temp = temp->next->next;
	}
	return mid;
}
```

A simpler solution: 
1. Find middle of a linked list, noted as `mid`
2. Set `mid`'s `pre`'s next `= NULL`, then, convert its left part as the left tree, and right part as the right tree, and `mid` as root;
```cpp
TreeNode* sortedListToBST(ListNode* head) 
{
    if (head == NULL)
        return NULL;
    ListNode dummy(0);
    dummy.next = head;
    ListNode *p = head, *q = head, *pre = &dummy;
    while (q && q->next && q->next->next)
    {
        pre = p;
        p = p->next;
        q = q->next->next;
    }
    pre->next = NULL;
    TreeNode* root = new TreeNode(p->val);
    root->left = sortedListToBST(dummy.next);
    root->right = sortedListToBST(p->next);
    return root;
}
```

#### <em class="icon-check"></em>  [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)
Idea: very similar to [Convert Sorted List to Binary Search Tree](nolink). In array, we use `[s,e]` to represent a range, because computing `mid` can be `mid = s + (e-s)/2;`. And stop condition is **`if (e < s)`**.

#### <em class="icon-check"></em>  [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
Idea: The first element of preorder array is the root, find this element in inorder array, then we know the elements on the left of this element in the inorder array is the left subtree, and the elements on the right of this element in the inorder array is the right subtree. Note: `[s,e]` results in stop condition is `if (s > e)`
```cpp
TreeNode* helper(vector<int>& P, int s1, int e1, vector<int>& I, int s2, int e2)
{
    if (s2 > e2 || s1 > e1)
        return NULL;
    int rootval = P[s1];
    int index = s2;
    for (; index <= e2 && I[index] != rootval; index++);
    int nleft = index - 1 - s2 + 1, nright = e2 - (index + 1) + 1;
    TreeNode *root = new TreeNode(rootval);
    root->left = helper(P, s1 + 1, s1 + nleft, I, s2, index - 1);
    root->right = helper(P, s1 + nleft + 1, s1 + nleft + nright, I, index + 1, e2);
    return root;
}
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) 
{
    int n1 = preorder.size(), n2 = inorder.size();
    if (n1 != n2 || n1 == 0)
        return NULL;
    return helper(preorder, 0, n1 - 1, inorder, 0, n2 - 1);
}
```

#### <em class="icon-check"></em>  [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
Idea: Very similar to [Construct Binary Tree from Preorder and Inorder Traversal](nolink).
```cpp
TreeNode* helper(vector<int>& inorder, int s1, int e1, 
	vector<int>& postorder, int s2, int e2)
{
    if (s1 > e1 || s2 > e2)
        return NULL;
    int rootval = postorder[e2];
    TreeNode* root = new TreeNode(rootval);
    int index = find(inorder.begin() + s1, inorder.begin() + e1 + 1, rootval)
			     - inorder.begin();
    root->left = helper(inorder, s1, index - 1, postorder, s2, index - 1 - s1 + s2);
    root->right = helper(inorder, index + 1, e1, postorder, index - s1 + s2, e2 - 1);
    return root;
}
TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) 
{
    int n1 = inorder.size(), n2 = postorder.size();
    if (n1 != n2 || n1 == 0)
        return NULL;
    return helper(inorder, 0, n1 - 1, postorder, 0, n2 - 1);
}
```
#### <em class="icon-check"></em>  [Clone Graph](https://leetcode.com/problems/clone-graph/) (Graph, Check Again!)
Idea: use `unordered_map<int,*> to store the cloned node`.
```cpp
UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) 
{
    if (node == NULL)
        return NULL;
    typedef UndirectedGraphNode UGN;
    unordered_map<int, UGN*> hashMap;
    unordered_set<UGN*> S;
    // Step 1. Clone nodes.
    queue<UGN*> Q;
    Q.push(node);
    UGN* cp = new UGN(node->label);
    hashMap[node->label] = cp;
    int n1 = 1, n2 = 0;
    while (!Q.empty())
    {
        UGN* x = Q.front();
        Q.pop();
        n1--;
        for (int i = 0; i < x->neighbors.size(); i++)
        {
            UGN* nb = x->neighbors[i];
            if (hashMap.count(nb->label))
                continue;
            hashMap[nb->label] = new UGN(nb->label);
            Q.push(nb);
            n2++;
        }
        if (n1 == 0)
            swap(n1, n2);
    }
    // Step 2. Clone neighbors.
    UGN* rst = hashMap[node->label];
    Q.push(node);
    S.insert(node);
    n1 = 1, n2 = 0;
    while (!Q.empty())
    {
        UGN* x = Q.front();
        UGN* cpx = hashMap[x->label];
        Q.pop();
        n1--;
        for (int i = 0; i < x->neighbors.size(); i++)
        {
            UGN* nb = x->neighbors[i];
            UGN* cpnb = hashMap[nb->label];
            cpx->neighbors.push_back(cpnb);
            if (S.count(nb))
                continue;
            S.insert(nb);
            Q.push(nb);
            n2++;
        }
        if (n1 == 0)
            swap(n1, n2);
    }
    return rst;
}
```
A more concise DFS version:
```cpp
unordered_map<UndirectedGraphNode*, UndirectedGraphNode*> hash;
UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) 
{
   if (node == NULL) 
	   return node;
   if (hash.find(node) == hash.end()) 
   {
       hash[node] = new UndirectedGraphNode(node->label);
       for (auto x : node->neighbors) 
       {
            (hash[node]->neighbors).push_back(cloneGraph(x));
       }
   }
   return hash[node];
}
```
#### <em class="icon-check"></em>  [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/) (Classic)
> **Summary**
> 1. Leaf Node Stop condition
> 2. If Processing is in current root, then restore it to its original value. (Note: Each branch needs restoration!)

#### <em class="icon-check"></em>  [House Robber III](https://leetcode.com/problems/house-robber-iii/)
Idea: Divide and Conquer. For each node, there are two results, selected `rst[1]` or not selected `rst[0]`.
1. `rst[0] = max(leftrst[0], leftrst[1]) + max(rightrst[0], rightrst[1])`
2. `rst[1] = root->val + leftrst[0] + rightrst[0]`.
```cpp
vector<int> helper(TreeNode* root)
{
    if (root == NULL)
        return vector<int>(2, 0);
    vector<int> rst(2);
    vector<int> leftrst = helper(root->left);
    vector<int> rightrst = helper(root->right);
    rst[0] = max(leftrst[0], leftrst[1]) + max(rightrst[0], rightrst[1]);
    rst[1] = root->val + leftrst[0] + rightrst[0];
    return rst;
}
int rob(TreeNode* root) 
{
    vector<int> rst = helper(root);
    return max(rst[0], rst[1]);
}
```

#### <em class="icon-check"></em>  [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)
Idea: Divide and Conquer. Define two variables `isbalanced` and `depth` stored into a `struct`. And check whether `abs(lefttree's depth - righttree's depth) <= 1 && lefttree isbalanced && righttree isbalanced`.
```cpp
class Node
{
public:
    Node() : depth(0), isbalanced(true) {}
    Node(int x1, bool x2) : depth(x1), isbalanced(x2) {}
    int depth;
    bool isbalanced;
};
Node helper(TreeNode* root)
{
    if (root == NULL)
        return Node();
    Node leftrst = helper(root->left);
    Node rightrst = helper(root->right);
    Node rst;
    rst.depth = max(leftrst.depth, rightrst.depth) + 1;
    rst.isbalanced = leftrst.isbalanced && rightrst.isbalanced
	    && abs(leftrst.depth - rightrst.depth) <= 1;
    return rst;
}
bool isBalanced(TreeNode* root) 
{
    if (root == NULL)
        return true;
    Node rst = helper(root);
    return rst.isbalanced;
}
```


#### <em class="icon-check"></em>  [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) (Hard) (Check Again!)
Idea: Define two variables, which are stored into a `struct` or `class`.
1. The max path sum of a tree `maxPathSum`,
2. The max path sum from root of a tree `maxRootPathSum`.
So we have a recursive equation about a root node and its children. Note the cases if subtree's result is less than 0. 
```cpp
class Node
{
public:
    Node() : maxPathSum(INT_MIN), maxRootPathSum(INT_MIN) {}
    Node(int x1, int x2) : maxPathSum(x1), maxRootPathSum(x2) {}
    int maxPathSum;
    int maxRootPathSum;
};
inline int max3(int a, int b, int c)
{
    return max(a, max(b, c));
}
Node Helper(TreeNode* root)
{
    if (root == NULL)
        return Node();
    Node leftrst = Helper(root->left);
    Node rightrst = Helper(root->right);
    Node rst;
    // maxPathSum locates in 
    // 1. left tree, or
    // 2. right tree or
    // 3. a path passing both trees, Note that in this case, 
	//    if subtree's result is < 0, drop it.
    rst.maxPathSum = max3
    (
        leftrst.maxPathSum,
        rightrst.maxPathSum,
        root->val + max(0, leftrst.maxRootPathSum) 
            + max(0, rightrst.maxRootPathSum)
    );
    // maxRootPathSum locates in
    // 1. left tree, or
    // 2. right tree
    // Note that the root itself must be included, 
    //    and if subtree's result is < 0, drop it.
    rst.maxRootPathSum = root->val 
        + max3(0, leftrst.maxRootPathSum, rightrst.maxRootPathSum);
    return rst;
}
int maxPathSum(TreeNode* root) 
{
    if (root == NULL)
        return 0;
    Node rst = Helper(root);
    return rst.maxPathSum;
}
```

### BFS

#### <em class="icon-check"></em>  [Populating Next Right Pointers in Each Node I / II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/) (Hard)
Idea: Level-order traversal, standard template. But O(n) space. So should solve it using level-order traversal + linked list.
```cpp
void connect(TreeLinkNode *root) 
{
    if (root == NULL)
        return;
    vector<TreeLinkNode> Dummy(2, TreeLinkNode(0));
    Dummy[0].next = root;
    TreeLinkNode *p, *prev;
    int level = 0;
    while (1)
    {
        p = Dummy[level % 2].next;
        if (p == NULL)
            break;
        Dummy[(level + 1) % 2].next = NULL;
        prev = &Dummy[(level + 1) % 2];
        while (p)
        {
            if (p->left)
            {
                prev->next = p->left;
                prev = p->left;
            }
            if (p->right)
            {
                prev->next = p->right;
                prev = p->right;
            }
            p = p->next;
        }
        level++;
    }
}
```

#### <em class="icon-check"></em>  [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
Idea: 
1. Standard Level-order traversal. This solution is trivial.
2. DFS. Define the `maxDepth` the tree is traversed, and `crtDepth` at each node, if `crtDepth > maxDepth` then it means at `crtDepth`, this node is first visited. Of course, keep `root->right` before `root->left` traversed.
```cpp
void dfs(TreeNode* root, int crtDepth, int& maxDepth, vector<int>& rst)
{
    if (crtDepth > maxDepth)
    {
        rst.push_back(root->val);
        maxDepth = crtDepth;
    }
    if (root->left == NULL && root->right == NULL)
        return;
    if (root->right)
        dfs(root->right, crtDepth + 1, maxDepth, rst);
    if (root->left)
        dfs(root->left, crtDepth + 1, maxDepth, rst);
}
vector<int> rightSideView(TreeNode* root) 
{
    vector<int> rst;
    if (root == NULL)
        return rst;
    int maxDepth = INT_MIN, crtDepth = 0;
    dfs(root, crtDepth, maxDepth, rst);
    return rst;
}
```

#### <em class="icon-check"></em>  [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
Idea: BFS seems faster, while another solution is Union-Find.

### Backtracking / Search - Advanced (DFS / BFS)

#### <em class="icon-check"></em>  [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)
Idea:
1. Initialize the hash tables for row, col and block;
2. There should be a flag indicating whether it's successful.
```cpp
bool dfs(vector<vector<char>>& bd, int i, int j, int row[9][9],
    int col[9][9], int blk[9][9])
{
    if (i == 9 && j == 0)
        return true;
    int blkIdx = i / 3 * 3 + j / 3;
    int nxti, nxtj;
    if (j == 8)
        nxti = i + 1, nxtj = 0;
    else
        nxti = i, nxtj = j + 1;
    if (bd[i][j] != '.')
    {
        bool rst = dfs(bd, nxti, nxtj, row, col, blk);
        if (rst)
            return true;
    }
    else
    {
        for (int k = 1; k <= 9; k++)
        {
            if (row[i][k - 1] || col[j][k - 1] || blk[blkIdx][k - 1])
                continue;
            row[i][k - 1] = col[j][k - 1] = blk[blkIdx][k - 1] = 1;
            bd[i][j] = k + '0';
            bool rst = dfs(bd, nxti, nxtj, row, col, blk);
            if (rst)
                return true;
            bd[i][j] = '.';
            row[i][k - 1] = col[j][k - 1] = blk[blkIdx][k - 1] = 0;
        }
    }
    return false;
}
void solveSudoku(vector<vector<char>>& board) 
{
    int row[9][9] = {0}, col[9][9] = {0}, blk[9][9] = {0};
    for (int i = 0; i < 9; i++)
    {
        for (int j = 0; j < 9; j++)
        {
            char c = board[i][j];
            if (c == '.')
                continue;
            row[i][c - '1'] = col[j][c - '1'] 
                = blk[i / 3 * 3 + j / 3][c - '1'] = 1;
        }
    }
    dfs(board, 0, 0, row, col, blk);
}
```

#### <em class="icon-check"></em>  [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/) (Classic!!!)
Idea: DFS string `s`: divide `s` into two substrings `s1`, `s2`. If `s1` is palindrome, then DFS `s2`. otherwise continue. 
```cpp
void dfs(string& s, int start, vector<string>& crtrst, 
    vector<vector<string>>& finalrst, const vector<vector<int>>& P)
{
    int n = s.size();
    if (start == n)
    {
        finalrst.push_back(crtrst);
        return;
    }
    for (int i = start; i < n; i++)
    {
        bool isPalindrome = P[start][i];
        if (!isPalindrome)
            continue;
        crtrst.push_back(s.substr(start, i - start + 1));
        dfs(s, i + 1, crtrst, finalrst, P);
        crtrst.pop_back();
    }
}
void gen_is_palindrome(string& s, vector<vector<int>>& P)
{
    int n = P.size();
    for (int i = 0; i < n; i++)
        P[i][i] = true;
    for (int i = 0; i < n - 1; i++)
        P[i][i + 1] = s[i] == s[i + 1];
    int i = 0, j = 2, k = 3;
    while (j < n)
    {
        P[i][j] = P[i + 1][j - 1] && s[i] == s[j];
        i++;
        j++;
        if (j == n)
        {
            i = 0;
            j = k++;
        }
    }
}
vector<vector<string>> partition(string s) 
{
    vector<vector<string>> finalrst;
    vector<string> crtrst;
    int n = s.size();
    if (n == 0)
        return finalrst;
    vector<vector<int>> P(n, vector<int>(n, 0));
    gen_is_palindrome(s, P);
    dfs(s, 0, crtrst, finalrst, P);
    return finalrst;
}
```

#### <em class="icon-check"></em>  [Combinations](http://www.lintcode.com/en/problem/combinations/)
Idea: For $C_n^k$, we use `k` to denote how many numbers have been added. Once `k == 0`, we push the `crtrst` into `finalrst`; 
```cpp
void dfs(int s, int e, int k, vector<int>& crtrst, 
    vector<vector<int>>& finalrst)
{
    if (k == 0)
    {
        finalrst.push_back(crtrst);
        return;
    }
    for (int i = s; i <= e; i++)
    {
        crtrst.push_back(i);
        dfs(i + 1, e, k - 1, crtrst, finalrst);
        crtrst.pop_back();
    }
}
vector<vector<int>> combine(int n, int k) 
{
    vector<vector<int>> finalrst;
    vector<int> crtrst;
    if (n == 0 || k == 0 || k > n)
        return finalrst;
    dfs(1, n, k, crtrst, finalrst);
    return finalrst;
}
```

#### <em class="icon-check"></em>  [Combination Sum](https://leetcode.com/problems/combination-sum/) / [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/) / [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/) (Classic!!!)
Idea: DFS Template
> DFS: define a `crtrst`, `finalrst`. The stop condition is when the condition satisfied, `crtrst` will be added to `finalrst`.

```cpp
// This is a general template:
void dfs(const vector<int>& cds, int s, int tar, 
	vector<int>& crtrst, vector<vector<int>>& finalrst)
{
	// Stop Contidion:
	if (tar == 0)
	{
		finalrst.push_back(crtrst);
		return;
	}
	// If Not to Stop Condition:
	for (int i = s; i < cds.size(); i++)
	{
		// tar - cds[i] < 0 means we do not need to calculate the rest elements since they must be larger than the current one.
		if (tar - cds[i] < 0)
			break;
		// repeated numbers need to be counted once.
		if (i > s && cds[i] == cds[i - 1]) 
			continue;
		crtrst.push_back(cds[i]);
		dfs(cds, s, tar - cds[i], crtrst, finalrst);
		crtrst.pop_back();
	}
}

vector<vector<int>> CombinationSum(vector<int>& cds, int tar)
{
	// Given an array of candidates, 
	// Search the combinations such that their sum is tar.
	// 1. Define crtrst, finalrst:
	vector<vector<int>> finalrst;
	vector<int> crtrst;
	sort(cds.begin(), cds.end()); // make the results ascending order
	// 2. DFS: dfs the cds with starting index s, searching it
	// to every possible corner, and add the satisfied rst to finalrst;
	int s = 0;
	dfs(cds, s, tar, crtrst, finalrst);
}
```

```cpp
void dfs(int n, int k, int s, vector<int>& crtrst, 
    vector<vector<int>>& finalrst, vector<int>& visited)
{
    if (n == 0 && k == 0)
    {
        finalrst.push_back(crtrst);
        return;
    }
    if (n == 0 && k != 0 || n != 0 && k == 0)
        return;
    for (int i = s; i <= 9; i++)
    {
        if (visited[i])
            continue;
        if (n - i < 0)
            break;
        crtrst.push_back(i);
        visited[i] = 1;
        dfs(n - i, k - 1, i + 1, crtrst, finalrst, visited);
        visited[i] = 0;
        crtrst.pop_back();
    }
}
vector<vector<int>> combinationSum3(int k, int n) 
{
    vector<vector<int>> finalrst;
    vector<int> crtrst;
    vector<int> visited(10, 0);
    if (k == 0 || n < k)
        return finalrst;
    dfs(n, k, 1, crtrst, finalrst, visited);
    return finalrst;
}
```


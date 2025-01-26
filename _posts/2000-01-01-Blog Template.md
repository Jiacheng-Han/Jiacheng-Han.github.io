---
title: 'Blog Template'
date: 2000-01-01
permalink: /posts/2000/01/Blog-Template/
tags:
  - template
---

# 非一边过

* [236. 二叉树的最近公共祖先 - 力扣（LeetCode）](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

内存超限，给每一个节点的父节点都用栈存起来了。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
void check(TreeNode* node, map<TreeNode*, stack<TreeNode*>> &mp){
    if(node->left!=nullptr){
        mp[node->left] = mp[node];
        mp[node->left].push(node->left);
        check(node->left, mp);
    }
    if(node->right!=nullptr){
        mp[node->right] = mp[node];
        mp[node->right].push(node->right);
        check(node->right, mp);
    }
    return;
} 

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* r = root;
        map<TreeNode*, stack<TreeNode*>> mp;
        check(root, mp);
        unordered_set<TreeNode*> s;
        while(!mp[p].empty()) {
            s.insert(mp[p].top());
            mp[p].pop();
        }
        while(!mp[q].empty()){
            if(s.count(mp[q].top())) return mp[q].top();
            mp[q].pop();
        }
        return root;
    }
};
```

稍微修改了一点，看到题目中的键值各不相同，所以只存父节点指针。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    map<int, TreeNode*> mp;

    void check(TreeNode* node){
        if(node->left!=nullptr){
            mp[node->left->val] = node;
            check(node->left);
        }
        if(node->right!=nullptr){
            mp[node->right->val] = node;
            check(node->right);
        }
        return;
    } 

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* r = root;
        check(root);
        unordered_set<TreeNode*> s;
        TreeNode *i = p, *j = q;
        while(i!=nullptr) {
            s.insert(i);
            i = mp[i->val];
        }
        while(j!=nullptr){
            if(s.count(j)) return j;
            j = mp[j->val];
        }
        return root;
    }
};
```

* [739. 每日温度 - 力扣（LeetCode）](https://leetcode.cn/problems/daily-temperatures/)

没有想到非暴力做法，使用单调栈

```python
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> ans(n);
        stack<int> s;
        for(int i = 0; i < n; i++){
            while(!s.empty() && temperatures[i] > temperatures[s.top()]){
                ans[s.top()] = i - s.top();
                s.pop();
            }
            s.push(i);
        }
        return ans;
    }
};
```

* [207. 课程表 - 力扣（LeetCode）](https://leetcode.cn/problems/course-schedule/description/?envType=problem-list-v2&envId=2cktkvj)

这道题的本质是判断是否有环，但写成了是否通路

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<bool> a(numCourses, 1), b(numCourses);
        map<int, vector<int>> tree;
        for(int i = 0; i < prerequisites.size(); i++){
            tree[prerequisites[i][1]].emplace_back(prerequisites[i][0]);
            a[prerequisites[i][0]] = 0;
        }
        for(int i = 0; i < numCourses; i++){
            if(a[i]){
                queue<int> q;
                q.push(i);
                while(!q.empty()){
                    int top = q.front();
                    b[top] = 1;
                    for(auto j : tree[top]) 
                        q.push(j);
                    q.pop();
                }
            }
        }
        for(int i = 0; i < numCourses; i++){
            if(!b[i]) return 0;
        }
        return 1;
    }
};
```

* [155. 最小栈 - 力扣（LeetCode）](https://leetcode.cn/problems/min-stack/)[155. 最小栈 - 力扣（LeetCode）](https://leetcode.cn/problems/min-stack/)

没有考虑到小根堆和栈的stl的`pop`弹出是不一样的

```cpp
class MinStack {
    stack<int> s;
    priority_queue<int> p;
public:
    MinStack() {

    }

    void push(int val) {
        p.push(val);
        s.push(val);
    }

    void pop() {
        p.pop();
        s.pop();
    }

    int top() {
        return s.top();
    }

    int getMin() {
        return p.top();
    }
};
```

需要一个辅助栈记录每一个元素进入时，当前栈内的最小值。

```cpp
class MinStack {
    stack<int> s1, s2;
public:
    MinStack() {
        s2.push(INT_MAX);
    }

    void push(int val) {
        s1.push(val);
        s2.push(min(val, s2.top()));
    }

    void pop() {
        s1.pop(), s2.pop();
    }

    int top() {
        return s1.top();
    }

    int getMin() {
        return s2.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

# 一边过

* [160. 相交链表 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        set<ListNode*> s;
        for(ListNode *i = headA; i != NULL; i = i->next){
            s.insert(i);
        }
        int len = s.size();
        for(ListNode *i = headB; i != NULL; i = i->next){
            len = s.size();
            s.insert(i);
            if(len == s.size()) return i; 
        }
        return NULL;
    }
};
```

* [234. 回文链表 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-linked-list/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* i = head;
        vector<int> v(1e6);
        int h = 1, t; 
        while(i!=nullptr){
            v[h++] = i->val;
            i = i -> next;
        }
        t = h - 1;
        h = 1;
        while(h < t){
            if(v[h++]!=v[t--]) return false;
        }
        return true;
    }
};
```

* [226. 翻转二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/invert-binary-tree/)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void f(TreeNode* root){
        if(root == NULL){
            return ;
        }
        f(root->left);
        f(root->right);
        TreeNode node = TreeNode(root->val, root->right, root->left);
        root->left = node.left;
        root->right = node.right;
    }

    TreeNode* invertTree(TreeNode* root) {
        f(root);
        return root; 
    }
};
```

* [215. 数组中的第K个最大元素 - 力扣（LeetCode）](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

大根堆

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, less<int>> p;
        for(auto i : nums) p.push(i);
        for(int i = 1; i < k; i++) p.pop();
        return p.top();
    }
};
```

* [206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/)

但我这个写法不是直接反转原本链表，而是重建了一个新的链表，原本链表没有删除。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *i = head, *temp = nullptr, *root = nullptr;
        while(i != NULL){
            root = new ListNode(i->val, temp);
            temp = root; 
            i = i->next;
        }
        return root;
    }
};
```

直接反转原来的节点。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *pre = nullptr, *curr = head;
        while(curr){
            ListNode *next = curr->next; //记录下一个节点的位置
            curr->next = pre; //修改当前节点
            pre = curr;
            curr = next;
        }
        return pre;
    }
};
```

* [200. 岛屿数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/)

经典BFS搜索

```cpp
class Solution {
public:
    bool vis[300][300];
    int next[4][2] = {{-1,0},{1,0},{0,1},{0,-1}};
    int numIslands(vector<vector<char>>& grid) {
        int ans = 0, h = grid.size(), w = grid[0].size();
        for(int i = 0; i < h; i++){
            for(int j = 0; j < w; j++){
                if(vis[i][j] || grid[i][j]=='0') continue;
                queue<pair<int,int>> q;
                q.push({i,j});
                while(!q.empty()){
                    pair<int,int> p = q.front();
                    q.pop();
                    for(int k = 0; k < 4; k++){
                        if(p.first + next[k][0] < h && p.second + next[k][1] < w && p.first + next[k][0] >= 0 && p.second + next[k][1] >= 0){
                            if(grid[p.first + next[k][0]][p.second + next[k][1]]=='1' && !vis[p.first + next[k][0]][p.second + next[k][1]]){
                                q.push({p.first + next[k][0], p.second + next[k][1]});
                                vis[p.first + next[k][0]][p.second + next[k][1]] = 1;
                            }
                        }
                    }
                }
                ans++;
            }
        }
        return ans;
    }
};
```

* [198. 打家劫舍 - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber/)

经典dp

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int len = nums.size(), ans = 0;
        vector<int> dp(len, 0);
        for(int i = 0; i < len; i++){
            dp[i] = nums[i];
            for(int j = 0; j < i - 1; j++){
                dp[i] = max(dp[j] + nums[i], dp[i]);
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

写了一个很蠢的做法

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int len = nums.size(), temp = nums[0], sum = 0;
        for(auto i : nums){
            if(temp != i){
                sum = 0;
                temp = i;
            }
            sum++;
            if(sum>len/2) return i;
        }
        return 0;
    }
};
```

其实可以直接利用众数，直接返回

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};
```

学了一下Boyer-Moore 算法，O(N)的时间复杂度，O(1)的空间复杂度。

        如果我们把众数记为`+1`，把其他数记为`−1`，将它们全部加起来，显然和大于 `0`，即如果一个数组有大于一半的数相同，那么任意删去两个不同的数字，新数组还是会有相同的性质。

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int a = nums[0], sum = 0;
        for(int num : nums){
            if(num == a) sum++;
            else {
                sum--;
                if (sum < 0){
                    a = num;
                    sum = 1;
                }
            }
        }
        return a;
    }
};
```

* [238. 除自身以外数组的乘积 - 力扣（LeetCode）](https://leetcode.cn/problems/product-of-array-except-self/)

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> a(nums.size()), b(nums.size()), ans(nums.size());
        a[0] = nums[0], b[nums.size() - 1] = nums[nums.size() - 1];
        for(int i = 1; i < nums.size(); i++){
            a[i] = a[i-1] * nums[i];
            b[nums.size() - 1 - i] = b[nums.size() - i] * nums[nums.size() - 1 - i];
        }
        ans[0] = b[1], ans[nums.size() - 1] = a[nums.size() - 2];
        for(int i = 1; i < nums.size() - 1; i++){
            ans[i] = a[i - 1] * b[i + 1];
        }
        return ans;
    }
};
```

变成O(1)的空间复杂度的话，就把反方向的乘法直接一个一个乘进ans数组里。

## 更新节点

* 2025-01-24

* 2025-01-25

* 2025-01-26

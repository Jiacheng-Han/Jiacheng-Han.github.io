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


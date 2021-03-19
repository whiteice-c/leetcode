输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

```
输入：head = [1,3,2]
输出：[2,3,1]
```



思路一：栈

顺序遍历链表，依次将节点值压入栈中，结束遍历后，依次出栈并加入vector中即为所求。

```c++
vector<int> reversePrint(ListNode* head) {
        stack<int> stk;
        vector<int> ans;

        while( head != nullptr ){
            stk.push(head->val);
            head = head-> next;
        }
        while( !stk.empty() ){
            ans.push_back(stk.top());
            stk.pop();
        }

        return ans;
    }
```

**复杂度分析：**

时间复杂度：$O(n)$ 

空间复杂度：$O(n)$ 



思路二：暴力

直接插入，使用insert方法。

```c++
vector<int> reversePrint(ListNode* head) {
        vector<int> ans;

        while( head != nullptr ){
            ans.insert(ans.begin(),head->val);
            head = head->next;
        }

        return ans;
    }
```

**复杂度分析：**

时间复杂度：$O(n^2) $ ,插入首位的时间复杂度为$ O(n) $

空间复杂度：$O(n)$ 



思路三：递归方法

递归遍历链表，回溯时插入到结果中。

```c++
vector<int> ans;
    vector<int> reversePrint(ListNode* head) {
        if( !head ) return vector<int> (0);
        reversePrint(head->next);
        ans.push_back(head->val);
        return ans;
    }
```

**复杂度分析：**

时间复杂度：$O(n)$ 

空间复杂度：$O(n)$ 


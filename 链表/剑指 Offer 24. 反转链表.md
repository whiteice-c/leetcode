定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```



思路一：

我们创建一个哑结点作为新的头节点，然后遍历给定链表元素，每次将当前元素插入到哑节点之后，结束后返回哑节点的下一个节点即可。

```c++
ListNode* reverseList(ListNode* head) {
        ListNode* dummy =  new ListNode(-1);
        dummy->next = nullptr;
         
        while(head != nullptr){
            ListNode* temp = head->next;
            head->next = dummy->next;
            dummy->next = head;
            head = temp;
        }
        return dummy->next;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n) $ ,n为链表的长度

空间复杂度：$ O(1) $  



思路二：原地操作

我们创建两个指针分别指向相邻的两个节点，顺序遍历，每次将他们之间的指针方向修改为反方向，当尾指针遍历结束时，尾指针指向的元素就是新的头节点。

```c++
ListNode* reverseList(ListNode* head) {
        if( head == nullptr || head->next == nullptr ) return head;
        ListNode* pre = head, *cur = head->next;
         
        pre->next = nullptr;
        while(cur != nullptr){
            ListNode *temp = cur->next;
            cur->next = pre;
            pre = cur;
            if(temp) cur = temp;
            else return cur;
        }
        return cur;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n) $ ,n为链表的长度

空间复杂度：$ O(1) $  
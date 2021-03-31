给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 *没有重复出现* 的数字。

**示例 1:**

```
输入: 1->2->3->3->4->4->5
输出: 1->2->5
```



思路一：双指针遍历

由于是排序链表，那么相同的元素一定是连着的，因此我们用两个指针pre，cur分别指向前后两个节点，然后我们先用cur指针遍历节点，若$cur->val = cur->next->val$ ，那么说明我们遇到了相同元素，此时我们保持pre不变（指向第一次遇到相同元素的前节点），继续向后遍历，直到遇到$cur->val \neq cur->next->val $ ,  然后用 $ pre->next = cur->next $ 即可删除所有的重复元素。若未遇到重复元素，那么我们将两个指针同时后移即可。为了便于起始初始化和最后返回头节点，我们需要定义一个哑节点。

```c++
ListNode* deleteDuplicates(ListNode* head) {
        if(head == nullptr || head->next == nullptr) return head;

        ListNode *dummy = new ListNode(-1);
        dummy->next = head;
        ListNode *pre = dummy,*cur = head;

        while( cur && cur->next ){
            if( cur->val == cur->next->val ){
                while( cur->next && cur->val == cur->next->val ){
                    cur = cur->next;
                }
                pre->next = cur->next;
                cur = cur->next;
            }else{
                cur = cur->next;
                pre = pre->next;
            }   
        }

        ListNode *newHead = dummy->next;
        delete dummy;
        return newHead;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n) $ ，n为链表节点个数。

空间复杂度：O(n)。
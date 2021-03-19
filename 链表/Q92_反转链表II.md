反转从位置 *m* 到 *n* 的链表。请使用一趟扫描完成反转。

**说明:**
1 ≤ *m* ≤ *n* ≤ 链表长度。



```
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```



思路：

我们使用一个计数来找到我们反转部分的起始p和结束位置q。然后将p到q的节点进行反转，然后再将这个反转的子链表接回去即可，这个操作需要我们事先保存好起始p节点、p的前节点pre、q节点、q的后继节点rear，然后通过下面操作将反转好的子链表接回去。

```
pre->next = q;  //反转后的子尾节点变为子头节点
p->next = rear; //反转后的子头节点变为子尾节点
```

由于我们需要记录前驱节点，我们在给定链表前加一个哑节点，以统一操作。

```c++
ListNode* reverseBetween(ListNode* head, int left, int right) {
        if( head -> next == nullptr || left == right ) return head;
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        int count = 1;
        ListNode *pre = dummy;

        while( count != left){//找到反转的起始节点
            pre = pre->next;
            count++;
        }

        ListNode *subHead = pre, *subTail = pre->next, *cur = pre->next;//保存反转部分的头节点，反转后变为尾节点需指向原链表的尾部，反转部分的前驱节点反转后需指向新的头节点
        pre = pre->next;
        cur = cur->next;
        while( count != right ){//反转子链表
            ListNode *temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
            count++;
        }
        subHead->next = pre;//将子链表接回原链表
        subTail->next = cur;

        ListNode *newHead = dummy->next;
        delete dummy;
        return newHead;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n) $

空间复杂度：O(1)
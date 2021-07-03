输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。



```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



<b>归并思路</b>

利用归并思想，比较两个链表元素，每次将较小元素合并到新链表的末尾。

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := new(ListNode)
    cur := dummy

    for{
        if l1 == nil || l2 == nil{
            break
        }
        if l1.Val <= l2.Val{
            cur.Next = l1 
            l1 = l1.Next
        }else{
            cur.Next = l2
            l2 = l2.Next
        }
        cur = cur.Next
    }

    if l1 == nil {
        cur.Next = l2
    }else{
        cur.Next = l1
    }

    return dummy.Next
}
```



<b>递归写法</b>

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 == nil {
        return l2
    }
    if l2 == nil {
        return l1 
    }

    if l1.Val <= l2.Val{
        l1.Next = mergeTwoLists(l1.Next,l2)
        return l1
    }else{
        l2.Next = mergeTwoLists(l1,l2.Next)
        return l2
    }
}
```


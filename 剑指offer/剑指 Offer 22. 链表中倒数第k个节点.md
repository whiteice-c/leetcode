输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。



```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```



<b>思路一：计数</b>

我们遍历两次链表，第一次记录链表的长度，第二次我们直接返回第 n-k+1个节点即可。

```go
func getKthFromEnd(head *ListNode, k int) *ListNode {
    cnt := 0
    tmp := head

    for tmp != nil{
        cnt++
        tmp = tmp.Next
    }

    pos := cnt-k 
    for i:= 1 ; i <= pos; i++{
        head = head.Next
    }
    return head 
}
```





<b>思路二：双指针</b>

我们可以使用双指针在一次遍历的情况下找到倒数第k个节点，具体的，我们让快慢指针指向的节点之间的距离相距为k，这样当快指针遍历结束指向尾部空指针时，慢指针指向的节点就是所求节点。

```go
func getKthFromEnd(head *ListNode, k int) *ListNode {
    pre,rear := head,head
    for i := 1; i <= k; i++{
        pre = pre.Next
    }

    for pre != nil {
        pre = pre.Next
        rear = rear.Next
    }
    return rear 
}
```


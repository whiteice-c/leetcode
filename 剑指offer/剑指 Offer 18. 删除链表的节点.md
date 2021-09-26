给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

```
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```



<b>思路：双指针</b>

建立一个哑节点，使用前后两个指针遍历链表，若遇到目标节点，直接删除即可。

```go
func deleteNode(head *ListNode, val int) *ListNode {
    dummy := new(ListNode)
    dummy.Next = head
    pre := dummy 

    for {
        if head == nil {
            break
        }
        if head.Val == val {
            pre.Next =head.Next
            return dummy.Next 
        }else{
            pre = pre.Next
            head = head.Next
        }
    }

    return head
}
```


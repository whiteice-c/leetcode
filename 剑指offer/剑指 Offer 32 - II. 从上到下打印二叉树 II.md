从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

```
   3

   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```


提示：

节点总数 <= 1000



<b>思路一：广度优先遍历</b>

使用广度优先遍历逐层遍历所有元素，具体的，每次先计算当前队列中的节点个数，然后将这些节点一次遍历完毕，遍历时并将每个节点的子节点加入到队列，当队列为空，我们完成了逐层遍历。

```go
func levelOrder(root *TreeNode) [][]int {
    if root == nil {
        return [][]int{}
    }
    m := []*TreeNode{root}

    ans := make([][]int,0)
    for len(m) > 0 {
        n := len(m)
        subans := make([]int,0)
        for i := 0; i < n ; i++ {
            node := m[i]
            subans = append(subans,node.Val)
            if node.Left != nil {
                m = append(m,node.Left)
            }
            if node.Right != nil {
                m = append(m,node.Right)
            }
        }

        ans = append(ans,subans)
        m = m[n:len(m)]
    }

    return ans
}
```


输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树 [3,9,20,null,null,15,7]，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。



 <b>思路：深度优先遍历</b>

通过深度优先遍历记录高度，并返回左右分支的最大深度

```go
func dfs(root *TreeNode , h int) int{
    if root == nil{
        return h 
    }

    h1 := dfs(root.Left,h+1)
    h2 := dfs(root.Right,h+1)
    if h1 >= h2 {
        return h1
    }else{
        return h2
    }
}

func maxDepth(root *TreeNode) int {
    return dfs(root,0)
}
```


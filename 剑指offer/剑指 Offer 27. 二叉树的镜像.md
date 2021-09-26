请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

镜像输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**示例 1：**

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```



<b>思路：深度优先遍历</b>

二叉树的镜像特点为对于每一个节点，其左右孩子都互相交换位置，因此我们只需要递归遍历节点，交换其左右孩子即可。

```go
func mirrorTree(root *TreeNode) *TreeNode {
    if root == nil {
        return root 
    }

    if root.Left != nil && root.Right != nil{
        tmp := root.Left
        root.Left = root.Right
        root.Right = tmp 
        mirrorTree(root.Left)
        mirrorTree(root.Right)
    }else if root.Left != nil{
        root.Right = root.Left
        root.Left = nil 
        mirrorTree(root.Right)
    }else if root.Right != nil{
        root.Left = root.Right
        root.Right = nil 
        mirrorTree(root.Left)
    }
    return root 
}
```


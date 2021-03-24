输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。



```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```



思路一：递归

前序递归遍历特点：[ 根， [左子树节点]， [右子树节点] ]

中序递归遍历特点：[  [左子树节点]，根， [右子树节点] ]

如果我们使用前序递归构建二叉树，那么我们需要知道当前节点作为根节点时，它的左子树和右子树有哪些节点(即找到分割点)，而这个恰好可以通过中序遍历找到：定位到根节点在中序遍历中的位置，其恰好将左右子树分割，进一步的我们也就能够获取到左右子树节点的个数，从而回到先序遍历中选取对应个数的节点依次建树。因此，我们事先用一个map将所有的根节点在中序遍历中的位置记录下来，然后使用递归建树即可。

```c++
unordered_map<int,int> rid;

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();

        for( int i = 0; i < n; ++i ) rid[inorder[i]] = i;
        return dfs(preorder,inorder,0,n-1,0,n-1);
    }

    TreeNode* dfs(vector<int>& preorder, vector<int>& inorder,int preorder_left, int preorder_right ,int inorder_left,int inorder_right){

        if (preorder_left > preorder_right) {
            return nullptr;
        }
        
        int rootVal = preorder[preorder_left]; //确定根节点
        int root_index = rid[rootVal];  //确定根节点在中序遍历中的位置
        int leftTree_size = root_index - inorder_left; //确定左子树节点个数

        TreeNode *root = new TreeNode(rootVal);
        //依次从先序遍历中获取leftTree_size个结点作为左子树建树
        root->left = dfs(preorder,inorder,preorder_left+1,preorder_left+leftTree_size,inorder_left, root_index-1);
        //依次从先序遍历中获取剩余节点对右子树建树
        root->right = dfs(preorder,inorder,preorder_left+leftTree_size+1,preorder_right,root_index+1, inorder_right);
        return root;
    }   
```

**复杂度分析：**

时间复杂度：$O(n)$ 

空间复杂度：$O(n)$ 



思路二：栈

首先我们需要了解前序遍历的一个特点：对于前序遍历序列任意相邻的两个节点$[u,v]$ ,它们只可能存在两种关系：

1. v是u的左孩子

2. v是u或u的某个祖先的右孩子

   ```
         3                  
        / \                 前序遍历：[3,9,8,20,15,7]
       9  20                8是9的左孩子，20是8的祖先的右孩子
     /   /  \				 中序遍历：[8,9,3,15,20,7]
    8  15   7          
   ```

我们定义两个指针p,q，其中p指向前序遍历序列当前遍历的节点，q指向[ 中序遍历节点且满足当前指向是p在沿着当前节点到左孩子的最远节点] （例如上例中，初始p指向3,q指向8,节点8就是3沿着左节点路径能到达的最远节点）。

现在我们用指针p依次遍历前序遍历中的节点，并将过程中未考其右孩子的节点全部压入栈中。我们结合中序的指针q可以很容易判断出来：若指针p指向的结点与q指向的节点不同，则说明沿着左孩子路径上还存在节点，我们继续建立左孩子子树即可，且因一直未考虑右孩子，我们将p遍历的节点压入栈中。

当p,q指向的节点相同时，说明p指向的当前节点无左孩子，其下一个节点是某个祖先的右孩子或当前节点的右孩子，我们需要确认是哪个节点的右孩子并将它建树。这时我们就需要从栈中寻找了。首先我们需要知道另一个特点：当p,q指向节点相同时，那么p遍历的这一条左路径的节点序列恰好在中序遍历中呈倒序排列：

```
      3                               p
     / \                 前序遍历：[3,9,8,20,15,7]
    9  20                			
  /   /  \				 中序遍历：[8,9,3,15,20,7]
 8  15   7  			          q
 
 						当p,q都指向节点8时，前序遍历过程中遍历的节点为 3->9->8，恰好对应中序遍历的						   倒序 8->9->3
```

因此，我们此时依次出栈元素和q指向元素比较，若相等则出栈，q指针后移一位。直到此时的栈顶元素与中序指针q指向的元素不同或是栈顶为空，那么p指向的下一节点就是最新出栈节点的右孩子。

```c++
stack<TreeNode*> stk;

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    int n = preorder.size(), p = 1 , q = 0;
    if( n == 0 ) return nullptr;
    TreeNode *head = new TreeNode(preorder[0]);
    stk.push(head);

    while( p < n ){
        if( preorder[p-1] != inorder[q] ){  //有左孩子
            TreeNode *node = new TreeNode(preorder[p]);
            stk.top()->left = node;
            stk.push(node);
        }
        else{ //找右孩子的父亲
            TreeNode *node = new TreeNode(preorder[p]);
            TreeNode *temp;
            while(!stk.empty() && stk.top()->val == inorder[q]){
                temp = stk.top();
                stk.pop();
                q++;
            }
            temp->right = node;
            stk.push(node);
        }
        p++;
    }

    return head;
}
```

**复杂度分析：**

时间复杂度：$O(n)$ 

空间复杂度：$O(n)$ 


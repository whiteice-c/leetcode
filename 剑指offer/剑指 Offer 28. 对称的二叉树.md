请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

```
   1
   / \
  2   2
   \   \
   3    3
```



<b>思路：层次遍历</b>

每次遍历一层元素，用一个字符串表示该层的元素，其中空节点用0表示，然后判断字符串是否对成即可。

```c++
bool issymmetry(string s){
        int l = 0, r = s.size()-1;
        while(l <= r){
            if(s[l] != s[r]) return false;
            l++;
            r--;
        }
        return true;
    }

    bool isSymmetric(TreeNode* root) {
        if(!root) return true;
        queue<TreeNode*> q;
        q.push(root);

        bool flag = true; //记录下一层是否存在飞空节点
        while(!q.empty() && flag){
            flag = false;
            int sz = q.size();
            string s;
            
            while(sz--){
                TreeNode *node = q.front();
                q.pop();
                if(node == nullptr){
                    s += '0';
                    continue;
                } 
                s += node->val;
                if(node->left){
                    q.push(node->left);
                    flag = true;
                }
                else q.push(nullptr);
                if(node->right){
                    q.push(node->right);
                    flag = true;
                }
                else q.push(nullptr);
            }
            if(!issymmetry(s)) return false;
        }

        return true;
    }
```


给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。



```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```



<b>思路： 回溯</b>

由于要获取到所有的子集，也就是搜索路径 问题，我们使用dfs遍历即可。由于不能包含重复子集，因此我们可以先对给定数组进行排序，然后在搜索过程中，回溯替换当前节点时，若替换的节点值未改变，那么我们不将它加入路径。

```c++
	vector<vector<int>> ans;

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        int n = nums.size();
        vector<int> subans;
        ans.push_back({});
        sort(nums.begin(),nums.end());
        backtrack(nums,subans,0,n);

        return ans;
    }

    void backtrack(vector<int>& nums,vector<int>& subans,int index,int n){
        if( index == n ) return;

        for( int i = index ; i < n; ++i ){
            if( i != index && nums[i] == nums[i-1] ) continue;
            subans.push_back(nums[i]);
            ans.push_back(subans);
            backtrack(nums,subans,i+1,n);
            subans.pop_back();
        }
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n*2^n) $ ,最差情况下，每个数作为根节点都会生成$ 2^n $ 个子节点的组合。

空间复杂度：$O(n) $ ， 在最差情况下需要递归$ O(n) $层。



<b>思路二：迭代</b>

同问题Q78,我们将每个元素的状态表示为对应的二进制位，然后遍历转换子集即可。这里需要考虑重复元素问题，同样的额，我们先排序，然后观察其产生重复组合的规律：例如 $n = 3, nums=[1,2,2]$ 

| 状态 | 对应子集 | 数值 |
| ---- | -------- | ---- |
| 000  | {}       | 0    |
| 001  | {1}      | 1    |
| 010  | {2}      | 2    |
| 011  | {1,2}    | 3    |
| 100  | {2}      | 4    |
| 101  | {1,2}    | 5    |
| 110  | {2,2}    | 6    |
| 111  | {1,2,2}  | 7    |

 可以发现 $ 010,100$ 和 $ 011, 101 $ 对应的子集相同，原因就是bit1,bit2具有相同的元素，当其中一位被选中，而另一位未被选中时，就会出现相同子集，因此我们需要排除这种情况。

```c++
vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        int n = nums.size();
        vector<int>subans;
        vector<vector<int>> ans;

        sort(nums.begin(),nums.end());

        for( int mask = 0; mask < (1<<n); ++mask ){ //遍历每个状态
            subans.clear();
            bool flag = true;
            for(int i = 0 ; i < n; ++i){//将状态转换为对应子集
                if( mask & (1 << i) ){
                    if( i > 0 && !(mask & (1 << (i-1))) && nums[i] == nums[i-1] ){ //相同则不更新
                        flag = false;
                        break;
                    }
                    subans.push_back(nums[i]);
                }
            }   
            if(flag) ans.push_back(subans);
        }

        return ans;
    }
```



```c++
vector<vector<int>> subsets(vector<int>& nums) {
        vector<int>subans;
        vector<vector<int>> ans;

        int n = nums.size();

        for( int mask = 0; mask < (1<<n); ++mask ){ //遍历每个状态
            subans.clear();
            for(int i = 0 ; i < n; ++i)  //将状态转换为对应子集
                if( mask & (1 << i) ) subans.push_back(nums[i]);
            ans.push_back(subans);
        }

        return ans;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n*2^n) $ ,共有$ 2^n $ 个状态，每个状态需要遍历所有元素。

空间复杂度：$O(n) $ 。


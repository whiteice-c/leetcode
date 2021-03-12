给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

说明：

所有数字（包括目标数）都是正整数。
解集不能包含重复的组合。 

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```



思路：搜索算法

与Q39组合总和思路相同，不同点是：

1. 数组中存在重复元素
2. candidates 中的每个数字在每个组合中只能使用一次。

因此我们作出两点调整即可，当当前位置确定选择candidates[i] 后，我们需要从i+1开始选取下一个元素而不是i。另外，由于存在重复元素，我们需要先对数组排序，然后在搜索元素时，若当前位置想要修改元素，需要先判断下一个数组中的元素是否与当前元素相同，若相同，那么我们直接跳过，不做判断。

```c++
vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        int len = candidates.size();
        vector<vector<int>> ans;
        vector<int> subans;
        backtrack(candidates,ans,subans,len,0,target);
        return ans;
    }

    void backtrack(vector<int>& candidates, vector<vector<int>>& ans,vector<int>& subans,int len ,int idx, int target ){
        if( 0 == target ){
            ans.push_back(subans);
            return;
        }
        if( target < 0 ) return;
        if( idx == len ) return; //超出范围，返回

        for( int i = idx; i < len; ++i ){
            if( i > idx && candidates[i-1] == candidates[i]) continue; //同一位置选取遇到重复元素则跳过
            subans.push_back(candidates[i]);//搜索第i个元素选取当前数的组合
            backtrack(candidates,ans,subans,len,i+1,target-candidates[i]);
            subans.pop_back(); //回溯
        }
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n*2^n) $ ,最差情况下，每个数作为根节点都会生成$ 2^n $ 个子节点的组合。

空间复杂度：$O(target) $ ， 在最差情况下需要递归$ O(\textit{target}) $层。
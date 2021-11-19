给定一个可包含重复数字的序列 `nums` ，**按任意顺序** 返回所有不重复的全排列。

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`



思路：回溯

​	我们可以把目标的初始想象为一个长度为n的空位置，我们利用给定数组对该目标填充数字，每个数字只能填充一次，这样我们就能够得到所有的全排列。但由于数组中存在重复的元素，因此会产上大量重复的组合，我们考虑一个简单的例子，看看如何排除掉重复的组合。求数组 $[1,1,2]$的全排列：

​	为了区别数组相同元素，我们改为$[1a,1b,2]$ ，我们利用标记数组+递归填充，会得到这样的重复组合：$[1a,1b,2]$ 和 $ [1b,1a,2] $ ，实际上二者是相同的，为此我们需要做一些条件限制，首先填充位置我们通过标记数组保证了每个数字使用一次，另外，由于数组中相同数字是相邻的（排序），当我们在后续空位置填充数字时，若所选数字与前一位置的数字相同，说明遇到了相同元素，即出现了上面的情况，我们应当只考虑一次，只选择$[1a,1b,2]$ 排除$ [1b,1a,2] $ ，后者显然填充数字时先选择了后面的1,因此我们可以通过标记数组来实现，当发现相同数字且靠前的数字未被选择时，我们直接忽略：

```c++
if( i != 0 && nums[i] == nums[i-1] && !visited[i-1]){
                continue;
            }
```



```c++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> ans;
        int n = nums.size();
        vector<int> visited(n,0);
        vector<int> subans;

        sort(nums.begin(),nums.end());
        backtrack(nums,ans,subans,visited,0,n);

        return ans;
    }

    void backtrack(vector<int>& nums,vector<vector<int>>& ans,vector<int>& subans,vector<int>& visited,int pos,int n){

        if(pos == n){
            ans.push_back(subans);
            return ;
        }

        for(int i = 0; i < n; i++){
            if( visited[i] || (i != 0 && nums[i] == nums[i-1] && !visited[i-1])){
                continue;
            }
            subans.push_back(nums[i]);
            visited[i] = 1;
            backtrack(nums,ans,subans,visited,pos+1,n);
            subans.pop_back();
            visited[i] = 0;
        }
    }

};
```


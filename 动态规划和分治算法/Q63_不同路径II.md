一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？



```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```



<b>动态规划</b>

我们设$  dp[i][j] $ 表示从 $[0,0]$ 点走到$ [i,j] $ 的路径总数，现在来分析动态转移方程。

- 对于坐标$ [i,j] $ ，且该位置没有障碍物，从上一步走到$ [i,j] $ 共有两种可能，一种是从$[i-1,j]$，一种为$ [i,j-1] $ ，因此$dp[i][j] = dp[i-1][j]+dp[i][j-1]$ 。
- 若坐标处$[i,j]$有障碍物，则 $dp[i][j] = 0$ 。
- 对于坐标$ [0,i] $ 和$ [j,0] $ 两种类型坐标，他们只有一种或0种方法从上一步走到这里，因此需要特殊处理。

```c++
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size(),n = obstacleGrid[0].size();
        
        vector<vector<int>> dp(m,vector<int>(n));

        for(int i = 0 ; i <  m; ++i){
            if(!obstacleGrid[i][0]) dp[i][0] = 1;
            else break;
        } 
        for(int i = 0 ; i <  n; ++i){
            if(!obstacleGrid[0][i]) dp[0][i] = 1;
            else break;
        } 

        for(int i = 1 ; i < m; ++i){
            for(int j = 1; j < n; ++j){
                if(!obstacleGrid[i][j]){
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(mn) $ 。

空间复杂度：$O(mn) $。 



<b>滑动数组优化：</b>

由动态转移方程可知，我们的$ dp[i][j] $ 只和$ dp[i-1][j],dp[i][j-1] $ 有关，因此我们使用一维数组就可以解决问题：求解$ [i,j] $ 位置的方案时，一维数组中$f(j) \to f(n)$ 保存的为前一行坐标$  [i-1,j] \to [i-1,n] $ 对应的方案数，$f(0) \to f(j-1)$ 保存的为$  [i,0] \to [i,j-1] $ 的方案，因此求解$ f(j) $ 只需再此基础上加$ f[j-1] $ 即可。

```c++
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size(),n = obstacleGrid[0].size();
        
        vector<int> f(n);

        f[0] = obstacleGrid[0][0] == 0;
        for(int i = 0 ; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(obstacleGrid[i][j]){
                    f[j] = 0;
                    continue;
                }
                if(j >= 1) f[j] += f[j-1];
            }
        }
        return f[n-1];
    }
```

空间复杂度：$O(n) $。 


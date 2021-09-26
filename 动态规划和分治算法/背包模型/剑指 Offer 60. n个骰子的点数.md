把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

 

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

```
输入: 1
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
```



<b>动态规划</b>

设$dp[i][j]$ 表示i个骰子的点数和为j的概率，那么若已知$dp[1...i-1,1...j-1]$ 的所有结果，我们可以推得$dp[i][j]$ ：

具体的，每多一个骰子，点数可能会增加1-6点6中可能，因此对于$dp[i][j]$， 若满足点数为j，那么我们假设前 i-1个骰子的点数为 $m (m < j) $ ，$dp[i][j]$ 等于所有可能的 $dp[i-1][m+k]$ 的累加和(m应当满足 $k+m = j , 1 \leq k \leq 6 $)：
$$
dp[i][j] = \sum_{k=1}^{6} dp[i-1][j-k]*\frac{1}{6}
$$

```c++
vector<double> dicesProbability(int n) {
        vector<vector<double>> dp(n+1,vector<double>(6*n+1));
        
        for(int j = 1; j <= 6; j++){
            dp[1][j] = (double)1/6;
        }

        for(int i = 2; i <= n; i++ ){
            for(int j = i; j <= 6*i; j++){
                for(int k = 1; k <= 6 ; k++){
                    if(j-k >= i-1 && j-k <= 6*(i-1)) dp[i][j] += (dp[i-1][j-k]*1/6);
                }
            }
        }
        
        vector<double> ans;
        for(int i = n; i <= 6*n; i++)
            ans.push_back(dp[n][i]);

        return ans;
    }
```


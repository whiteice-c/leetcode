输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。



```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```



<b>思路：动态规划</b>

设$dp[i] $ 表示以第i个元素结尾的连续子数组和的最大值，考虑第i个元素，有两种情况：

1. 加入到之前的子数组中，
2. 成为新的子数组

在$dp[i-1] >= 0 $ 的情况下，我们选择情况1，狗则选择情况2.状态转移方程为：
$$
dp[i] = 
\begin{cases}
dp[i-1]+nums[i] & dp[i-1] >= 0 \\
nums[i] & dp[i-1] < 0
\end{cases}
$$

```go
func maxSubArray(nums []int) int {
    maxv,tmp := ^int(^uint(0)>>1),0
    
    for _,num := range nums{
        tmp += num
        if tmp > maxv {
            maxv = tmp 
        }

        if tmp < 0 {
            tmp = 0
        }
    }

    return maxv
}
```


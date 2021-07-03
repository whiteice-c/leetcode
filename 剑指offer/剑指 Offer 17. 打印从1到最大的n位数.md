输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```



n与最大数得关系：$ th = 10^n-1 $

```go
func printNumbers(n int) []int {
    th := 1
    for i:= 1; i <=n; i++{
        th *=10 
    }

    ans := make([]int,th-1)
    for i:= 1; i < th; i++{
        ans[i-1] = i
    }
    return ans
}
```


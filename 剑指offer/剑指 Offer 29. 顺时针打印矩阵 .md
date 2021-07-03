输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。



```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```



**限制：**

- `0 <= matrix.length <= 100`
- `0 <= matrix[i].length <= 100`



<b>思路：模拟</b>

​	我们模拟题目给定的遍历路径，依次遍历元素，具体的，我们需要定义四个状态用于遍历方向的改变，由于遍历方向变化是固定的，依次是向右、向下、向左、向上进行循环，因此状态转换也是固定的。同时我们还需要用四个变量记录行和列的可遍历起始结束点，每次在一个方向遍历结束后，都会使其中一个点发生变化，初始时四个点为 $rows = 0,rowe = m-1,col_s = 0,col_e = n-1 $例如，初始时我们遍历第一行，遍历结束后此时会改变方向为向下，遍历每一行最后一列的元素，但由于第一行最后一列元素已经遍历过，因此之前遍历第一行结束后需要将rows+1，表示第一行所有元素全部遍历过。

​	当遍历的元素个数等于矩阵元素个数，结束循环。

```go
func spiralOrder(matrix [][]int) []int {
    m := len(matrix)
    if m == 0 {
        return nil
    }
    n := len(matrix[0])
    row_s,row_e,col_s,col_e ,size,cnt := 0, m-1 , 0 , n-1,m*n, 0

    ans := make([]int,size)
    stage := 0
    for cnt < size{
        if stage == 0{
            for j := col_s; j <= col_e; j++{
                ans[cnt] = matrix[row_s][j]
                cnt++
            }
            row_s++
            stage = 1
        }else if stage == 1{
            for i := row_s; i <= row_e; i++{
                ans[cnt] = matrix[i][col_e]
                cnt++
            }
            col_e--
            stage = 2
        }else if stage == 2{
            for j := col_e; j >= col_s ; j--{
                ans[cnt] = matrix[row_e][j]
                cnt++
            }
            row_e--
            stage = 3
        }else{
            for i := row_e; i >= row_s ; i--{
                ans[cnt] = matrix[i][col_s]
                cnt++
            }
            col_s++
            stage = 0
        }
    }

    return ans
}
```


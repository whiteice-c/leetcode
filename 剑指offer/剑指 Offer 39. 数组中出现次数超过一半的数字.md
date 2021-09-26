数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

 

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

```
示例 1:

输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```



<b>思路一：hashmap</b>

用map统计数字出现的次数，当元素次数超过一半时，返回该元素。

```go
func majorityElement(nums []int) int {
    m := make(map[int]int)

    for _,num := range nums{
        m[num]++
        if m[num] >= len(nums)/2+1 {
            return num
        }
    }
    return -1
}
```


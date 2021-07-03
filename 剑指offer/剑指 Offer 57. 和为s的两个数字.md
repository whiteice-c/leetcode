难度简单117收藏分享切换为英文接收动态反馈

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]
```

**示例 2：**

```
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]
```

 

<b>思路：hashmap</b>

循环遍历给定数组，同时用map分别保存遍历元素num和target-num，当遍历元素在map中存在时，则直接返回结果。

```go
func twoSum(nums []int, target int) []int {
    m := make(map[int]int)
    ans := []int{}

    for _,num := range nums{
        if _,ok := m[num]; ok{
            ans = append(ans,m[num])
            ans = append(ans,num)
            return ans
        }else{
            m[target-num] = num 
        }
    }
    return ans 
}
```


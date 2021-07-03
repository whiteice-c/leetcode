输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```



<b>思路：双指针</b>

我们定义两个指针，开始时分别指向数组头和尾部，分别向中间遍历，其中头指针寻找偶数，尾指针寻找奇数，当两个指针找到后则交换两个数组元素的位置，然后继续移动指针寻找，当两个指针相遇，所有元素满足左边为奇数右边为偶数。

```go
func exchange(nums []int) []int {
    n := len(nums)
    l := 0
    r := n-1
    for{
        if l >= r {
            break
        }
        for{
            if nums[l] % 2 == 0 || l >= r{
                break
            }
            l++
        }
        for{
            if nums[r] % 2 == 1 || l >= r{
                break
            }
            r--
        }
        tmp := nums[l]
        nums[l] = nums[r]
        nums[r] = tmp
        l++
        r--
    }
    return nums 
}
```

<b>优化版本</b>

```go
func exchange(nums []int) []int {
	for i, j := 0, 0; i < len(nums); i++ {
		if nums[i]&1 == 1 {
			nums[i], nums[j] = nums[j], nums[i]
			j++
		}
	}
	return nums
}
```


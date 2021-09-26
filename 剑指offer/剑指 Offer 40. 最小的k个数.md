输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```



<b>思路：快速排序</b>

快速排序每一次都能确定一个元素在最终排序数组的真实位置k，以及该元素左边的元素都比他小，因此，我们可以通过快排思路来解决问题，具体的，若确定的元素位置恰好为 k-1，那么可以直接返回该数组，若比k-1大，则需要从该元素的左半区域继续寻找。

```go
func getLeastNumbers(arr []int, k int) []int {
    if k == 0 {
        return make([]int,0)
    }
    return findKth(arr,k,0,len(arr)-1)
}

func findKth(arr []int, k,left,right int) []int{
    l , r := left,right
    base := arr[l]

    for l < r{
        for l < r && arr[r] >= base {
            r--
        }
        for l < r && arr[l] <= base {
            l++
        }
        if l < r{
            arr[l],arr[r] = arr[r],arr[l]
        }
    } 
    arr[l],arr[left] = base,arr[l]
    if l == k-1 {
        return arr[:k]
    }else if l > k-1{
        return findKth(arr,k,left,l-1)
    }else{
        return findKth(arr,k,l+1,right)
    }
}
```


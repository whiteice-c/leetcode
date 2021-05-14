给定一个数组，请用快排的思想返回第K大的元素。



<b>思路：</b>

根据快排思想，我们每次确定的一个元素的最终位置为$ l $ ，那么它就是第 $l+1$ 大的元素，我们查看k与$l+1$  的关系：

+ 若 $ k=l+1 $ ，则直接返回结果。
+ 若 $ k<l+1 $ ， 说明目标元素在当前位置的左边，我们从左边寻找即可。
+ 若 $ k>l+1 $ ， 说明目标元素在当前位置的右边，我们从左边寻找即可。

```c++
int kthNum(vector<int>& nums,int left,int right,int k){
    if(left >= right) return nums[left];

    int l = left,r = right,base = nums[left];
    while(l < r){
        while(l < r && nums[r] >= base ) r--;
        while(l < r && nums[l] <= base ) l++;
        if(l < r) swap(nums[l],nums[r]);
    }
    nums[left] = nums[l];
    nums[l] = base;
    if( l == k-1 ) return nums[l];
    else if( l > k-1 ) return kthNum(nums,left,l-1,k);
    else  return kthNum(nums,l+1, right,k);
}
```


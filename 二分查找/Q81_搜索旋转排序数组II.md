已知存在一个按非降序排列的整数数组 nums ，数组中的值不必互不相同。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转 ，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,4,4,5,6,6,7] 在下标 5 处经旋转后可能变为 [4,5,6,6,7,0,1,2,4,4] 。

给你 旋转后 的数组 nums 和一个整数 target ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 nums 中存在这个目标值 target ，则返回 true ，否则返回 false 。

```
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true
```



<b>思路一：暴力</b>

顺序遍历比较是否相等即可。

```c++
bool search(vector<int>& nums, int target) {
        int n = nums.size(), l = 0 , r = n-1;

        for( int num : nums ) 
            if( num == target ) return true;

        return false;
    }
```

<b>复杂度分析</b>

时间复杂度：$O(n)$  

空间复杂度：O(1)。



<b>思路一：二分查找</b>

与 Q33思路相同，我们同样可以使用二分查找。但由于数组中存在重复元素，当出现 $ nums[mid] == nums[0] == nums[n-1] $ 时，我们无法判断哪边区间有序，因此我们只能将整个区间的左右边界缩小一格继续判断。

```c++
bool search(vector<int>& nums, int target) {
        int n = nums.size(), l = 0 , r = n-1;

        while( l <= r ){
            int mid = (l+r)>>1;
            if( nums[mid] == target ) return true;
            
            if( nums[mid] == nums[l] && nums[mid] == nums[r] ){ //无法判断左右区间哪个有序
                l++;
                r--;
            }
            else if( nums[mid] >= nums[l]){ //[l,mid]左区间有序
                if( nums[mid] > target && target >=nums[l] ) r = mid-1; //在左区间
                else l = mid+1; //到右区间寻找
            }
            else{ //[mid,r]右区间有序(升序)
                if( nums[mid] < target && target <= nums[r] ) l = mid+1; 
                else r = mid-1; //不在右区间，到左区间寻找
            }
        }

        return false;
    }
```

<b>复杂度分析</b>

时间复杂度：$O(n)$ ，最坏情况下整个数组全部为相同元素。

空间复杂度：O(1)。
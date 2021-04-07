给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。 不需要考虑数组中超出新长度后面的元素。
```



<b>思路：双指针</b>

我们可以使用快慢指针顺序遍历数组，其中用慢指针定位比较元素，快指针向后遍历，查看是否与慢指针指向元素相等，若相等，则向后移动快指针，当不相等时，此时区间$ [slow,fast-1] $ 都是相同的元素，我们检查元素个数，若个数大于2,则我们删除对应的元素，删除后由于数组大小改变，对应元素的index也改变，我们应注意调整到当前元素位置。

```c++
int removeDuplicates(vector<int>& nums) {
        int n =  nums.size();
        if( n < 3 ) return n;

        int i = 0,j = 1;
        while( j < n ){
            if( nums[i] == nums[j] ) j++;
            else{
                int cnt = j-i-2;
                if( cnt >= 1 ){
                    nums.erase(nums.begin()+i,nums.begin()+i+cnt);
                    j -= cnt+1; //调整指针指向
                    i =  j-1;
                    n = nums.size();
                }
                else{
                    j++;
                    i++;
                }
            }
        }
 
        if( j-i-2 >= 1 )  //末尾出现相同元素还未删除
            nums.erase(nums.begin()+i,nums.begin()+j-2);


        return nums.size();
    }
```

<b>优化：</b>

删除元素需要额外的时间（数组移动），我们考虑不删除而是直接覆盖。我们依旧使用快慢指针，慢指针$slow$表示已经处理的数组的长度，我们使用$ nums[slow-1] $ 表示上一个应该被保留的元素 ,$ nums[slow-2] $ 表示上上个应该被保留的元素 ， $ fast $ 表示当前遍历的元素(在$slow$ 之后)：

1. 当$ nums[slow-2] = nums[fast] $ 时，说明$fast$ 位置对应的元素不应该被保留（升序数组，此时必有$ nums[fast] = nums[slow-1] = nums[slow-2] $），则我们直接向后移动$fast$ 指针，不做任何操作。
2. 当$ nums[slow-2] \neq nums[fast] $ 时， 说明该元素应该被保留，我们将其直接复制到$slow$ 处，然后将$slow$ 指针向后移动，更新已处理长度和$slow-1,slow-2$ 。

由于数组长度小于2时，我们不需要判断，因此初始化$ slow = fast = 2 $ . 当$fast$ 遍历结束后，我们更新了所有可保留的元素，长度为$slow$ 。

```c++
int removeDuplicates(vector<int>& nums) {
        int n =  nums.size();
        if( n < 3 ) return n;
        
        int slow = 2,fast = 2;

        while(fast < n){
            if( nums[slow-2] != nums[fast] ){
                nums[slow] = nums[fast];
                ++slow;
            }
            ++fast;
        }
        return slow;
    }
```

<b>复杂度分析：</b>

时间复杂度：$O(n)$ 。

空间复杂度：O(1)
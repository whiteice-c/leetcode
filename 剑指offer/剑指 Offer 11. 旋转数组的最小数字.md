把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1



<b>思路一：暴力</b>

```c++
int minArray(vector<int>& numbers) {
        for(int i = 1; i < numbers.size(); ++i){
            if(numbers[i] < numbers[i-1]) return numbers[i];
        }
        return numbers[0];
    }
```

<b>复杂度分析</b>

时间复杂度：$O(n)$ 。

空间复杂度：O(1)。



<b>思路一：二分查找</b>

同问题Q33，我们使用二分查找寻找最小值，当左区间有序时，那么左区间$ nums[l] $ 为最小值，我们更新最小值，然后继续从右区间寻找。

```c++
int minArray(vector<int>& numbers) {
        int l = 0, r = numbers.size()-1,ans = INT_MAX;

        while(l <= r){
            int mid = (l+r)>>1;
            if(numbers[mid] == numbers[l] && numbers[mid] == numbers[r]){ //无法判断旋转点
                ans = min(ans,numbers[mid]);
                l++;
                r--;
            }
            else if(numbers[mid] < numbers[l]){ //旋转点在mid左边，且右半区有序
                ans = min(numbers[mid],ans);
                r = mid-1;
            }
            else{//旋转点在mid右边，且左半区有序
                ans = min(numbers[l],ans);
                l = mid+1;
            }
        }

        return ans;
    }
```

<b>复杂度分析</b>

时间复杂度：$O(logn)$ 。

空间复杂度：O(1)。


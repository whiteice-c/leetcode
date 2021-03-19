在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。



```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```



思路：hashmap

遍历元素，并将其出现的次数保存到map中，当遇到一个已经出现的元素，直接返回即可。

```c++
int findRepeatNumber(vector<int>& nums) {
        unordered_map<int,int> m;

        for(int i = 0; i < nums.size(); ++i){
            if( m[nums[i]] != 0 ) return nums[i];
            m[nums[i]]++;
        }
        return -1;
    }
```

**复杂度分析：**

时间复杂度：$O(n)$ 

空间复杂度：$O(n)$ 



set也可以实现同样的操作：

```c++
int findRepeatNumber(vector<int>& nums) {
        set<int> s;

        for(auto num : nums){
            auto [ _ ,status ] = s.insert(num);
            if( !status ) return num;
        }
        return -1;
    }
```


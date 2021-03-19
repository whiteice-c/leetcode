在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

 

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

给定 target = 5，返回 true。
给定 target = 20，返回 false
```



思路一：

根据递增的特点，我们先直接比较target和行列的最后一位元素大小，从而缩小比较区域。然后再进行精确查找。

```c++
bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if( m < 1 ) return false;
        int n = matrix[0].size();
        if( n < 1 ) return false;

        int row = 0 , col = 0;
        for( ; row < m; row++ ){
            if( matrix[row][n-1] >= target ) break;
        }
        for( ; col < n; col++ ){
            if( matrix[m-1][col] >= target ) break;
        }
        for( int i = row; i < m; ++i ){
            for( int j = col; j < n; ++j ){
                if( matrix[i][j] == target ) return true;
            }
        }
        return false ;
    }
```

**复杂度分析：**

时间复杂度：$O(nm)$ 

空间复杂度：$O(1)$

思路二：（限制区域+二分查找）

限制区域后使用二分法逐行查找

```c++
bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if( m < 1 ) return false;
        int n = matrix[0].size();
        if( n < 1 ) return false;

        int row = 0 , col = 0;
        for( ; row < m; row++ ){
            if( matrix[row][n-1] >= target ) break;
        }
        for( ; col < n; col++ ){
            if( matrix[m-1][col] >= target ) break;
        }

        for( int i = row; i < m; ++i ){
            int left = col, right = n-1;
            while( left <= right ){
                int mid = (left + right)>>1;
                if( matrix[i][mid] == target ) return true;
                else if( matrix[i][mid] > target ) right = mid-1;
                else left = mid+1;
            }
        }
        return false ;
    }
```

**复杂度分析：**

时间复杂度：$O(nlogn)$ 

空间复杂度：$O(1)$ 



思路三：（线性查找）

我们利用行列递增的特点，可以从右上角开始搜索，若当前元素等于target，则直接返回。若大于target，那么向下搜索的元素一定都比tartget大，因此我们只能向左搜索。若小于target，那么左边元素一定都比target小，我们只能向下搜索，当我们搜索到target或是遇到边界时即可停止搜索。

```c++
bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if( m < 1 ) return false;
        int n = matrix[0].size();
        if( n < 1 ) return false;

        int row = 0, col = n-1;
        while( row < m &&  col >= 0  ){
            if( matrix[row][col] == target ) return true;
            else if( matrix[row][col] > target ) col--;
            else row++;
        }

        return false ;
    }
```

**复杂度分析：**

时间复杂度：$O(n+m)$ ,访问的列最多增加n次，行最多增加m次。

空间复杂度：$O(1)$ 


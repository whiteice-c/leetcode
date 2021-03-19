给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。



```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```

思路：循环

同问题Q_59,我们按照路径遍历元素，并设置四个变量来记录当前矩形的四个顶点，每走一条路径，我们都更新一次顶点，当遍历元素数目达到矩阵元素数目，返回结果即可。



```c++
vector<vector<int>> ans(n,vector<int>(n));

        int rowstart = 0, rowend = n, colstart = 0, colend = n,count = n*n,j = 1;

        while( true ){
            for(int i = colstart; i < colend; ++i )
                ans[rowstart][i] = j++;
            count -= (colend-colstart);
            rowstart++;
            if(count <= 0) return ans;
            for(int i = rowstart; i < rowend; ++i)
                ans[i][colend-1] = j++;
            count -= (rowend-rowstart);
            if(count <= 0) return ans;
            colend--;
            for(int i = colend-1; i >= colstart; --i)
                ans[rowend-1][i] = j++;
            count -= (colend-colstart);
            if(count <= 0) return ans;
            rowend--;
            for(int i = rowend-1; i >= rowstart; --i)
                ans[i][colstart] = j++;
            count -= (rowend-rowstart);
            if(count <= 0) return ans;
            colstart++;
        }
        return ans;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n^2) $ 

空间复杂度：$O(1)  $


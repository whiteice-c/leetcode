实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。



<b>思路一:暴力法</b>

从小到达求解 $ i^2 > x $ ,当遇到第一个满足条件的i的时候，则直接返回i-1即可。

```c++
int mySqrt(int x) {
        if(x == 0)
            return 0;

        for(int i = 1 ; i <= x ; ++i){
            if( x / i < i ) //防止越界，用除法代替
                 return i-1;
        }
        return x;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(n^2) $ 

空间复杂度： $ O(1) $ 



<b>思路二（二分法）:</b>

​	开始我们从给定数范围的中间开始寻找，另left = 0 ，right = x;另 $mid = (left + right)/2$  ,然后我们得到$ x/mid $ 的结果，有以下三种可能:

+ 当其大于mid时，说明我们需要在区间[mid+1,right]的范围继续去寻找，即更新left = mid+1 进入下一次循环;

+ 相反的，当其小于i时，我们则需要在区间[left,mid-1]中继续寻找，即更新right = mid-1 进入下一次循环;;

+ 若当前循环中等于mid的值，则直接返回结果。

当 l > r时，说明未找到整数平方根，其整数部分恰好为right指向的值。

```c++
int mySqrt(int x) {

        int l = 0 , r = x, mid = 0;
        if(x < 2)  //特殊值，防止下面的除数为0的情况。
            return x;

        while(l <= r){
            mid = (l+r) / 2;

            if( x/mid == mid )
                return mid;
            else 
                x/mid > mid ? l = mid + 1:r = mid - 1;
        }

        return r;
    }
```

当然也可以用乘法来求，此时不需要考虑除数为0的情况，但是要注意结果溢出问题：

```c++
int mySqrt(int x) {

        int l = 0 , r = x, mid = 0;

        while(l <= r){
            mid = (l+r) / 2;
			long ans = mid*mid
            if( ans == x )
                return mid;
            else 
                ans < x ? l = mid + 1:r = mid - 1;
        }

        return r;
    }
```

<b>复杂度分析：</b>

时间复杂度： $  O(logx) $ 

空间复杂度： $ O(1) $ 



<b>思路三：牛顿法</b>

​	牛顿迭代法可以用来快速求解函数零点。它利用了用切线近似曲线的思想，对于一个含有零点的曲线方程，首先通过选取零点近领域曲线上的一点作为起始点 $ x_i $，获得该点的切线方程 $ y = kx +b $ ,并求取切线与x轴的交点作为下一轮迭代的起始点,经过多轮迭代最终快速逼近零点。

​	对于题目求解 $ sqrt(x) $ 问题，我们可以将其转化为求解方程：
$$
f(x) = x^2 - C
$$
方程 $ f(x) = x^2 - C $ 的根为$ x = \sqrt{C} $ ，显然，当 $ C=x $ 时 $ sqrt(x) $ 的解同时也是方程的根。现在我们用牛顿迭代方法去快速求解方程解。选取起始点 $ x_0 = C $ (因为函数有两个零点 $ \mathop{+} \limits_{-} \sqrt{C} $ ，为保证我们能正确迭代到正零点，我们需保证起始点大于 $\sqrt{C}$),则在每一轮迭代中有：

​	当前点为 $( x_i,x_i^2 - C ) $ ,对应的切线方程为  $ y = 2xx_i - x_i^2 -C $ ,对应的切线与x轴的交点为（也是下一轮的迭代点$ x_{i+1} $） ：
$$
x_{i+1} = \frac{1}{2}(x_i+\frac{C}{x_i})
$$
​	因为我们的迭代是逐渐逼近零点的，当我们的相邻两个迭代点非常接近时，我们认为找到了零点。



算法流程：

+ 令C=x，起始点$x_0 = C = x$   

+ 求解下一个迭代点 $x_{i+1} = \frac{1}{2}(x_i+\frac{C}{x_i}) $ 

+ 当$ abs(x_{i+1} - x_i) < 1e-7 $ 时，返回迭代点即为结果。

```c++
int mySqrt(int x) {
        if (x == 0) {
            return 0;
        }

        double C = x, x_pre = x;
        double x_next = 0.5*(x_pre + C/x_pre);

        while( fabs(x_next - x_pre) > 1e-7){
            x_pre = x_next;
            x_next = 0.5*(x_pre + C/x_pre);
        }

        return int(x_next);
        
    }
```



参考文献：

1. https://www.matongxue.com/madocs/205/ ，如何通俗易懂地讲解牛顿迭代法？
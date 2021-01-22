将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```



方法一：

我们写出任意字符的下标排列，观察其规律(如字符长度21,行数为5)

```
1				9				17     step=8=rowLen*2-2
2			8	10			16	18	   step=(6,2)
3		7		11		15		19	   step=(4,4)
4	6			12	14			20	   step=(2,6)
5				13				21	   step=8
```

因此，根据规律，我们逐行直接访问对应下标的字符并拼接即可。

```c++
string convert(string s, int numRows) {
        int len = s.length();
        if(numRows == 1 || len <= numRows) return s;
        
        int step1 = numRows*2-2,step2 =step1;
        string res = "";
        
        for(int j = 0; j < len; j += step1 ) res += s[j]; //拼接第一行
        step1 -= 2;
        for(int i = 2; i < numRows; ++i,step1 -= 2){ //从第二行开始拼接
            int j = i-1,k = j;
            res += s[j]; //先添加第一个元素
            j += step1;
            while(j < len){
                res += s[j];
                if( j-k == step1){
                    k = j;
                    j += step2-step1;;
                }else{
                    k = j;
                    j += step1;
                }
            }
        }
        for(int j = numRows-1; j < len; j += step2 ) res += s[j]; //拼接最后一行

        return res;
    }
```

<b>复杂度分析</b>

时间复杂度：$ O(s)$ ，s为给定字符串的长度。

空间复杂度：$O(1)$ 。 





方法二：

我们可以使用 $min(numRows,len(s)) $个列表来表示 Z 字形图案中的非空行。

从左到右迭代 s，将每个字符添加到合适的行。可以使用当前行和当前方向这两个变量对合适的行进行跟踪。

只有当我们向上移动到最上面的行或向下移动到最下面的行时，当前方向才会发生改变。

```c++
string convert(string s, int numRows) {

        if (numRows == 1) return s;

        vector<string> rows(min(numRows, int(s.size())));
        int curRow = 0;
        bool goingDown = false;

        for (char c : s) {
            rows[curRow] += c;
            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
            curRow += goingDown ? 1 : -1;
        }

        string ret;
        for (string row : rows) ret += row;
        return ret;
    }
```

<b>复杂度分析</b>

时间复杂度：$ O(s)$ ，s为给定字符串的长度。

空间复杂度：$O(n)$ 。 


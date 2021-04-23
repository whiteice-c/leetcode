给定一个Excel表格中的列名称，返回其相应的列序号。

例如，

```
 A -> 1
 B -> 2
 C -> 3
 ...
 Z -> 26
 AA -> 27
 AB -> 28 
 ...

```

<b>方法：</b>

进制转换问题，思路恰好是Q168的反向。

```c++
int titleToNumber(string s) {
        int ans = 0, len = s.length();
        //‘A’ = 62 ‘1’ = 49
        for(int i = len-1 ; i >=0 ; --i){
            ans += (int)(s[i] - 'A' + 1)*pow(26,len-i-1);
        }
        
        return ans;
    }
```

<b>复杂度分析：</b>

时间复杂度：$ O(s) $ ,s为给定字符串的长度

空间复杂度： $ O(1) $   


请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

```
输入：s = "We are happy."
输出："We%20are%20happy.
```



思路一： 

新建一个字符串，循环遍历原字符串，若非空格，则直接拼接在新字符串后，否则替换为 "%20"。

```c++
string replaceSpace(string s) {
        string res;
        
        for( auto ch : s){
            if( ch == ' ' ) res += "%20";
            else res += ch;
        }

        return res;
    }
```

**复杂度分析：**

时间复杂度：O(n)

空间复杂度：O(1)


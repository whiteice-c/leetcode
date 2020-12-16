给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。



方法一（穷尽子串比较）：

​		从左到右依次提取haystack长度为len的子串，然后与needle比较，查看是否相同，若相同，则停止遍历，返回当前子串起始位置即可。

```c++
    int strStr(string haystack, string needle) {
        int len = needle.length();

        if (len == 0)
            return 0;

        for(int i = 0,j = 0 ; i < haystack.length() ; ++i){
            if(i + len > haystack.length())  //限制比较字符串范围，同时限制子串比原串长度的情况
                return -1;
            if(haystack.substr(i,len) == needle)
                return i;
        }

        return -1;
    }
```

复杂度分析

时间复杂度：O((N - L)L)，其中 N 为 haystack 字符串的长度，L 为 needle 字符串的长度。内循环中比较字符串的复杂度为 L，总共需要比较 (N - L) 次。

空间复杂度：O(1)。



方法二（双指针）：

​	方法一的问题在于会将 haystack 所有长度为 L 的子串都与 needle 字符串比较，但实际上只有当两个字符串的第一个字符相同时，才有向后比较的意义。且我们不需要提取整个子串再去比较，只需要向后移动指针，逐一比较，若指针能够移到最后则说明相同。否则，则将指针回溯，回到上次第一个相等字符的下一位置，继续用相同方法比较。



算法

- 移动 pn 指针，直到 pn 所指向位置的字符与 needle 字符串第一个字符相等。

- 通过 pn，pL，curr_len 计算匹配长度。

- 如果完全匹配（即 curr_len == L），返回匹配子串的起始坐标（即 pn - L）。

- 如果不完全匹配，回溯。使 pn = pn - curr_len + 1， pL = 0， curr_len = 0。



```c++
    int strStr(string haystack, string needle) {
        int len = needle.length();

        if (len == 0)
            return 0;

        for(int j = 0 ,i = 0; j < haystack.length() ; ++j){
            if(needle[i] == haystack[j]){
                i += 1;
                if(i == len )
                    return j - i + 1;
            }else{
                j = j - i;
                i = 0;
            }
        }

        return -1;
    }
```

**复杂度分析**

- 时间复杂度：最坏时间复杂度为 O((N - L)L)，最优时间复杂度为 O(N)。
- 空间复杂度：O(1)。



方法三（ Rabin Karp ）：

​		先生成窗口内子串的哈希码，然后再跟 needle 字符串的哈希码做比较。

​		由于只会出现小写的英文字母，因此可以将字符串转化成值为 0 到 25 的整数数组： arr[i] = (int)S.charAt(i) - (int)'a'。按照这种规则，abcd 整数数组形式就是 [0, 1, 2, 3]，转换公式如下所示。
$$
num([abcd]) = num([0,1,2,3]) = 0 \times 26^3 + 1 \times 26^2 + 2 \times 26^1 +3 \times 26^0
$$
这样，字符串abcd有且可以由唯一的一个整数代替表示。进而，得到转换的通用公式，对于长度为L的字符串，我们可以按照以下公式转换成对应的哈系码(a=26)：
$$
\begin{align}
num(str) &= s_0a^{L-1} +s_1a^{L-2} + ... + s_ia^{L-1-i} + ... + s_{L-1}a^1 + s_{L}a^0\\
 &= \sum_{i=0}^{L-1}s_ia^{L-1-i}
\end{align}
$$
通过比对两个字符串的哈希码是否相同，即可判断字符串是否一样。现在，我们可以通过滑动窗口的方法来遍历haystack字符串的子串对应的哈希码，以达到O(n)的时间复杂度下就可以比较完毕。具体做法我们以abcd到bcde滑动为例：

整数形式数组从 [0, 1, 2, 3] 变成了 [1, 2, 3, 4]，数组最左边的 0 被移除，同时最右边新添了 4。滑动后数组的哈希值可以根据滑动前数组的哈希值来计算，计算公式如下所示。
$$
num(new\_str) = (num(str) - 0 \times 26^3) \times 26 + 4 \times26^0
$$
更一般的，对于长度为L的子串滑动时，计算如下
$$
num(new\_str) = (num(str) - str[0] \times a^{L-1}) \times a + num(str[L]) \times a^0\\
 = num(str)\times a - str[0]  \times a^{L} + num(str[L])
$$
**算法流程：**

- 计算子字符串 haystack.substring(0, L) 和 needle.substring(0, L) 的哈希值。

- 从起始位置开始遍历：从第一个字符遍历到第 N - L 个字符。
  - 根据前一个哈希值计算滚动哈希。
  - 如果子字符串哈希值与 needle 字符串哈希值相等，返回滑动窗口起始位置。

- 返回 -1，这时候 haystack 字符串中不存在 needle 字符串。



```c++
//滚动哈希
    int strStr(string haystack, string needle) {

        int a = 26;
        long mod_num = pow(2,31);
        int len = needle.length();
        long convert_num = 0;
        long needle_num = 0;

        if(haystack.length() < len)  //子串长，直接返回
            return -1;

        //计算haystack(0,L)和needle的哈系值
        for(int i = 0; i < len; ++i){
            convert_num = (convert_num*a + (int) haystack[i] - (int) 'a')%mod_num;
            needle_num = (needle_num*a + (int) needle[i] - (int) 'a')%mod_num;
        }
        if(convert_num == needle_num)
            return 0;

        long al = 1; //用于防止窗口移动时出现的结果溢出，提前求余
        for(int i = 0; i < len; ++i) al = al * a % mod_num;

        //滚动哈系比较
        for(int i = len; i < haystack.length(); ++i){

            convert_num = (convert_num * a - \
                        ((int)haystack[i-len] - (int) 'a')* al \
                        + (int)haystack[i] - (int) 'a') % mod_num;

            if(convert_num == needle_num)
                return i - len + 1;
        }

        return -1;
    }
```

**如何避免溢出：**

​	由于$a^L$ 可能是个很大的值，因此我们需要设置数值上限来避免溢出。我们可以采用取模的方式，之前采用下面的代码避免溢出，然而并不起作用，原因是 $ 26^L $ 有可能会直接溢出。

```c++
    convert_num = (int)(convert_num + ((int) haystack[i] - (int) 'a') * (long)pow(26,len-i-1))%mod_num;
    needle_num = (int)(needle_num + ((int) needle[i] - (int) 'a') * (long)pow(26,len-i-1))%mod_num;
```

因此改为下面的代码，它变为迭代的去取余防止溢出，而不是一次性的求解$ 26^L $ ，即每次在上一次的基础上乘26,然后进行取余防溢出，因为我们的变量是long型，因此无论如何无法用一次乘法（乘26）就导致结果溢出。

```c++
convert_num = (convert_num*a + (int) haystack[i] - (int) 'a')%mod_num;
needle_num = (needle_num*a + (int) needle[i] - (int) 'a')%mod_num;
```

**复杂度分析：**

时间复杂度：O(N)，计算 needle 字符串的哈希值需要 O(L)时间，之后需要执行 (N - L)次循环，每次循环的计算复杂度为常数。

空间复杂度：O(1)。

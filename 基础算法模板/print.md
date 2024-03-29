<b>KMP算法</b>

字符串s与模板串p匹配问题：

定义$ next[i] = j $ 为模板串中， $p[1...j]$ 与$p[i-j+1...i] $ 是相等的。即以i为终点的子串$p[i-j+1...i] $与以起点1开始的子串$p[1...j]$最长相同部分的长度为j。

```
1               i
a1 a2 a3 a4 ... ai ... an 
若 next[i] = 3  则有 [a1,a2,a3] = [a(i-2),a(i-1),ai]
```

这样，当字符串s与模板串p匹配过程中，若有$ s[i] \neq p[j] $ 时，我们可以求得$ next[j-1] $ 的长度 $k$ ，表示$p[1...k]$ 与 $p[j-1-k,j-1] $ 是相等的，而$ s[i,i-k-1] = p[j-1-k,j-1]$  ， 我们不需要从头开始匹配，直接从比较 $ s[i]$  和$p[k+1]$   开始即可。 

求解next数组就是自身匹配的过程，若 $p[i] \neq p[j] $ ，则子串$ p[1,j] $ 回退，直到$ p[1,k] = p[i-k-1,i] $ ，此时有$ next[i] = k $ ，若$p[i] = p[j] $ ，则有$ p[1,j] = p[i-j-1,i] , next[i] = j $ 。

```c++
// s[]是长文本，p[]是模式串，n是s的长度，m是p的长度  
求模式串的Next数组：
//ne[1] = 0 ,字符串下标从1开始,只有一个字符时无子串
for (int i = 2, j = 0; i <= m; i ++ ) //与自身匹配的过程，s[i-j-1,i] = s[1,j]
{
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j ++ ;
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i <= n; i ++ )
{ 
    while (j && s[i] != p[j + 1]) j = ne[j]; //不匹配时(此时已匹配s[i-j-1,i-1] = p[1,j])，回退到next[j]处开始匹配(即找到 p[1,k] = p[j-k+1,j] = s[i-1-k+1,i-1])且满足从k匹配时有s[i] = p[k+1],那么就可以从k处开始向后匹配，若 j = 0，说明需要开始从头匹配
    if (s[i] == p[j + 1]) j ++ ; //回退后，next[j]后的第一个字符匹配成功，或从头开始匹配成功
    if (j == m)
    {
        j = ne[j]; //匹配成功的下一次开始匹配位置
        // 匹配成功后的逻辑
    }
}
```

求解next数组代码过程：

```
 p = abababab
 1 2 3 4 5 6 7 8
 a b a b a b a b
 
 next[1] = 0  此时子串长度为0，无相同后缀前缀子串
 next[2] : p[2] neq [1], j = next[1] = 0, p[2] \neq p[1]  next[2] = j = 0
 next[3] : p[3] = p[1], j = 1 , next[3] = 1
 next[4] : p[4] = p[2], j = 2, next[4] = 2
 ...
```



<b>Trie树</b>

用于高效地存储和查找字符串集合的数据结构 

```c++
int son[N][26], cnt[N], idx;
// 0号点既是根节点，又是空节点
// son[][]存储树中每个节点的子节点，N为给定字符串的总长度
// cnt[]存储以每个节点结尾的单词数量
// idx用于计数创建节点，创建过程中为首次出现的字符节点分配不同的序列号

// 插入一个字符串
void insert(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) son[p][u] = ++ idx; //当前节点不存在该儿 子，创建
        p = son[p][u]; //更新父亲节点，准备记录下一节点
    }
    cnt[p] ++ ;//记录以该节点结尾的单词数量
}

// 查询字符串出现的次数
int query(char *str)
{
    int p = 0;
    for (int i = 0; str[i]; i ++ )
    {
        int u = str[i] - 'a';
        if (!son[p][u]) return 0;
        p = son[p][u];
    }
    return cnt[p];
}
```





<b>并查集</b>

每个集合用一棵树表示。树根的编号就是整个集合的编号，每个节点存储它的父节点，p[x]表示节点x的父节点。

搜索父节点：` while(p[x] != x) x = p[x]` ,对于孩子节点离父节点较远的节点，搜索时间复杂度较高，因此我们可以用路径搜索的方法进行压缩，当找到根节点后，我们将该路径上的节点的值都更新指向根节点，下次就只需要$ O(1)$ 的时间就能够搜索到，可以用递归回溯实现。

```c++
    int p[N]; //存储每个点的祖宗节点

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]); //回溯时会路径压缩
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ ) p[i] = i;

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b); //合并时也路径压缩,a的祖宗节点指向b的祖宗节点，a合并到b集合里
```

<b>维护size的并查集</b>

维护每个数集合的节点数：

```c++
    int p[N], size[N];
    //p[]存储每个点的祖宗节点, size[]只有祖宗节点的有意义，表示祖宗节点所在集合中的点的数量

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        size[i] = 1;
    }

    // 合并a和b所在的两个集合：
    size[find(b)] += size[find(a)];
    p[find(a)] = find(b); 
```

<b>维护到祖宗节点距离的并查集</b>

```c++
    int p[N], d[N];
    //p[]存储每个点的祖宗节点, d[x]存储x到p[x]的距离

    // 返回x的祖宗节点
    int find(int x)
    {
        if (p[x] != x)
        {
            int u = find(p[x]); //保存路径压缩信息，更新距离后再压缩路径
            d[x] += d[p[x]]; // 节点到祖宗节点的距离为 1 + 其父节点到祖宗节点的距离
            p[x] = u;
        }
        return p[x];
    }

    // 初始化，假定节点编号是1~n
    for (int i = 1; i <= n; i ++ )
    {
        p[i] = i;
        d[i] = 0;
    }

    // 合并a和b所在的两个集合：
    p[find(a)] = find(b);
    d[find(a)] = distance; // 根据具体问题，初始化find(a)的偏移量
```


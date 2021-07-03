在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

示例:

```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```


限制：

0 <= s 的长度 <= 50000



<b>思路一：hashmap</b>

通过hashmap保存每个字符的出现次数以及第一次出现的位置，然后返回出现单词且下标最前的元素。

```go
func firstUniqChar(s string) byte {
    type pair struct{
        freq int
        mid  int 
    }
    m := make(map[byte]*pair)

    for i := range s {
        if _,ok := m[s[i]]; ok{
            m[s[i]].freq++
        }else{
            m[s[i]] = &pair{1,i}
        }
    }

    ans := 51000
    for _,v := range m{
        if v.freq == 1{
            if ans > v.mid{
                ans = v.mid
            }
        }
    }

    if ans == 51000 {
        return byte(' ')
    }else{
        return s[ans]
    }
}
```

<b>优化：</b> 我们可以只记录其第一次出现的位置即可，当第二次出现时，我们将其出现位置改为-1，表示非单次出现。

```go
func firstUniqChar(s string) byte {
    m := make(map[byte]int)

    for i := range s {
        if _,ok := m[s[i]]; ok{
            m[s[i]] = -1
        }else{
            m[s[i]] = i
        }
    }

    ans := 50001
    for _,v := range m{
        if v != -1{
            if ans > v {
                ans = v
            }
        }
    }

    if ans == 50001{
        return byte(' ')
    }else{
        return s[ans]
    }
}
```


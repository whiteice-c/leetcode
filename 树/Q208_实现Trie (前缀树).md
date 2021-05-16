Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。



请你实现 Trie 类：

Trie() 初始化前缀树对象。
void insert(String word) 向前缀树中插入字符串 word 。
boolean search(String word) 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。
boolean startsWith(String prefix) 如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。



```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

提示：

$1 <= word.length, prefix.length <= 2000$ 
word 和 prefix 仅由小写英文字母组成
insert、search 和 startsWith 调用次数 总计 不超过$ 3 * 10^4$ 次



<b>前缀树</b>

Trie 是一颗非典型的多叉树模型，多叉好理解，即每个结点的分支数量可能为多个。

它和一般的多叉树不一样，尤其在结点的数据结构设计上，一般的多叉树的结点是这样的：

```
struct TreeNode {
    VALUETYPE value;    //结点值
    TreeNode* children[NUM];    //指向孩子结点
};
```

而 Trie 的结点是这样的(假设只包含'a'~'z'中的字符)：

```
struct TrieNode {
    bool isEnd; //该结点是否是一个串的结束
    TrieNode* next[26]; //字母映射表
};
```

可以看出，前缀树中的节点并没有直接的保存数据，它是通过建立对应的字母映射表来表示当前节点中含有哪些字符，即若$ next[i]$ 中的指针不为空，则说明当前节点的$  i$ 位置处有字符$ 'a'+i $， 该指针继续指向其孩子节点，我们通过遍历孩子节点中的非空指针继续判断后续字符，从而构成一个个单词。

<b>插入</b>

我们顺序遍历给定字符串，对于每一个字符，我们判断字母映射表字符对应的位置的指针是否为空，若为空，则在该位置创建新的字母映射表，表示当前位置有字符，并把指针更新为当前节点，然后继续判断后续字符，向下建树。

<b>查找</b>

顺序遍历给定字符串，查看每个字符在当前层的字母映射表的对应位置是否为空指针，不为空则存在，更新指针继续i向下查找后续字符，若是空指针，则说明字符串不存在，直接返回。所有字符匹配，且最后一个字符的成员$isEnd = true $ ，则查找成功。 

<b>前缀匹配</b>

给定字符串完全匹配，则匹配成功。

```c++
class Trie {
private:
    vector<Trie *> children;
    bool isEnd;

    Trie* searchPrefix(string s){
        Trie *node = this;
        for( char ch : s ){
            int id = ch - 'a';
            if( node->children[id] == nullptr ){ //创建孩子链表表示当前节点有字符
                return nullptr;
            }
            node = node->children[id]; //准备创建下一字符链表
        }
        return node;
    }
public:
    /** Initialize your data structure here. */
    Trie(): children(26), isEnd(false) {}
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Trie *node = this;
        for( char ch : word ){
            int id = ch - 'a';
            if( node->children[id] == nullptr ){ //创建孩子链表表示当前节点有字符
                node->children[id] = new Trie;
            }
            node = node->children[id]; //准备创建下一字符链表
        }
        node->isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Trie *node = searchPrefix(word);
        return node && node->isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        return searchPrefix(prefix);
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

<b>复杂度分析</b>

时间复杂度：$ O(s) $ ，插入、查找的时间复杂度等于$O(s)$ ,为字符串的长度。 

空间复杂度：$ O(26*Len) $ ,Len 为所有字符的长度之和。



<b>数组实现代码</b>

```c++
int son[1000010][26];
    int cnt[1000010];
    int idx;
    /** Initialize your data structure here. */
    Trie() {
        idx = 0;
        memset(son,0,sizeof(son));
        memset(cnt,0,sizeof(cnt));
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        int p = 0;
        for(int i = 0; word[i]; ++i){
            int u = word[i]-'a';
            if(!son[p][u]) son[p][u] = ++idx;
            p = son[p][u];
        }
        cnt[p]++;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        int p = 0;
        for(int i = 0; word[i]; ++i){
            int u = word[i]-'a';
            if(!son[p][u]) return false;
            p = son[p][u];
        }
        return cnt[p];
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        int p = 0;
        for(int i = 0; prefix[i]; ++i){
            int u = prefix[i]-'a';
            if(!son[p][u]) return false;
            p = son[p][u];
        }
        return true;
    }
```


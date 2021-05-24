请你为 最不经常使用（LFU）缓存算法设计并实现数据结构。

实现 LFUCache 类：

+ LFUCache(int capacity) - 用数据结构的容量 capacity 初始化对象
+ int get(int key) - 如果键存在于缓存中，则获取键的值，否则返回 -1。
+ void put(int key, int value) - 如果键已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量时，则应该在插入新项之前，使最不经常使用的项无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最近最久未使用 的键。
  注意「项的使用次数」就是自插入该项以来对其调用 get 和 put 函数的次数之和。使用次数会在对应项被移除后置为 0 。

为了确定最不常使用的键，可以为缓存中的每个键维护一个 使用计数器 。使用计数最小的键是最久未使用的键。

当一个键首次插入到缓存中时，它的使用计数器被设置为 1 (由于 put 操作)。对缓存中的键执行 get 或 put 操作，使用计数器的值将会递增。

 

**提示：**

- `0 <= capacity, key, value <= 104`
- 最多调用 `105` 次 `get` 和 `put` 方法



```
输入：
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
输出：
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

解释：
// cnt(x) = 键 x 的使用计数
// cache=[] 将显示最后一次使用的顺序（最左边的元素是最近的）
LFUCache lFUCache = new LFUCache(2);
lFUCache.put(1, 1);   // cache=[1,_], cnt(1)=1
lFUCache.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lFUCache.get(1);      // 返回 1
                      // cache=[1,2], cnt(2)=1, cnt(1)=2
lFUCache.put(3, 3);   // 去除键 2 ，因为 cnt(2)=1 ，使用计数最小
                      // cache=[3,1], cnt(3)=1, cnt(1)=2
lFUCache.get(2);      // 返回 -1（未找到）
lFUCache.get(3);      // 返回 3
                      // cache=[3,1], cnt(3)=2, cnt(1)=2
lFUCache.put(4, 4);   // 去除键 1 ，1 和 3 的 cnt 相同，但 1 最久未使用
                      // cache=[4,3], cnt(4)=1, cnt(3)=2
lFUCache.get(1);      // 返回 -1（未找到）
lFUCache.get(3);      // 返回 3
                      // cache=[3,4], cnt(4)=1, cnt(3)=3
lFUCache.get(4);      // 返回 4
                      // cache=[3,4], cnt(4)=2, cnt(3)=3
```



<b>思路一：hashmap</b>

我们用hashmap 保存key，value，其中value我们分别记录下key 的值、使用频率以及使用时间。

```c++
class LFUCache {
public:
    int capacity ,t;
    struct CacheData{
        int value;
        int fre;
        int time;
    };
    unordered_map<int,CacheData> m;
    LFUCache(int capacity) {
        this->capacity = capacity;
        t = 0;
    }
    
    int get(int key) {
        if( m.find(key) != m.end() ){ //找到则更新频率和使用时间
            m[key].time = t;
            m[key].fre++;
            t++;
            return m[key].value;
        } 
        return -1;
    }
    
    void put(int key, int value) {
        if(capacity == 0) return;
        if( m.find(key) != m.end() ){ //已经存在则更新value，时间和使用频率
            m[key].value = value; 
            m[key].time = t;
            m[key].fre++;
        }
        else{
            if(m.size() == capacity){ //缓存满则找到最少使用且最久未使用的key
                int min_f = INT_MAX,min_k = -1;
                for(auto [k,v] : m){
                    if(v.fre < min_f){
                        min_f = v.fre;
                        min_k = k;
                    }
                    else if(v.fre == min_f){
                        if(m[min_k].time > v.time){
                            min_f = v.fre;
                            min_k = k;
                        }
                    }
                };
                m.erase(min_k);   //删除找到的key
            }

            CacheData d = {value,1,t}; //加入当前key
            m.emplace(key,d);
        }
        t++;
    }
};

```

<b>复杂度分析：</b>

时间复杂度：$ O(n) $ 

空间复杂度：$O(n)$ 



<b>思路二：双hashmap</b>

我们可以使用两个hashmap，第一个hashmap保存freq_table = [freq,List]， 保存的为freq和其对应的链表，另外一个hashmap保存的是node_map = [key,node] ，即每个节点在第一个hashmao中的具体位置。

+ put：若插入的元素在node_map中不存在且缓存未满，则我们需要将其插入到freq_table[1]的链表头部。若插入的元素不存在但缓存满了，我们需要从含有节点的最低频率链表中删除尾部元素，然后再插入到freq_table[1]的链表头部。若插入的元素已经存在，我们需要更新value和freq，并将其从原频率freq_table[node->freq]链表中删除，然后加入到新的频率链表freq_table[node->freq+1]头部。
+ get : 若节点存在，我们需要更新其频率，将其从频率freq_table[node->freq]链表中删除，然后加入到新的频率链表freq_table[node->freq+1]头部。若不存在，则返回-1.
+ minfreq: 我们需要维护一个min_freq，以实现在$O(1)$ 时间内删除缓存中最低频率的节点。



```c++
struct Node{
    int freq,key,value;
    Node(int _freq,int _key,int _value) : freq(_freq) ,key(_key),value(_value){} 
};

class LFUCache {
private:
    int capacity,cnt,minfreq;
    unordered_map<int,list<Node>::iterator> node_map;
    unordered_map<int,list<Node>> freq_table; //保存频率表对应的头尾节点
public:
    LFUCache(int capacity) {
        this->capacity = capacity;
        cnt = 0;
        minfreq = 1;
    }
    
    int get(int key) {   
        if(capacity == 0) return -1;
        //先删除该节点，然后添加到freq+1链表头部,观察是否需要更新min_freq
        auto it = node_map.find(key);
        if(it != node_map.end()){
            //先删除该节点，然后添加到freq+1链表头部,观察是否需要更新min_freq
            list<Node>::iterator node = it->second;
            int freq = node->freq, value = node->value;
            freq_table[freq].erase(node); //删除该节点
            if(freq_table[freq].size() == 0 ){
                freq_table.erase(freq);
                if(freq == minfreq ) minfreq++;//更新最小节点
            }
            freq_table[freq+1].push_front(Node(freq+1,key,value));
            node_map[key] =  freq_table[freq+1].begin();
            return value;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if(capacity == 0 ) return;
        auto it = node_map.find(key);
        if(it != node_map.end()){
            //先删除该节点，然后添加到freq+1链表头部,观察是否需要更新min_freq
            list<Node>::iterator node = it->second;
            int freq = node->freq;
            freq_table[freq].erase(node); //删除该节点
            if(freq_table[freq].size() == 0 ){
                freq_table.erase(freq);
                if(freq == minfreq ) minfreq++;//更新最小节点
            }
            freq_table[freq+1].push_front(Node(freq+1,key,value));
            node_map[key] =  freq_table[freq+1].begin();  //更新内存地址
            return;
        }

        if(cnt < capacity){
            //添加到frea_table[1]头部
            freq_table[1].push_front(Node(1,key,value));
            node_map[key] = freq_table[1].begin();
            minfreq = 1;
            cnt++;
        }
        else{
            //先删除frea_table[minfreq]的尾部节点，观察是否需要更新min_freq
            auto node = freq_table[minfreq].back();
            node_map.erase(node.key);
            freq_table[minfreq].pop_back();
            if(freq_table[minfreq].size() == 0) freq_table.erase(minfreq);
            freq_table[1].push_front(Node(1,key,value));
            node_map[key] = freq_table[1].begin();
            minfreq = 1;
        }

    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```


运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制 。
实现 LRUCache 类：

LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

**进阶**：你是否可以在 `O(1)` 时间复杂度内完成这两种操作？

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

提示：

+ 1 <= capacity <= 3000
+ 0 <= key <= 3000
+ 0 <= value <= 104
+ 最多调用 3 * 104 次 get 和 put



<b>思路：双向链表+hashmap</b>

对于put操作，我们想要实现先进先出的特性，那么可以使用队列来操作。插入数据时，我们先要判断数据是否存在，为了能够在$O(1)$的时间内完成操作，我们想到额外用hashmap保存队列中每个key所在节点的内存地址，这样就可以在$O(1)$ 时间内找到。对于get操作，若存在给节点，我们需要先将该节点从链表中删除，然后将它重新放在表头之后，以表示它是最近被使用的，若想使删除和移动操作在$O(1)$ 时间内完成，那么队列需要使用双端链表构造。

get操作细节：

- 若插入的数据已经存在，则我们需要更新对应的value值，并将它从链表中删除，然后插入到表头，以表示它是最近被使用的。
- 若数据不存在且缓冲未满，那么我们直接将其插入到表头之后就可以。
- 若数据不存在且缓冲已满，那么我们需要将链表尾端节点删除，然后创建新节点插入都表头之后。

```c++
class LRUCache {
private:
    struct ListNode{
        int key;
        int val;
        ListNode *pre;
        ListNode *next;
    };
    int capacity;
    int cnt;
    ListNode *head,*tail;
    unordered_map<int,ListNode*> m;

    //定义插入删除统一函数，
    void insert(ListNode* head,ListNode *denode,ListNode *innode){
        if(denode){//判断是否需要删除操作
            ListNode *preNode = denode->pre, *nextNode = denode->next;
            preNode->next = nextNode;
            nextNode->pre = preNode;
        }
        //插入节点到头部
        innode->next = head->next;
        innode->pre = head;
        head->next->pre = innode;
        head->next = innode;
    }
public:
    LRUCache(int capacity) {
        this->capacity = capacity;
        cnt = 0;
        head = new ListNode;
        tail = new ListNode;
        head->key = tail->key = -1;
        head->val = tail->val = -1;
        head->next = tail;
        tail->pre = head;
    }
    
    int get(int key) {
        if( m.find(key) != m.end()){
            insert(head,m[key],m[key]);
            return m[key]->val;
        }
        return -1;
    }
    
    void put(int key, int value) {
        if( m.find(key) != m.end()){
            //更新val，删除移动到头部
            m[key]->val = value;
            insert(head,m[key],m[key]);
            return ;
        }

        if(cnt < capacity){
            //直接插入到头部
            ListNode *node = new ListNode;
            node->key = key,node->val = value;
            insert(head,nullptr,node);
            m[key] = node;
            cnt++;
        }
        else{
            //删除尾部节点，然后插入到头部
            ListNode *node = new ListNode;
            node->key = key,node->val = value;
            m.erase(tail->pre->key);
            insert(head,tail->pre,node);
            m[key] = node;
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

<b>复杂度分析：</b>

时间复杂度：$ O(1) $ 

空间复杂度：$O(n)$ 






用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )



```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```



<b>思路：双栈</b>

我们定义两个栈，其中stk1只负责插入数据，为了实现先进先出，我们需要顺序从stk1弹出，再依次入栈stk2，才能实现。因此，弹出时，我们先检查stk2有无元素，若有直接弹出，没有则将stk1的元素全部压入stk2中，然后弹出栈顶元素即可、

```c++
stack<int> stk1,stk2;
    CQueue() {

    }
    
    void appendTail(int value) {
        stk1.push(value);
    }
    
    int deleteHead() {
        if(!stk2.empty()){
            int ans = stk2.top();
            stk2.pop();
            return ans;
        }

        while(!stk1.empty()){
            stk2.push(stk1.top());
            stk1.pop();
        }
        
        if(!stk2.empty()){
            int ans = stk2.top();
            stk2.pop();
            return ans;
        }
        
        return -1;
    }
```

<b>复杂度分析：</b>

时间复杂度：$O(n)$ ,入队为$ O(1)$ ，出栈为$O(n)$

空间复杂度：$O(n)$ 
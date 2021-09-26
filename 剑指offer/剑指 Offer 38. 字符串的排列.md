输入一个字符串，打印出该字符串中字符的所有排列。

 

你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。



```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```



<b>思路：记忆化递归回溯</b>

我们使用递归搜索每一种可能的答案，对于每一个可能组合的字符串s，我们对其每一个位置i处的字符进行遍历，并在路径上记忆已经使用过的字符，在路径的下一层若遇到已经使用过的字符，我们直接跳过。由于可能存在相同的字符导致出现相同的组合，我们使用set去重。

```c++
void backtrack(string s,int n , string& subans, set<string>& ans,vector<int> &visited){
        if( subans.size() == n ){
            ans.insert(subans);
            return;
        }

        for(int i = 0; i < n; ++i){
            if(visited[i] == 1) continue;
            subans += s[i];
            visited[i] = 1;
            backtrack(s,n,subans,ans,visited);
            subans.pop_back();
            visited[i] = 0;
        }
        return ;
    }

    vector<string> permutation(string s) {
        int n = s.size();
        
        string subans;
        set<string> ans;
        vector<int> visited(n,0);

        backtrack(s,n,subans,ans,visited);
        vector<string> ret;

        for(auto str : ans) ret.push_back(str);
        return ret;
    }
```


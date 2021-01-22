请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

注意：

本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 $[−2^{31},  2^{31} − 1]$。如果数值超过这个范围，请返回  $2^{31} − 1$ 或 $−2^{31}$ 。

 



方法一(边界问题)：

遍历给定字符串，先找到第一个有效字符['+','-','0'-'9'],用一个bool变量保存该数字的正负，用另一个变量来记录我们找到了第一个有效字符，以便之后遇到非数字字符时停止遍历。在有效字符区间进行数字转换即可。

```c++
int myAtoi(string s) {
        bool isNegative = false;
        int count = 0;
        long ans = 0;

        for(int i = 0 ; i < s.length(); ++i){
            if(s[i] != ' '){
                if(count == 0){ //首次遇到非空格字符，判断是否符合数字要求
                    if( !(s[i] == '+' || s[i] == '-' || (s[i] >= '0' && s[i] <= '9'))) return 0;              
                }else{
                    if( !( s[i] >= '0' && s[i] <= '9')) return isNegative ? 0-ans:ans;//再次遇到非空格，返回结果
                }
                
                count += 1;
                if(s[i] == '-'){
                    isNegative = true;
                    continue;
                } 
                else if(s[i] == '+') continue;
                else{
                    ans = ans*10 + int(s[i])-48;
                    if(ans > INT_MAX) return isNegative ? INT_MIN:INT_MAX;
                }
            }
            else if(s[i] == ' ' && count > 0) //之后遇到空格则返回结果
                return isNegative ? 0-ans:ans;
        }

        return isNegative ? 0-ans:ans;
    }
```

<b>复杂度分析</b>

时间复杂度：$ O(s)$ ，s为给定字符串的长度。

空间复杂度：$O(1)$ 。 



方法二：（有限状态机）

直接思考题目的各个条件及边界，很容易造成逻辑混换和代码臃肿问题，可以使用状态机描述整个题目状态，根据状态来写代码。

|           | ' '   | '+' or '-' | '0' - '9' | others |
| --------- | ----- | ---------- | --------- | ------ |
| start     | start | sign       | in number | end    |
| sign      | end   | end        | in number | end    |
| in number | end   | end        | in number | end    |
| end       | end   | end        | end       | end    |



```c++
class Automation {
    unordered_map<string,vector<string>> table = {
            {"start", {"start", "signed", "in_number", "end"}},
            {"signed", {"end", "end", "in_number", "end"}},
            {"in_number", {"end", "end", "in_number", "end"}},
            {"end", {"end", "end", "end", "end"}}
        };
    string state = "start";

    int getCol(char c){
        if( c == ' ' ) return 0 ;
        else if( c == '+' || c == '-' ) return 1;
        else if( c >= '0' && c <= '9' ) return 2;
        else return 3;
    }
    
public:
    long ans = 0;
    int sign = 1;

    int get(char c){
        state = table[state][getCol(c)];
        if(state == "in_number"){
            ans = ans*10 + c - '0';
            ans = sign == 1 ? min(ans, (long)INT_MAX) : min(ans, -(long)INT_MIN);
        }
        else if( state == "signed") sign = c == '+' ? 1:-1;
        else if( state == "end") return 0;
        return 1;
    }
};

class Solution {
public:
    int myAtoi(string s) {
        Automation am;

        for(auto& c : s)
            if(!am.get(c)) break; //end状态提前结束判断
        return am.ans*am.sign;
    }

};
```

<b>复杂度分析</b>

时间复杂度：$ O(s)$ ，s为给定字符串的长度。

空间复杂度：$O(1)$ 。 
```
1 5        ===  6
10 20      ===  30
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    long tmp,sum;
    while(cin >> tmp){
        sum = 0;
        while(true){
            sum += tmp;
            if( cin.get() == '\n' ) break; //若不是回车，则把空格去掉
            cin >> tmp;
        }
        cout << sum << endl;
    }
}
```

```
2
1 5	      ===  6
10 20      ===  30
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    int cnt;
    cin >> cnt;
    long tmp,sum=0;
    while(cnt--){
        sum = 0;
        cin >>tmp;
        while(true){
            sum += tmp;
            if( cin.get() == '\n' ) break;
            cin >> tmp;
            
        }
        cout << sum << endl;
    }
}
```

```
4 1 2 3 4		===  10
5 1 2 3 4 5		===  15
0		end
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    long tmp,sum,cnt;
    while(cin >> cnt){
        sum = 0;
        if( cnt == 0 ) return 0;
        cin >> tmp; //取第一个正整数
        while(true){
            sum += tmp;
            if( cin.get() == '\n' ) break; //若不是回车，则把空格去掉
            cin >> tmp;
        }
        cout << sum << endl;
    }
}

```

```
2
4 1 2 3 4 		===  10
5 1 2 3 4 5		===  15

```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    int cnt;
    cin >> cnt;
    long tmp,sum=0;
    while(cnt--){
        sum = 0;
        cin >>tmp; //去掉第一个正整数
        cin >>tmp;
        while(true){
            sum += tmp;
            if( cin.get() == '\n' ) break;
            cin >> tmp;
        }
        cout << sum << endl;
    }
}

```

```
4 1 2 3 4	===  10
5 1 2 3 4 5	===  15
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    long tmp,sum=0;
    while(cin >> tmp){ //取第一个数字个数
        sum = 0;
        cin >> tmp; //去该行第一个计算数字
        while(true){
            sum += tmp;
            if( cin.get() == '\n' ) break;
            cin >> tmp;
        }
        cout << sum << endl;
    }
}

```

```
1 2 3		===  6
4 5			===  9
0 0 0 0 0		end
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    long tmp,sum=0;
    while(cin >> tmp){ //取第一个数字
        sum = 0;
        while(true){
            sum += tmp;
            if( cin.get() == '\n' ) break;
            cin >> tmp;
        }
        cout << sum << endl;
    }
}

```

```
5
c d a bb e		== a bb c d e
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    string s;
    int space;
    cin >> space;
    vector<string> ans;

    while(true){
        cin >> s;
        ans.push_back(s);
        if(cin.get() == '\n') break;
    }
    sort(ans.begin(),ans.end());

    for(auto str:ans) cout << str << " " ;
    cout << endl;
}

```

```
a c bb		== a bb c
f dddd		==	dddd f
nowcoder	== nowcoder
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    string s;
    vector<string> ans;

    while(cin>>s){
        ans.push_back(s);
        if(cin.get() == '\n'){ //去空格或换行操作
            sort(ans.begin(),ans.end());
            for(auto str:ans) cout << str << " " ;
            cout << endl;
            ans.clear();
        }
    }
}

```

```
a,c,bb	== a,bb,c
f,dddd	== dddd,f
nowcoder	== nowcoder
```

```c++
#include<bits/stdc++.h>
using namespace std;

int main(){
    string s;
    vector<string> str;
    while (cin >> s) {
        stringstream ssr(s);//从s读入数据
        string temp;
        while (getline(ssr, temp, ','))//ssr流对象以，分割提取temp
            str.push_back(temp);
        if (cin.get() == '\n') {
            sort(str.begin(), str.end());

            for(int i = 0 ; i < str.size()-1; ++i) cout << str[i] << ",";
            cout << str[str.size()-1] << endl;
            str.clear();
        }
    }

    return 0;
}
```
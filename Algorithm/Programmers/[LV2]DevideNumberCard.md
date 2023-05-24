2023.05.24

# __[프로그래머스 LV2] 숫자 카드 나누기__

----

## __문제__

[숫자 카드 나누기](https://school.programmers.co.kr/learn/courses/30/lessons/135807)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <iostream>
using namespace std;

int GCD(int a,int b)
{
    if(b == 0) 
        return a;

    return GCD(b,a%b);
}
int solution(vector<int> arrayA, vector<int> arrayB) 
{
    int answer = 0;
    int cur = arrayA[0];

    for(int i = 1 ; i < arrayA.size();i++)
    {
        cur = GCD(cur, arrayA[i]);
        if(cur == 1) 
            break;
    }

    if(cur != 1)
    {
        int i;
        for(i = 0 ; i < arrayB.size() ;i++)
        {
            if(arrayB[i] % cur == 0) break;
        }
        if(i == arrayB.size()) 
            answer = cur;
    }

    cur = arrayB[0];
    for(int i = 1 ; i < arrayB.size() ;i++)
    {
        cur = GCD(cur, arrayB[i]);

        if(cur == 1) 
            break;
    }

    if(cur != 1)
    {
        int i;
        for(i = 0 ; i < arrayA.size() ;i++)
        {
            if(arrayA[i] % cur == 0) 
                break;
        }
        if(i == arrayA.size())
             answer = max(cur,answer);
    }
    return answer;
}
```

최대공약수를 구하여 나머지를 비교하는 방법으로 풀이했습니다.


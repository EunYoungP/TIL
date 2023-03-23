[2023.03.03](#나의-풀이참고)

[2023.03.18 재풀이](#_재풀이)

[2023.03.23 재풀이](#재풀이2)

# __[프로그래머스 LV3] N으로 표현__

DP

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/222695643-23ee675d-a20a-49e6-bdb5-936b7b638a11.PNG"></img>
<BR><BR>


## __나의 풀이__(참고)
```c++
#include <string>
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

// 본인을 조합하여 만드는 경우의수를 계산하여 추가해줍니다. (e.g. N 이 5라면, 5/55/555/5555 등을 추가)
int MakeSelfComb(int N, int count)
{
    int temp = 0;
    for(int i = 0; i < count; i++)
    {
        // if( N ==4) count 1 : 4, count 2 : 44, count 3 : 444
        // N , N + 10^1, N + 10^2
        temp += N * pow(10, i);
    }
    return temp;
}

int solution(int N, int number) {
    int answer = 0;
    
    // DP[n] 은, N을 n번 사용하는 경우의 수를 의미합니다.
    // N이 8보다 크면 -1을 리턴하기 때문에 1 ~ 8 까지만 사용합니다.
    vector<int> DP[9];
    
    // N을 1번만 사용할 경우는 N 본인인 경우밖에 없기 때문에 DP[1] = 1이 됩니다.
    // N을 2번 사용할 수 있는 경우의 수는, NN과 N을 1번만 사용하는 경우들을 조합하는 경우입니다. (e.g. 5/5, 5*5, 5+5)
    // N을 3번 사용할 수 있는 경우의 수는, NNN과 N을 1번 사용하는 경우와 2번 사용하는 경우들을 조합하는 경우입니다. (e.g. 5+5 / 5, 5*5 / 5, 5/5 + 5)
    
    // N 자신을 조합해서 만들 수 있는 경우를 함수로 따로 추가해줍니다.
    DP[1].push_back(MakeSelfComb(N, 1));
    for(int i = 0; i < DP[1].size(); i++)
    {
        if(DP[1][i] == number)
            return 1;
    }
    
    // 8번 초과하여 사용하는 경우 -1을 반환하기때문에 그 전까지만 검사합니다.
    for(int i = 2; i <= 8; i++)
    {
        DP[i].push_back(MakeSelfComb(N, i));
        for(int k = 1; k < i; k++)
        {
            for(auto& a : DP[k])
            {
                if(a == 0)continue;
                for(auto& b : DP[i-k])
                {
                    if(b == 0)continue;
                    
                    DP[i].push_back(a + b);
                    DP[i].push_back(a * b);
                    DP[i].push_back(a / b);
                    DP[i].push_back(a - b);
                }
            }
        }
        for(int l = 0; l < DP[i].size(); l++)
        {
            if(DP[i][l]==number)
                return i;
        }
    }
    return -1;
}
```

DP[K]의 경우의 수를 메모해 두고 사용해야겠다는 생각을 했지만 DP로 구현해내는데 어려움을 겪고 구글링의 도움을 받았습니다..

벡터 DP[K]는 N을 K번 사용하여 조합할 수 있는 경우의 수를 배열로 만들어 가지고 있는 벡터입니다.

예를들어 K가 3일 경우, 즉 N을 3번 사용하여 조합할 수 있는 모든 경우의 수는

- N이 1번 사용되는 경우(DP[1]) N이 2번 사용되는 경우(DP[2])를 연산한 경우,

- N이 2번 사용되는 경우(DP[2]) N이 1번 사용되는 경우(DP[1])를 연산한 경우,

- 그리고 N을 K번 있는 그대로 조합한 경우(e.g. N, NN, NNN, NNNN 등) 로 나눌 수 있습니다.

이 예시에서 볼 수 있듯 K번 조합되는 경우의 수는 DP[i] 와 DP[K-i]을 조합하여 나올 수 있는 경우의 수라고 할 수 있습니다.<Br><Br>


## __재풀이_
```c++
#include <string>
#include <vector>
#include <cmath>
#include <iostream>

using namespace std;

int MakeSelfRepeat(int N, int count)
{
    int temp =0;
    for(int i = 0; i < count; i++)
    {
        temp += N * pow(10, i);
    }
    cout <<temp<<endl;
    return temp;
}

int solution(int N, int number) {
    int answer = -1;
    vector<vector<int>> n(9);
    
    //n[0].push_back(0);
    n[1].push_back(N);
    
    if(N == number)
        return 1;
    
    //5, 55 /6, 66/7, 77, 777
    for(int i = 2; i <= 8; i++)
    {
        n[i].push_back(MakeSelfRepeat(N, i));
        
        for(int j = 1; j < i; j++)
        {
            for(auto& a : n[j])
            {
                if(a == 0)continue;
                for(auto& b : n[i-j])
                {
                    if(b == 0)continue;
                    n[i].push_back(a + b);
                    n[i].push_back(a - b);
                    n[i].push_back(a * b);
                    n[i].push_back(a / b);
                }
            }
        }
        
        for(int k = 0; k < n[i].size(); k++)
        {
            if(n[i][k] == number)return i;
        }
    }
    return answer;
}
```

두 번째 풀이에서 DP 문제인 점을 바로 파악할 수 있었습니다.

하지만 반복문이 너무 많이 사용되다 보니 

이렇게 사용해도 되는지에 대한 고민에 빠져 답을 내기 어려웠습니다.

첫 번쨰 풀이에서는 배열 속 벡터 형식으로 DP리스트를 표현했지만 이번에는 이중 벡터로 표현했습니다.<br><Br>

## __재풀이2__

```c++
#include <string>
#include <vector>
#include <cmath>

using namespace std;

int GetDuplication(int N, int count)
{
    int temp = 0;
    for(int i = 0; i < count; i++)
    {
        temp += N * pow(10, i);
    }
    return temp;
}

int solution(int N, int number) {
    int answer = -1;
    vector<vector<int>> dp(9);
    
    dp[1].push_back(GetDuplication(N, 1));
    
    // for(auto& a : dp[1])
    // {
    //     if(a == number)
    //         return 1;
    // }
    
    for(int i= 0; i < dp[1].size(); i++)
    {
        if(dp[1][i] == number)
            return 1;
    }
    
    // i 개를 이용한 조합 = N-i 개의 조합 +/-/*// i 개의 조합
    for(int i = 2; i <= 8; i++)
    {
        dp[i].push_back(GetDuplication(N, i));
        
        for(int j = 1; j < i; j++)
        {
            for(auto& a : dp[j])
            {
                if(a == 0)
                    continue;
                for(auto& b : dp[i-j])
                {
                    if(b==0)
                        continue;
                    
                    dp[i].push_back(a * b);
                    dp[i].push_back(a / b);
                    dp[i].push_back(a + b);
                    dp[i].push_back(a - b);
                }
            }
        }
        // for(auto& u : dp[i])
        // {
        //     if(u == number)
        //         return i;
        // }
        for(int l = 0; l < dp[i].size(); l++)
        {
            if(dp[i][l] == number)return i;
        }
    }
    return answer;
}
```

같은 N 숫자가 겹치는 경우의 수를 구하는 함수를 따로 구현하는 것은 쉽게 할 수 있었습니다.

첫 번째, 두 번째 풀이와 달랐던 부분은 주석처리 된 범위기반 반복문을 사용해서 이중벡터를 처리한 점입니다.

주석 아래 반복문대신 주석 안의 내용을 사용해도 답은 같습니다.
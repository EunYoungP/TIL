2023.03.02

# __[프로그래머스 LV3] 거스름돈__

DP

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/222460639-6dac3aba-c5c1-445e-bef1-c6063f0a2e44.PNG"></img>
<BR><BR>


## __나의 풀이__(참고)
```c++
#include <string>
#include <vector>

using namespace std;
// DP 문제
int solution(int n, vector<int> money) {
    int answer = 0;
    
    vector<int> dp(n+1, 0);
    dp[0] =1;
    int memo = 0;
    
    for(int i =0; i < money.size(); i++)
    {
        for(int j = 1; j < n + 1; j++)
        {
            memo = 0;
            if(j >= money[i])
            {
                memo = j - money[i];
                dp[j] = dp[j] + dp[memo];
            }
            else
                continue;
        }
    }
    return dp[n];
}
```

DP 는 해당하는 인덱스의 거스름돈을 만들기위한 경우의 수를 모아놓은 벡터입니다.


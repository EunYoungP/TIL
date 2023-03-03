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
    
    // 동전 종류의 수
    for(int i =0; i < money.size(); i++)
    {
        // 거스름돈 - 특정 종류의 동전
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

dp[동전의 값] = dp[동전의 값] + dp[현재 거스름돈 - 동전의 값]

만약 거스름돈이 5 고, 현재 동전의 종류가 1 이라면,

5 는 1을 5번 뺼 수 있습니다. 1-1, 2-1, 3-1, 4-1, 5-1 총 다섯 경우입니다.

memo 변수에 위 다섯 값들이 차례대로 들어갑니다.

경우의 수는 아래와 같습니다.

* memo = 1 - 1

    dp[1] = dp[1] + dp[0];

* memo = 2 - 1

    dp[2] = dp[2] + dp[1];

* memo = 3 - 1

    dp[3] = dp[3] + dp[2];

* memo = 4 - 1

    dp[4] = dp[4] + dp[3];

* memo = 5 - 1

    dp[5] = dp[5] + dp[4];


top-down 형식으로 이루어진 점화식을 세울 수 있습니다.

[]